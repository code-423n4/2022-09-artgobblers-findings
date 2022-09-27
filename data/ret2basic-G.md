# Art Gobblers Contest Gas Optimization Report

## Summary

The following gas optimization issues were found during the code audit:

1. Use `calldata` instead of `memory` (5 instances)
2. Cache `<array>.length` (2 instances)
3. Use `unchecked{}` to suppress overflow/underflow check (7 instances)
4. Using `bool`s for storage incurs overhead (2 instances)
5. Don't initialize variables with default value (7 instances)
6. Use `++i`/`--i` instead of `i++`/`i--` (2 instances)
7. Use `private` instead of `public` for constants (8 instances)
8. Use custom errors instead of `revert()`/`require()` strings (16 instances)
9. Use shift right/left instead of division/multiplication if possible (2 instances)

Total 51 instances of 9 issues.

## 1. Use `calldata` instead of `memory` (5 instances)

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for loop to copy each index of the `calldata` to the `memory` index. This overhead can be optimized by using `calldata` directly.

```solidity
src/ArtGobblers.sol::693 => function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {

src/Pages.sol::265 => function tokenURI(uint256 pageId) public view virtual override returns (string memory) {

src/utils/token/GobblersERC1155B.sol::73 => function uri(uint256 id) public view virtual returns (string memory);

src/utils/token/GobblersERC721.sol::27 => function tokenURI(uint256 id) external view virtual returns (string memory);

src/utils/token/PagesERC721.sol::28 => function tokenURI(uint256 id) external view virtual returns (string memory);
```

## 2. Cache `<array>.length` (2 instances)

If `<array>.length` is used as for loop termination condition, then the `.length` method will be called in each iteration. Caching it in a local variable can save gas.

```solidity
src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
```

## 3. Use `unchecked{}` to suppress overflow/underflow check (7 instances)

Starting from version 0.8.0, Solidity does overflow/underflow checks by default. It is a good feature to prevent vulnerabilities but it has a significant overhead, especially when used in for loop. When using uint256/int256, it is extremely hard to trigger overflow, so it makes sense to skip these checks. To suppress the overflow/underflow checks, use `unchecked {}`. For increment situations, just use `unchecked {}` directly; for decrement situations, add a `require()` statement before decrementing to prevent underflow.

```solidity
src/ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {

src/ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {

src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {

src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {

src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```

## 4. Using `bool`s for storage incurs overhead (2 instances)

Use `uint256(1)` and `uint256(2)` for true/false. Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

```solidity
src/ArtGobblers.sol::212 => bool waitingForSeed;

src/ArtGobblers.sol::727 => bool isERC1155
```

## 5. Don't initialize variables with default value (7 instances)

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```solidity
src/ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {

src/ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {

src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {

src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {

src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```

## 6. Use `++i`/`--i` instead of `i++`/`i--` (2 instances)

Using `++i`/`--i` saves 6 gas per loop.

```solidity
src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
```

## 7. Use `private` instead of `public` for constants (8 instances)

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

```solidity
src/ArtGobblers.sol::112 => uint256 public constant MAX_SUPPLY = 10000;

src/ArtGobblers.sol::115 => uint256 public constant MINTLIST_SUPPLY = 2000;

src/ArtGobblers.sol::118 => uint256 public constant LEGENDARY_SUPPLY = 10;

src/ArtGobblers.sol::122 => uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;

src/ArtGobblers.sol::126 => uint256 public constant MAX_MINTABLE = MAX_SUPPLY

src/ArtGobblers.sol::177 => uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;

src/ArtGobblers.sol::180 => uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;

src/ArtGobblers.sol::184 => uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```

## 8. Use custom errors instead of `revert()`/`require()` strings (16 instances)

Using `require()`/`revert()` strings is expensive. Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors.

Custom errors decrease both deploy and runtime gas costs. Note that runtime gas cost is only relevant when the revert condition is met.

```solidity
src/ArtGobblers.sol::437 => require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

src/ArtGobblers.sol::696 => if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.

src/ArtGobblers.sol::705 => if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");

src/ArtGobblers.sol::711 => revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.

src/ArtGobblers.sol::885 => require(from == getGobblerData[id].owner, "WRONG_FROM");

src/ArtGobblers.sol::887 => require(to != address(0), "INVALID_RECIPIENT");

src/Pages.sol::266 => if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

src/utils/token/GobblersERC1155B.sol::107 => require(owners.length == ids.length, "LENGTH_MISMATCH");

src/utils/token/GobblersERC721.sol::62 => require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

src/utils/token/GobblersERC721.sol::66 => require(owner != address(0), "ZERO_ADDRESS");

src/utils/token/GobblersERC721.sol::95 => require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

src/utils/token/PagesERC721.sol::55 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

src/utils/token/PagesERC721.sol::59 => require(owner != address(0), "ZERO_ADDRESS");

src/utils/token/PagesERC721.sol::85 => require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");

src/utils/token/PagesERC721.sol::103 => require(from == _ownerOf[id], "WRONG_FROM");

src/utils/token/PagesERC721.sol::105 => require(to != address(0), "INVALID_RECIPIENT");
```

## 9. Use shift right/left instead of division/multiplication if possible (2 instances)

A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left. While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```solidity
src/ArtGobblers.sol::449 => getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);

src/ArtGobblers.sol::462 => cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```
