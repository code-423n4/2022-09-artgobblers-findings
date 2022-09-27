# Gas optimizations

## [G-01] Useless explicit initialization of variable to default value

Explicitly initializing a variable with its default value wastes gas.

### Files links :

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

### Findings :

```solidity
2022-09-artgobblers/src/ArtGobblers.sol:432: 	 for (uint256 i = 0; i < cost; ++i) {
2022-09-artgobblers/src/ArtGobblers.sol:592: 	 for (uint256 i = 0; i < numGobblers; ++i) {
2022-09-artgobblers/src/Pages.sol:251: 	 for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol:37: 	 for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:114: 	 for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:173: 	 for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol:186: 	 for (uint256 i = 0; i < amount; ++i) {
```

## [G-02] Cache Array Length Outside of Loop

Reading array length at each iteration of a loop uses additionnal gas every loop. Aditionnaly, `owners.length` is also used in a require statement of the same function in `GobblersERC1155B.sol` line 107, which make the caching of `owners.length` even more useful.

### Files links :

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

### Findings :

```solidity
2022-09-artgobblers/src/utils/GobblerReserve.sol:37: 	 for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:107:    require(owners.length == ids.length, "LENGTH_MISMATCH");
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:114: 	 for (uint256 i = 0; i < owners.length; ++i) {
```

## [G-03] `i++` instead of `++i`

`i++` costs 5 more gas per loop than `++i`.

### Files links :

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

### Findings :

```solidity
2022-09-artgobblers/src/Pages.sol:251: 	 for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol:37: 	 for (uint256 i = 0; i < ids.length; i++) {
```

## [G-04] Inline `unchecked{++i;}` could be used

The increment of a loop can be inlined and unchecked since there is no risk of overflow/underflow, which costs less gas.

### Files links :

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

### Findings :

```solidity
2022-09-artgobblers/src/Pages.sol:251: 	 for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol:37: 	 for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/ArtGobblers.sol:432:            for (uint256 i = 0; i < cost; ++i) {
2022-09-artgobblers/src/ArtGobblers.sol:592:            for (uint256 i = 0; i < numGobblers; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:173:            for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol:186:            for (uint256 i = 0; i < amount; ++i) {
```

## [G-05] Use custom errors instead of revert string

Custom errors available from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met).

### Files links :

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol

### Findings :

```solidity
2022-09-artgobblers/src/ArtGobblers.sol:437: 	 require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
2022-09-artgobblers/src/ArtGobblers.sol:885: 	 require(from == getGobblerData[id].owner, "WRONG_FROM");
2022-09-artgobblers/src/ArtGobblers.sol:887: 	 require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/src/ArtGobblers.sol:889: 	 require(
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:107: 	 require(owners.length == ids.length, "LENGTH_MISMATCH");
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:149: 	 require(
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:185: 	 require(
2022-09-artgobblers/src/utils/token/GobblersERC721.sol:62: 	 require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol:66: 	 require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol:95: 	 require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol:121: 	 require(
2022-09-artgobblers/src/utils/token/GobblersERC721.sol:137: 	 require(
2022-09-artgobblers/src/utils/token/PagesERC721.sol:55: 	 require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol:59: 	 require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/PagesERC721.sol:85: 	 require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol:103: 	 require(from == _ownerOf[id], "WRONG_FROM");
2022-09-artgobblers/src/utils/token/PagesERC721.sol:105: 	 require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/src/utils/token/PagesERC721.sol:107: 	 require(
2022-09-artgobblers/src/utils/token/PagesERC721.sol:135: 	 require(
2022-09-artgobblers/src/utils/token/PagesERC721.sol:151: 	 require(
```
