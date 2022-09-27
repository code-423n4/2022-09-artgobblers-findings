### G-01 Use defualt value rather than overwriite variable with their default value.

Overriting varibles with defualt values with their default value will waste only gas and not necessary.

_There are **7** instances of this issue:_

File: ArtGobblers.sol lines 432, 592

```
for (uint256 i = 0; i < cost; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432

```
for (uint256 i = 0; i < numGobblers; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592

File: Pages.sol line 251

```
for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

File: GobblerReserve.sol line 37

```
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37

File: GobblersERC1155B lines 114,173

```
for (uint256 i = 0; i < owners.length; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114

```
for (uint256 i = 0; i < amount; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L173

File: GobblersERC721 line 186

```
for (uint256 i = 0; i < amount; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186

### G-02 In for loop use outside variable for array length

For loop written like this`for (uint256 i; i < array.length; ++i) {` will cost more gas than `for (uint256 i; i < _lengthOfArray; ++i) {` because for every iteration we use mload and memory_offset
that will cost about 6 gas

_There are **2** instances of this issue:_

File: GobblerReserve.sol: line 37

```
for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37

File GobblersERC1155B.sol line 114

```
for (uint256 i = 0; i < owners.length; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114

### G-03 If you use bit shifting will save some gas

Use of bit shifting operations are more cheap than normal multiplication/division operations. `MUL` and `DIV` costs 5 gas rather than `SHL` and `SHR` that costs 3 gas. You can use them where is possible

_There are **2** instances of this issue:_

File: ArtGobblers.sol lines 449,462

```
getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L449

```
2022-09-artgobblers/src/ArtGobblers.sol::462 => cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ?
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L462

### G-05 i++, i += 1 and i = i + 1 IN FOR LOOPS COSTS MORE GAS COMPARED TO ++i

++i will save about 5 gas for each iteration

_There are **2** instances of this issue:_

File: Pages.sol line 251

```
for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

File: GobblerReserve.sol line 37

```
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37

### G-06 Recomend to use `uint256(1)`/`uint256(2)` for true and false

If you don't use boolean for storage you will avoid Gwarmaccess (100 gas). Also boolean from true to false cost 20000 gas rather than uint256(2) to uint256(1) that cost less.

_There have **4** istance of this issues:_

```
mapping(address => mapping(address => bool)) public isApprovedForAll;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L77

```
mapping(address => bool) public hasClaimedMintlistGobbler;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L149

```
bool waitingForSeed;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L212

```
mapping(address => mapping(address => bool)) internal _isApprovedForAll;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L70

### G-07 Recomend to use one operator for comparison where is possible

If you use `>=` or `<=` this will cost more gas because in the EVM there is no implementation of Opcodes for `>=` and `<= and two operations are done.

_There have **4** istance of this issues:_

```
if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L435

```
if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L497

```
if (gobblerId <= gobblerRevealsData.lastRevealedId) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L695

```
if (gobblerId <= currentNonLegendaryId) return UNREVEALED_URI;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L702

### G-04 = + cost less than += for state variables

_There have **2** istance of this issues:_

```
burnedMultipleTotal += getGobblerData[id].emissionMultiple;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L439

```
legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L464

### G-03 Use `uncheck()` in for loops whenere overflow and undeflow is not possible

Using of `uncheck(i++)`/`uncheck(++i)` will save about 30 gas per iteration because compiler not save check everytime. This feature come from 0.8

_There are **7** instances of this issue:_

File: ArtGobblers.sol lines 432, 592

```
for (uint256 i = 0; i < cost; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432

```
for (uint256 i = 0; i < numGobblers; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592

File: Pages.sol line 251

```
for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

File: GobblerReserve.sol line 37

```
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37

File: GobblersERC1155B lines 114,173

```
for (uint256 i = 0; i < owners.length; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114

```
for (uint256 i = 0; i < amount; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L173

File: GobblersERC721 line 186

```
for (uint256 i = 0; i < amount; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186
