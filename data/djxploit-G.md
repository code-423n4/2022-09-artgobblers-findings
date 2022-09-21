## Summary

|  | Issues | Instances |
| --- | --- | --- |
| 1 | Multiple address and uint mappings can be combined into a single mapping of an address to a struct and uint to a struct respectively, where appropriate | 2 |
| 2 | State variables only set in the constructor should be declared immutable | 3 |
| 3 | Avoid contract existence checks by using solidity version 0.8.10 or later | 11 |
| 4 | State variables should be cached in stack variables rather than re-reading them from storage | 1 |
| 5 | Multiple accesses of a mapping should use a local variable cache | 5 |
| 6 | <array>.length should not be looked up in every loop of a for-loop | 1 |
| 7 | Using bools for storage incurs overhead | 5 |
| 8 | It costs more gas to initialise variables to zero than to let the default of zero be applied | 3 |
| 9 | ++i costs less gas than i++, especially when it's used in for-loops | 2 |
| 10 | Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | 10 |
| 11 | Usage of >= or <= is cheaper than > or <  | 9 |


## Gas Optimisations

1. **Multiple `address` and `uint` mappings can be combined into a single mapping of an `address` to a struct and `uint` to a struct respectively, where appropriate**

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

*There are 2 instances of this issue:*
```
File: src/utils/token/GobblersERC721.sol

44:     mapping(uint256 => GobblerData) public getGobblerData;
...
75:     mapping(uint256 => address) public getApproved;
```
and 
```
File: src/utils/token/GobblersERC721.sol

59:     mapping(address => UserData) public getUserData;
...
77:     mapping(address => mapping(address => bool)) public isApprovedForAll;
```


2. **State variables only set in the constructor should be declared immutable**

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

*There are 3 instances of this issue:*
```
File: src/utils/token/GobblersERC721.sol

23:     string public name;
25:     string public symbol;
```
```
File: src/Pages.sol

96:    string public BASE_URI;
```

3. **Avoid contract existence checks by using solidity version 0.8.10 or later**

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (700 gas), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

*There are 11 instances of this issue:*
```
File: src/ArtGobblers.sol

541:        return randProvider.requestRandomBytes();
747:        ? ERC1155(nft).safeTransferFrom(msg.sender, address(this), id, 1, "")
748:        : ERC721(nft).transferFrom(msg.sender, address(this), id);
772:        goo.burnForGobblers(msg.sender, gooAmount);
786:       goo.mintForGobblers(msg.sender, gooAmount);
```
```
File: src/Goo.sol

205:       ? artGobblers.burnGooForPages(msg.sender, currentPrice)
206:       : goo.burnForPages(msg.sender, currentPrice);
```
```
File: src/utils/rand/ChainlinkV1RandProvider.sol

76:        artGobblers.acceptRandomSeed(requestId, randomness);
```
```
File: src/utils/GobblerReserve.sol 

38:        artGobblers.transferFrom(address(this), to, ids[i]);
```
```
File: src/utils/token/GobblersERC721.sol

123:       ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
139:       ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
```

4. **State variables should be cached in stack variables rather than re-reading them from storage**

Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*There is 1 instance of this issue:*

```
File: src/ArtGobblers.sol

// cache 'gooBalance(user)'
820:        uint256 updatedBalance = updateType == GooBalanceUpdateType.INCREASE
821:            ? gooBalance(user) + gooAmount
822:            : gooBalance(user) - gooAmount;
```

5. **Multiple accesses of a mapping should use a local variable cache**

Caching a mapping's value in a local storage variable when the value is accessed multiple times, saves ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

* There are 5 instances of this issue:*

```
File: src/ArtGobblers.sol

// cache `getUserData[msg.sender]`
454:            getUserData[msg.sender].lastBalance = uint128(gooBalance(msg.sender)); // Checkpoint balance.
455:            getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
456:            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
458:            getUserData[msg.sender].gobblersOwned = uint32(getUserData[msg.sender].gobblersOwned - cost + 1);

// cache `getUserData[currentIdOwner]`
660:                getUserData[currentIdOwner].lastBalance = uint128(gooBalance(currentIdOwner));
661:                getUserData[currentIdOwner].lastTimestamp = uint64(block.timestamp);
662:                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);

// cache `getUserData[user]`
761:            getUserData[user].emissionMultiple,
762:            getUserData[user].lastBalance,
763:            uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))

// cache `getUserData[from]`
903:            getUserData[from].lastBalance = uint128(gooBalance(from));
904:            getUserData[from].lastTimestamp = uint64(block.timestamp);
905:            getUserData[from].emissionMultiple -= emissionMultiple;
905:            getUserData[from].gobblersOwned -= 1;

// cache `getUserData[to]`
910             getUserData[to].lastBalance = uint128(gooBalance(to));
911            getUserData[to].lastTimestamp = uint64(block.timestamp);
912            getUserData[to].emissionMultiple += emissionMultiple;
913            getUserData[to].gobblersOwned += 1;
```

6. **<array>.length should not be looked up in every loop of a for-loop**

The overheads outlined below are PER LOOP, excluding the first loop
- storage arrays incur a Gwarmaccess (100 gas)
- memory arrays use MLOAD (3 gas)
- calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

*There are 1 instances of this issue:*

```
File: src/utils/GobblerReserve.sol

37:            for (uint256 i = 0; i < ids.length; i++) {
```

7. **Using bools for storage incurs overhead**

```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past

*There are 5 instance of this issue:*

```
File: src/ArtGobblers.sol

149:     mapping(address => bool) public hasClaimedMintlistGobbler;
212:     bool waitingForSeed;
```

```
File: src/utils/token/GobblersERC1155B.sol

37:      mapping(address => mapping(address => bool)) public isApprovedForAll;
```
```
File: src/utils/token/GobblersERC721.sol

77:      mapping(address => mapping(address => bool)) public isApprovedForAll;
```
```
File: src/utils/token/PagesERC721.sol

70:      mapping(address => mapping(address => bool)) internal _isApprovedForAll;
```

8. **It costs more gas to initialise variables to zero than to let the default of zero be applied**

*There are 3 instances of this issue:*

```
File: src/Pages.sol

251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
```
File: src/utils/GobblerReserve.sol

37:             for (uint256 i = 0; i < ids.length; i++) {
```
```
File: src/utils/token/GobblersERC721.sol

186:            for (uint256 i = 0; i < amount; ++i) {
```

9. **++i costs less gas than i++, especially when it's used in for-loops**

Saves 6 gas per loop

*There are 2 instance of this issue:*

```
File: src/Pages.sol

251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
```
File: src/utils/GobblerReserve.sol

37:             for (uint256 i = 0; i < ids.length; i++) {
```

10. **Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead**

```
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
```
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Use a larger size then downcast where needed

*There are 10 instances of this issue:*

```
File: src/Pages.sol

107:       uint128 public currentId;
114:       uint128 public numMintedForCommunity;
```
```
File: src/ArtGobblers.sol

159:         uint128 public numMintedFromGoo;
167:         uint128 public currentNonLegendaryId;
189:         uint128 startPrice;
191:         uint128 numSold;
204:        uint64 randomSeed;
206:        uint64 nextRevealTimestamp;
208:        uint56 lastRevealedId;
210:         uint56 toBeRevealed;
```

11. **Usage of `>=` or `<=` is cheaper than `>` or `<`**

*There are 9 instances of this issue:*

```
File: src/ArtGobblers.sol

341:          if (mintStart > block.timestamp) revert MintStartPending();
375:          if (currentPrice > maxPrice) revert PriceExceededMax(currentPrice);
415:          if (gobblerId > MAX_SUPPLY) revert NoRemainingLegendaryGobblers();
490:          if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);
587:          if (numGobblers > totalRemainingToBeRevealed) revert NotEnoughRemainingToBeRevealed(totalRemainingToBeRevealed);
848:          if (newNumMintedForReserves > (numMintedFromGoo + newNumMintedForReserves) / 5) revert ReserveImbalance();
```
```
File: src/Pages.sol

200:         if (currentPrice > maxPrice) revert PriceExceededMax(currentPrice);
248:         if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();
266:         if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```

