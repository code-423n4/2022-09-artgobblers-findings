# 2022-09-ARTGOBBLERS
## Gas Optimizations Report

### The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.
This change would save up to 6 gas per instance/loop.

_There are **2** instances of this issue:_

```solidity
File: src/Pages.sol

251:  for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/GobblerReserve.sol

37:   for (uint256 i = 0; i < ids.length; i++) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol

### State variables should be cached in stack variables rather than re-reading them.
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **3** instances of this issue:_

```solidity
File: script/deploy/DeployBase.s.sol

      /// @audit Cache `teamColdWallet`. Used 3 times in `run`
70:   teamReserve = new GobblerReserve(ArtGobblers(gobblerAddress), teamColdWallet);
71:   communityReserve = new GobblerReserve(ArtGobblers(gobblerAddress), teamColdWallet);
102:  pages = new Pages(mintStart, goo, teamColdWallet, artGobblers, pagesBaseUri);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol

```solidity
File: src/ArtGobblers.sol

      /// @audit Cache `LEGENDARY_AUCTION_INTERVAL`. Used 3 times in `legendaryGobblerPrice`
487:  uint256 numMintedAtStart = (numSold + 1) * LEGENDARY_AUCTION_INTERVAL;
497:  if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;
499:  else return FixedPointMathLib.unsafeDivUp(startPrice * (LEGENDARY_AUCTION_INTERVAL - numMintedSinceStart), LEGENDARY_AUCTION_INTERVAL);

      /// @audit Cache `gobblerRevealsData`. Used 4 times in `revealGobblers`
577:  uint256 randomSeed = gobblerRevealsData.randomSeed;
579:  uint256 lastRevealedId = gobblerRevealsData.lastRevealedId;
581:  uint256 totalRemainingToBeRevealed = gobblerRevealsData.toBeRevealed;
584:  if (gobblerRevealsData.waitingForSeed) revert SeedPending();
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

### It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied
Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **7** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

432:  for (uint256 i = 0; i < cost; ++i) {

592:  for (uint256 i = 0; i < numGobblers; ++i) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

251:  for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/GobblerReserve.sol

37:   for (uint256 i = 0; i < ids.length; i++) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

114:  for (uint256 i = 0; i < owners.length; ++i) {

173:  for (uint256 i = 0; i < amount; ++i) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

```solidity
File: src/utils/token/GobblersERC721.sol

186:  for (uint256 i = 0; i < amount; ++i) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

### Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_There are **11** instances of this issue:_

```solidity
File: script/deploy/DeployRinkeby.s.sol

13:   string public constant gobblerBaseUri = "https:

14:   string public constant gobblerUnrevealedUri = "https:

15:   string public constant pagesBaseUri = "https:
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployRinkeby.s.sol

```solidity
File: src/ArtGobblers.sol

112:  uint256 public constant MAX_SUPPLY = 10000;

115:  uint256 public constant MINTLIST_SUPPLY = 2000;

118:  uint256 public constant LEGENDARY_SUPPLY = 10;

122:  uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;

126:  uint256 public constant MAX_MINTABLE = MAX_SUPPLY

177:  uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;

180:  uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;

184:  uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

### Functions that are access-restricted from most users may be marked as `payable`
Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **4** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

560:  function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/utils/GobblerReserve.sol

