# Gas report

| G-N    |Issue|Instances|
|:------:|:----|:-------:|
| [G&#x2011;01] | Add `unchecked {}` in operations where the operands cannot over/underflow | 5 |
| [G&#x2011;02] | Use `constant` instead of `immutable` when possible | 3 |

Total: 8 instances over 2 issues

## [G-01] Add `unchecked {}` in operations where the operands cannot over/underflow

```solidity
File: /script/deploy/DeployBase.s.sol

66        address gobblerAddress = LibRLP.computeAddress(tx.origin, vm.getNonce(tx.origin) + 4);
67        address pageAddress = LibRLP.computeAddress(tx.origin, vm.getNonce(tx.origin) + 5);
```

```solidity
File: /src/ArtGobblers.sol

327        gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days);

412        gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold; // Assign id.

763            uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
```


## [G-02] Use `constant` instead of `immutable` when possible

```solidity
File: /script/deploy/DeployRinkeby.s.sol

 7    address public immutable coldWallet = 0x126620598A797e6D9d2C280b5dB91b46F27A8330;

 9    address public immutable root = 0x1D18077167c1177253555e45B4b5448B11E30b4b;

11    uint256 public immutable mintStart = 1656369768;
```
