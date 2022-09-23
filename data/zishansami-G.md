## Gas Optimizations
 *** 


### G-01: pre-increment `++i/--i` costs less gas than post-increment `i++/i--`
Saves 6 gas per loop in a for loop

Total instances of this issue: 2

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
```solidity
src/Pages.sol

251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

```

instance #2
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
```solidity
src/utils/GobblerReserve.sol

37:            for (uint256 i = 0; i < ids.length; i++) {

```

 *** 


### G-02: `++i/i++` should be placed in unchecked blocks to save gas as it is impossible for them to overflow in for and while loops
Unchecked keyword is available in solidity version `0.8.0` or higher and can be applied to iterator variables to save gas. 
Saves more than `30 gas` per loop.

Total instances of this issue: 2

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186
```solidity
src/utils/token/GobblersERC721.sol

186:            for (uint256 i = 0; i < amount; ++i) {

```

instance #2
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
```solidity
src/Pages.sol

251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

```

 *** 


### G-03: Length of the array (`<array>.length`) need not be looked up in every iteration of a for-loop
Reading array length at each iteration of the loop takes total 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the `array.length` saves around `3 gas` per iteration.

Total instances of this issue: 2

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
```solidity
src/utils/token/GobblersERC1155B.sol

114:            for (uint256 i = 0; i < owners.length; ++i) {

```

instance #2
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
```solidity
src/utils/GobblerReserve.sol

37:            for (uint256 i = 0; i < ids.length; i++) {

```

 *** 


### G-04: `x += y` costs more gas than `x = x + y` for state variables

Total instances of this issue: 9

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L439
```solidity
src/ArtGobblers.sol

439:                burnedMultipleTotal += getGobblerData[id].emissionMultiple;

```

instance #2
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L662
```solidity
src/ArtGobblers.sol

662:                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);

```

instance #3
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L844
```solidity
src/ArtGobblers.sol

844:            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);

```

instance #4
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L912
```solidity
src/ArtGobblers.sol

912:            getUserData[to].emissionMultiple += emissionMultiple;

```

instance #5
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L913
```solidity
src/ArtGobblers.sol

913:            getUserData[to].gobblersOwned += 1;

```

instance #6
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/lib/solmate/src/utils/SignedWadMath.sol#L203
```solidity
lib/solmate/src/utils/SignedWadMath.sol

203:        r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;

```

instance #7
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/lib/solmate/src/utils/SignedWadMath.sol#L205
```solidity
lib/solmate/src/utils/SignedWadMath.sol

205:        r += 600920179829731861736702779321621459595472258049074101567377883020018308;

```

instance #8
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L184
```solidity
src/utils/token/GobblersERC721.sol

184:            getUserData[to].gobblersOwned += uint32(amount);

```

instance #9
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L244
```solidity
src/Pages.sol

244:            uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);

```

 *** 


### G-05: Multiplication or Division by two should use bit shifting
`<x> * 2` is the same as `<x> >> 1` and `<x> / 2` is the same as `<x> >> 1`.
MUL and DIV opcodes cost `2 gas` more than SHL and SHR.

Total instances of this issue: 2

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L449
```solidity
src/ArtGobblers.sol

449:            getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);

```

instance #2
Permalink: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L461-L463
```solidity
src/ArtGobblers.sol

461:            legendaryGobblerAuctionData.startPrice = uint120(
462:                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
463:            );

```

 *** 


### G-06: Adding `payable` to functions which are only meant to be called by specific actors like `onlyOwner` will save gas cost
Marking functions payable removes additional checks for whether a payment was provided, hence reducing gas cost

Total instances of this issue: 3

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L560
```solidity
src/ArtGobblers.sol

560:    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {

```

instance #2
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/lib/solmate/src/auth/Owned.sol#L39
```solidity
lib/solmate/src/auth/Owned.sol

39:    function setOwner(address newOwner) public virtual onlyOwner {

```

instance #3
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L34
```solidity
src/utils/GobblerReserve.sol

34:    function withdraw(address to, uint256[] calldata ids) external onlyOwner {

```

 *** 


### G-07: No need to initialize non-constant/non-immutable variables to zero
Since the default value is already zero, overwriting is not required.
Saves `8 gas` per instance.

Total instances of this issue: 7

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
```solidity
src/ArtGobblers.sol

432:            for (uint256 i = 0; i < cost; ++i) {

```

instance #2
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592
```solidity
src/ArtGobblers.sol

592:            for (uint256 i = 0; i < numGobblers; ++i) {

```

instance #3
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
```solidity
src/utils/token/GobblersERC1155B.sol

114:            for (uint256 i = 0; i < owners.length; ++i) {

```

instance #4
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173
```solidity
src/utils/token/GobblersERC1155B.sol

173:            for (uint256 i = 0; i < amount; ++i) {

```

instance #5
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186
```solidity
src/utils/token/GobblersERC721.sol

186:            for (uint256 i = 0; i < amount; ++i) {

```

instance #6
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
```solidity
src/Pages.sol

251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

```

instance #7
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
```solidity
src/utils/GobblerReserve.sol

37:            for (uint256 i = 0; i < ids.length; i++) {

```

 *** 


### G-08: Using uints/ints smaller than 256 bits increases overhead
Gas usage becomes higher with uint/int smaller than 256 bits because EVM operates on 32 bytes and uses additional operations to reduce the size from 32 bytes to the target size.

Total instances of this issue: 18

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L159
```solidity
src/ArtGobblers.sol

159:    uint128 public numMintedFromGoo;

```

instance #2
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L167
```solidity
src/ArtGobblers.sol

167:    uint128 public currentNonLegendaryId;

```

instance #3
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L189
```solidity
src/ArtGobblers.sol

189:        uint128 startPrice;

```

instance #4
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L191
```solidity
src/ArtGobblers.sol

191:        uint128 numSold;

```

instance #5
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L204
```solidity
src/ArtGobblers.sol

204:        uint64 randomSeed;

```

instance #6
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L206
```solidity
src/ArtGobblers.sol

206:        uint64 nextRevealTimestamp;

```

instance #7
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L208
```solidity
src/ArtGobblers.sol

208:        uint56 lastRevealedId;

```

instance #8
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L210
```solidity
src/ArtGobblers.sol

210:        uint56 toBeRevealed;

```

instance #9
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L48
```solidity
src/utils/token/GobblersERC1155B.sol

48:        uint64 idx;

```

instance #10
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L50
```solidity
src/utils/token/GobblersERC1155B.sol

50:        uint32 emissionMultiple;

```

instance #11
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L38
```solidity
src/utils/token/GobblersERC721.sol

38:        uint64 idx;

```

instance #12
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L40
```solidity
src/utils/token/GobblersERC721.sol

40:        uint32 emissionMultiple;

```

instance #13
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L49
```solidity
src/utils/token/GobblersERC721.sol

49:        uint32 gobblersOwned;

```

instance #14
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L51
```solidity
src/utils/token/GobblersERC721.sol

51:        uint32 emissionMultiple;

```

instance #15
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L53
```solidity
src/utils/token/GobblersERC721.sol

53:        uint128 lastBalance;

```

instance #16
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L55
```solidity
src/utils/token/GobblersERC721.sol

55:        uint64 lastTimestamp;

```

instance #17
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L107
```solidity
src/Pages.sol

107:    uint128 public currentId;

```

instance #18
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L114
```solidity
src/Pages.sol

114:    uint128 public numMintedForCommunity;

```

 *** 


### G-09: Using `!= 0` costs less gas than `> 0` in a require() statement
Saves 6 gas per instance for solidity version less than 0.8.12.

Total instances of this issue: 1

instance #1
Link: https://github.com/code-423n4/2022-09-artgobblers/blob/main/lib/solmate/src/utils/SignedWadMath.sol#L142
```solidity
lib/solmate/src/utils/SignedWadMath.sol

142:        require(x > 0, "UNDEFINED");

```