34:   function withdraw(address to, uint256[] calldata ids) external onlyOwner {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol

```solidity
File: src/utils/token/GobblersERC721.sol

92:   function approve(address spender, uint256 id) external {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

```solidity
File: src/utils/token/PagesERC721.sol

82:   function approve(address spender, uint256 id) external {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol

### `++i`/`i++` should be `unchecked{++I}`/`unchecked{I++}` in `for`-loops
When an increment or any arithmetic operation is not possible to overflow it should be placed in `unchecked{}` block. \This is because of the default compiler overflow and underflow safety checks since Solidity version 0.8.0. \In for-loops it saves around 30-40 gas **per loop**

_There are **7** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

432:  for (uint256 i = 0; i < cost; ++i) {

592:  for (uint256 i = 0; i < numGobblers; ++i) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

251:  for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/GobblerReserve.sol

37:   for (uint256 i = 0; i < ids.length; i++) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

114:  for (uint256 i = 0; i < owners.length; ++i) {

173:  for (uint256 i = 0; i < amount; ++i) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

```solidity
File: src/utils/token/GobblersERC721.sol

186:  for (uint256 i = 0; i < amount; ++i) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables


_There are **2** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

844:  uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

244:  uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead
'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **19** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

159:  uint128 public numMintedFromGoo;

167:  uint128 public currentNonLegendaryId;

189:  uint128 startPrice;

191:  uint128 numSold;

204:  uint64 randomSeed;

206:  uint64 nextRevealTimestamp;

615:  uint64 swapIndex = getGobblerData[swapId].idx == 0

623:  uint64 currentIndex = getGobblerData[currentId].idx == 0

899:  uint32 emissionMultiple = getGobblerData[id].emissionMultiple;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

107:  uint128 public currentId;

114:  uint128 public numMintedForCommunity;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

48:   uint64 idx;

50:   uint32 emissionMultiple;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

```solidity
File: src/utils/token/GobblersERC721.sol

38:   uint64 idx;

40:   uint32 emissionMultiple;

49:   uint32 gobblersOwned;

51:   uint32 emissionMultiple;

53:   uint128 lastBalance;

55:   uint64 lastTimestamp;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

### Multiplication/division by a number thats a power of 2 should use bit shifting
`x * 2` is equivalent to `x << 1` and `x / 2` is the same as `x >> 1`. The `MUL` and `DIV` opcodes cost 5 gas, whereas `SHL` and `SHR` only cost 3 gas

_There are **1** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

462:  cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

### Use `calldata` instead of `memory` for function parameters
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **2** instances of this issue:_

```solidity
File: src/utils/token/GobblersERC1155B.sol

138:  bytes memory data

161:  bytes memory data
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`
In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **5** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

435:  if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);

462:  cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2

497:  if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;

695:  if (gobblerId <= gobblerRevealsData.lastRevealedId) {

702:  if (gobblerId <= currentNonLegendaryId) return UNREVEALED_URI;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

### Use `immutable` & `constant` for state variables that do not change their value


_There are **15** instances of this issue:_

```solidity
File: script/deploy/DeployBase.s.sol

25:   string private gobblerBaseUri;

26:   string private gobblerUnrevealedUri;

27:   string private pagesBaseUri;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol

```solidity
File: src/ArtGobblers.sol

136:  string public UNREVEALED_URI;

139:  string public BASE_URI;

170:  uint256 public numMintedForReserves;

195:  LegendaryGobblerAuctionData public legendaryGobblerAuctionData;

216:  GobblerRevealsData public gobblerRevealsData;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

96:   string public BASE_URI;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/token/GobblersERC721.sol

23:   string public name;

25:   string public symbol;

27:   function tokenURI(uint256 id) external view virtual returns (string memory);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

```solidity
File: src/utils/token/PagesERC721.sol

24:   string public name;

26:   string public symbol;

28:   function tokenURI(uint256 id) external view virtual returns (string memory);
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol

### Use custom errors rather than `revert()`/`require()` strings to save gas
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **24** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

437:  require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

696:  if (gobblerId == 0) revert("NOT_MINTED");

705:  if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");

711:  revert("NOT_MINTED");

885:  require(from == getGobblerData[id].owner, "WRONG_FROM");

887:  require(to != address(0), "INVALID_RECIPIENT");

889:  require(
890:      msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
891:      "NOT_AUTHORIZED"
892:  );
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

266:  if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

107:  require(owners.length == ids.length, "LENGTH_MISMATCH");

149:  require(
150:      ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
151:          ERC1155TokenReceiver.onERC1155Received.selector,
152:      "UNSAFE_RECIPIENT"
153:  );

185:  require(
186:      ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
187:          ERC1155TokenReceiver.onERC1155BatchReceived.selector,
188:      "UNSAFE_RECIPIENT"
189:  );
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

```solidity
File: src/utils/token/GobblersERC721.sol

62:   require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

66:   require(owner != address(0), "ZERO_ADDRESS");

95:   require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

121:  require(
122:      to.code.length == 0 ||
123:          ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
124:          ERC721TokenReceiver.onERC721Received.selector,
125:      "UNSAFE_RECIPIENT"
126:  );

137:  require(
138:      to.code.length == 0 ||
139:          ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
140:          ERC721TokenReceiver.onERC721Received.selector,
141:      "UNSAFE_RECIPIENT"
142:  );
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

```solidity
File: src/utils/token/PagesERC721.sol

55:   require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

59:   require(owner != address(0), "ZERO_ADDRESS");

85:   require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");

103:  require(from == _ownerOf[id], "WRONG_FROM");

105:  require(to != address(0), "INVALID_RECIPIENT");

107:  require(
108:      msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
109:      "NOT_AUTHORIZED"
110:  );

135:  require(
136:      ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
137:          ERC721TokenReceiver.onERC721Received.selector,
138:      "UNSAFE_RECIPIENT"
139:  );

151:  require(
152:      ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
153:          ERC721TokenReceiver.onERC721Received.selector,
154:      "UNSAFE_RECIPIENT"
155:  );
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol

### Use a more recent version of solidity
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

_There are **11** instances of this issue:_

```solidity
File: script/deploy/DeployBase.s.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol

```solidity
File: script/deploy/DeployRinkeby.s.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployRinkeby.s.sol

```solidity
File: src/ArtGobblers.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Goo.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol

```solidity
File: src/Pages.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/GobblerReserve.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol

```solidity
File: src/utils/rand/ChainlinkV1RandProvider.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol

```solidity
File: src/utils/rand/RandProvider.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/RandProvider.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

```solidity
File: src/utils/token/GobblersERC721.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

```solidity
File: src/utils/token/PagesERC721.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol
