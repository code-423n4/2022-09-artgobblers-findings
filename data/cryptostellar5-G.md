### G-01 IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (`0` for `uint`, `false` for `bool`, `address(0)` for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < numIterations; ++i) {` should be replaced with `for (uint256 i; i < numIterations; ++i) {`


https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
432: for (uint256 i = 0; i < cost; ++i) {
592: for (uint256 i = 0; i < numGobblers; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

```
114: for (uint256 i = 0; i < owners.length; ++i) {
173: for (uint256 i = 0; i < amount; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

```
37: for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

```
for (uint256 i = 0; i < amount; ++i) {
```

### G-02 ARRAY.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR- LOOP

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

```
37: for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

```
114: for (uint256 i = 0; i < owners.length; ++i) {
```

### G-03 USAGE OF UINTS or INTS SMALLER THAN 32 BYTES - 256 BITS INCURS OVERHEAD

> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

There are several instances across all the inscope smart contracts.

some of the examples include

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
uint128 public numMintedFromGoo;
uint128 public currentNonLegendaryId;
uint64 randomSeed;
uint64 nextRevealTimestamp;
uint56 lastRevealedId;
uint56 toBeRevealed;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

```
uint64 idx;
uint32 emissionMultiple;
uint32 gobblersOwned;
uint32 emissionMultiple;
uint128 lastBalance;
uint64 lastTimestamp;
```

### G-04 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
439:  burnedMultipleTotal += getGobblerData[id].emissionMultiple;
464: legendaryGobblerAuctionData.numSold += 1;
662: getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
844: uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
912: getUserData[to].emissionMultiple += emissionMultiple;
913: getUserData[to].gobblersOwned += 1;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
244: uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

```
184: getUserData[to].gobblersOwned += uint32(amount);
```


### G-05 UNCHECKING ARITHMETICS OPERATIONS THAT CANT UNDERFLOW or OVERFLOW


https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
180: uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
184: uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
327: gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days);
412: gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold;
708: if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold)
821: ? gooBalance(user) + gooAmount
822: : gooBalance(user) - gooAmount;

```



### G-06 USING BOOLS FOR STORAGE INCURS OVERHEAD

```
// Booleans are more expensive than uint256 or any type that takes up a full    // word because each write operation emits an extra SLOAD to first read the    // slot's contents, replace the bits taken up by the boolean, and then write    // back. This is the compiler's defense against contract upgrades and    // pointer aliasing, and it cannot be disabled.
```

[https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27)  
Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (**[100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)**) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past


https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
349: hasClaimedMintlistGobbler[msg.sender] = true;
521: gobblerRevealsData.waitingForSeed = true;
553: gobblerRevealsData.waitingForSeed = false;
```


### G-07 MULTIPLICATION or DIVISION BY TWO SHOULD USE BIT SHIFTING

`<x> * 2` is equivalent to `<x> << 1` and `<x> / 2` is the same as `<x> >> 1`. The `MUL` and `DIV` opcodes cost 5 gas, whereas `SHL` and `SHR` only cost 3 gas

```
449: getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);

462: cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```


### G-08 FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2), `DUP1`(3), `ISZERO`(3), `PUSH2`(3), `JUMPI`(10), `PUSH1`(3), `DUP1`(3), `REVERT`(0), `JUMPDEST`(1), `POP`(2), which costs an average of about **21 gas per call** to the function, in addition to the extra deployment cost

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol

```
101: function mintForGobblers(address to, uint256 amount) external only(artGobblers) {
108: function burnForGobblers(address from, uint256 amount) external only(artGobblers) {
115: function burnForPages(address from, uint256 amount) external only(pages) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

```
34: function withdraw(address to, uint256[] calldata ids) external onlyOwner {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
560: function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
```

### G-09 ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 (SAME FOR --I VS I-- OR I -= 1)

Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

-   `i += 1` is the most expensive form
-   `i++` costs 6 gas less than `i += 1`
-   `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

-   `i -= 1` is the most expensive form
-   `i--` costs 11 gas less than `i -= 1`
-   `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, 

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

```
37: for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
251: for (uint256 i = 0; i < numPages; i++)
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol

```
115: _balanceOf[from]--;
117: _balanceOf[to]++;
181: _balanceOf[to]++;
```


### G-10 USE CUSTOM ERRORS RATHER THAN REVERT() or REQUIRE() STRINGS TO SAVE GAS

There are several instances of this accross all in-scope contracts, here are a few examples.


https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

```
107 require(owners.length == ids.length, "LENGTH_MISMATCH");
186: require(
187:                 ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
188:                    ERC1155TokenReceiver.onERC1155BatchReceived.selector,
189:                 "UNSAFE_RECIPIENT"
            );
```