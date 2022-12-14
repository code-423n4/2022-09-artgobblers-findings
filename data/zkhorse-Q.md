## Low

### `ArtGobblers` and `Pages`: Token mints do not use safe transfers

Functions that mint tokens in `ArtGobblers` and `Pages` (`ArtGobblers#claimGobbler`, `ArtGobblers#mintFromGoo`, `ArtGobblers#mintLegendaryGobbler`, `Pages#mintFromGoo`, `Pages#mintCommunityPages`) do not check that the recipient address supports ERC721 token transfers. If this is a concern, consider using `_safeMint` to mint tokens.

## QA

### `ArtGobblers`: Zero goo amounts may be added/removed from balances

It's possible to call `addGoo` and `removeGoo` with a zero `gooAmount`:

[`ArtGobblers#L767-L787`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L767-L787)

```solidity
    /// @notice Add goo to your emission balance,
    /// burning the corresponding ERC20 balance.
    /// @param gooAmount The amount of goo to add.
    function addGoo(uint256 gooAmount) external {
        // Burn goo being added to gobbler.
        goo.burnForGobblers(msg.sender, gooAmount);

        // Increase msg.sender's virtual goo balance.
        updateUserGooBalance(msg.sender, gooAmount, GooBalanceUpdateType.INCREASE);
    }

    /// @notice Remove goo from your emission balance, and
    /// add the corresponding amount to your ERC20 balance.
    /// @param gooAmount The amount of goo to remove.
    function removeGoo(uint256 gooAmount) external {
        // Decrease msg.sender's virtual goo balance.
        updateUserGooBalance(msg.sender, gooAmount, GooBalanceUpdateType.DECREASE);

        // Mint the corresponding amount of ERC20 goo.
        goo.mintForGobblers(msg.sender, gooAmount);
    }
```

This will make an internal call to `updateUsergooBalance` and emit a `GooBalanceUpdated` event.

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L809-L829

```solidity
    /// @notice Update a user's virtual goo balance.
    /// @param user The user whose virtual goo balance we should update.
    /// @param gooAmount The amount of goo to update the user's virtual balance by.
    /// @param updateType Whether to increase or decrease the user's balance by gooAmount.
    function updateUserGooBalance(
        address user,
        uint256 gooAmount,
        GooBalanceUpdateType updateType
    ) internal {
        // Will revert due to underflow if we're decreasing by more than the user's current balance.
        // Don't need to do checked addition in the increase case, but we do it anyway for convenience.
        uint256 updatedBalance = updateType == GooBalanceUpdateType.INCREASE
            ? gooBalance(user) + gooAmount
            : gooBalance(user) - gooAmount;

        // Snapshot the user's new goo balance with the current timestamp.
        getUserData[user].lastBalance = uint128(updatedBalance);
        getUserData[user].lastTimestamp = uint64(block.timestamp);

        emit GooBalanceUpdated(user, updatedBalance);
    }
```

Since this wastes gas for the caller and emits a confusing event, consider checking for zero amounts in `addGoo` and `removeGoo`.

Additionally, we found that calling `addGoo(0)` repeatedly and frequently at intervals of less than 1 day can cause a user's goo balance to be lower than expected. We didn't see a clear path to an exploit here, since the error penalizes rather than rewards the caller, but it is unusual behavior that may deserve a closer look. See the test below, which passes with `TIMESTEP_OK` and fails with `TIMESTEP_FAIL`.

```solidity
    /// @notice make sure that actions that trigger balance snapshotting do not affect total balance.
    function testSnapshotDoesNotAffectBalanceFuzzed() public {
        //mint one gobbler for each user
        mintGobblerToAddress(users[0], 1);
        mintGobblerToAddress(users[1], 1);
        vm.warp(block.timestamp + 1 days);
        //give user initial goo balance
        vm.prank(address(gobblers));
        goo.mintForGobblers(users[0], 100);
        //reveal gobblers
        bytes32 requestId = gobblers.requestRandomSeed();
        uint256 randomness = 1022; // magic seed to ensure both gobblers have same multiplier
        vrfCoordinator.callBackWithRandomness(requestId, randomness, address(randProvider));
        gobblers.revealGobblers(2);
        //make sure both gobblers have same multiple, and same starting balance
        assertGt(gobblers.getUserEmissionMultiple(users[0]), 0);
        assertEq(gobblers.getUserEmissionMultiple(users[0]), gobblers.getUserEmissionMultiple(users[1]));

        assertEq(gobblers.gooBalance(users[0]), gobblers.gooBalance(users[1]));

        uint256 TIMESTEP_OK = 1 days;
        uint256 TIMESTEP_FAIL = 1 days - 1;
        unchecked {
            for (uint256 n = 0; n < 1000; n += 12) {
                vm.warp(block.timestamp + TIMESTEP_OK);
                vm.prank(users[0]);
                gobblers.addGoo(0);
                console.log("balance1 - balance0 =", gobblers.gooBalance(users[1]) - gobblers.gooBalance(users[0]));
            }
        }

        // make sure users have equal balance
        unchecked {
            assertEq(0,  gobblers.gooBalance(users[1]) - gobblers.gooBalance(users[0]));
        }
    }
```

### `ArtGobblers`: Gobbling ignores token approvals

The caller of `ArtGobblers#gobble` must be the _owner_ of the Gobbler being fed:

[`ArtGobblers#gobble`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L732-L734)

```solidity
        // The caller must own the gobbler they're feeding.
        if (owner != msg.sender) revert OwnerMismatch(owner);
```

However, this limits composability. Consider adopting semantics similar to `isApprovedForAll` to allow third parties to feed a Gobbler with owner approval.

### `DeployBase`: Double check wallet address

According to the Art Gobblers whitepaper,

> No Pages accrue directly to the team, but one in ten newly created pages do go to a vault to be distributed to artists in the community.

However, in `DeployBase.s.sol`, the `Pages` contract is deployed with `teamColdWallet` as the `community` argument:

[`DeployBase.s.sol#L101-L102`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L101-L102)

```solidity
        // Deploy pages contract.
        pages = new Pages(mintStart, goo, teamColdWallet, artGobblers, pagesBaseUri);
```

Double check whether you're passing the correct parameter here.

### `Pages` and `GobblerReserve`: Prefer preincrement

We have not submitted a separate gas report, but we consider it a sacred duty to point out the following two usages of `i++` that could instead use `++i`:

[`Pages.sol#L250-L253`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L250-L253)

```solidity
            // Mint the pages to the community reserve while updating lastMintedPageId.
            for (uint256 i = 0; i < numPages; i++) {
                _mint(community, ++lastMintedPageId);
            }
```

[`GobblerReserve.sol#L31-L41`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L31-L41)

```solidity
    /// @notice Withdraw gobblers from the reserve.
    /// @param to The address to transfer the gobblers to.
    /// @param ids The ids of the gobblers to transfer.
    function withdraw(address to, uint256[] calldata ids) external onlyOwner {
        // This is quite inefficient, but it's okay because this is not a hot path.
        unchecked {
            for (uint256 i = 0; i < ids.length; i++) {
                artGobblers.transferFrom(address(this), to, ids[i]);
            }
        }
    }
```
