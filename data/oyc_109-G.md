## [G-01] Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```
2022-09-artgobblers/src/ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {
2022-09-artgobblers/src/ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```

## [G-02] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

```
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
```

## [G-03] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

When dealing with unsigned integer types, comparisons with != 0 are cheaper then with > 0. This change saves 6 gas per instance

```
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::142 => require(x > 0, "UNDEFINED");
```

## [G-04] Use Shift Right/Left instead of Division/Multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```
2022-09-artgobblers/src/ArtGobblers.sol::449 => getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
2022-09-artgobblers/src/ArtGobblers.sol::462 => cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

## [G-05] Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

```
2022-09-artgobblers/lib/VRGDAs/src/LogisticVRGDA.sol::20 => int256 public immutable logisticLimit;
2022-09-artgobblers/lib/VRGDAs/src/LogisticVRGDA.sol::25 => int256 public immutable logisticLimitDoubled;
2022-09-artgobblers/lib/VRGDAs/src/VRGDA.sol::17 => int256 public immutable targetPrice;
2022-09-artgobblers/src/ArtGobblers.sol::92 => Goo public immutable goo;
2022-09-artgobblers/src/ArtGobblers.sol::95 => Pages public immutable pages;
2022-09-artgobblers/src/ArtGobblers.sol::98 => address public immutable team;
2022-09-artgobblers/src/ArtGobblers.sol::101 => address public immutable community;
2022-09-artgobblers/src/ArtGobblers.sol::146 => bytes32 public immutable merkleRoot;
2022-09-artgobblers/src/ArtGobblers.sol::156 => uint256 public immutable mintStart;
2022-09-artgobblers/src/Goo.sol::64 => address public immutable artGobblers;
2022-09-artgobblers/src/Goo.sol::67 => address public immutable pages;
2022-09-artgobblers/src/Pages.sol::86 => Goo public immutable goo;
2022-09-artgobblers/src/Pages.sol::89 => address public immutable community;
2022-09-artgobblers/src/Pages.sol::103 => uint256 public immutable mintStart;
2022-09-artgobblers/src/utils/GobblerReserve.sol::18 => ArtGobblers public immutable artGobblers;
2022-09-artgobblers/src/utils/rand/ChainlinkV1RandProvider.sol::20 => ArtGobblers public immutable artGobblers;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::34 => ArtGobblers public immutable artGobblers;
```

## [G-06] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
2022-09-artgobblers/lib/solmate/src/auth/Owned.sol::39 => function setOwner(address newOwner) public virtual onlyOwner {
2022-09-artgobblers/src/ArtGobblers.sol::560 => function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
2022-09-artgobblers/src/Goo.sol::101 => function mintForGobblers(address to, uint256 amount) external only(artGobblers) {
2022-09-artgobblers/src/Goo.sol::108 => function burnForGobblers(address from, uint256 amount) external only(artGobblers) {
2022-09-artgobblers/src/Goo.sol::115 => function burnForPages(address from, uint256 amount) external only(pages) {
2022-09-artgobblers/src/utils/GobblerReserve.sol::34 => function withdraw(address to, uint256[] calldata ids) external onlyOwner {
```

## [G-07] Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. 

```
2022-09-artgobblers/lib/solmate/src/utils/LibString.sol::30 => for { let temp := value } 1 {} {
2022-09-artgobblers/lib/solmate/src/utils/MerkleProofLib.sol::23 => for {} 1 {} {
```

## [G-08] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
2022-09-artgobblers/src/ArtGobblers.sol::159 => uint128 public numMintedFromGoo;
2022-09-artgobblers/src/ArtGobblers.sol::167 => uint128 public currentNonLegendaryId;
2022-09-artgobblers/src/ArtGobblers.sol::189 => uint128 startPrice;
2022-09-artgobblers/src/ArtGobblers.sol::191 => uint128 numSold;
2022-09-artgobblers/src/ArtGobblers.sol::204 => uint64 randomSeed;
2022-09-artgobblers/src/ArtGobblers.sol::206 => uint64 nextRevealTimestamp;
2022-09-artgobblers/src/Pages.sol::107 => uint128 public currentId;
2022-09-artgobblers/src/Pages.sol::114 => uint128 public numMintedForCommunity;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::48 => uint64 idx;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::50 => uint32 emissionMultiple;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::38 => uint64 idx;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::40 => uint32 emissionMultiple;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::49 => uint32 gobblersOwned;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::51 => uint32 emissionMultiple;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::53 => uint128 lastBalance;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::55 => uint64 lastTimestamp;
```

## [G-09] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false instead

```
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::51 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/ArtGobblers.sol::149 => mapping(address => bool) public hasClaimedMintlistGobbler;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::37 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::77 => mapping(address => mapping(address => bool)) public isApprovedForAll;
```

## [G-10] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::174 => ids[i] = ++lastMintedId; // Increment id while setting.
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::166 => ++getUserData[to].gobblersOwned;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::187 => getGobblerData[++lastMintedId].owner = to;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::117 => _balanceOf[to]++;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::181 => _balanceOf[to]++;
```

## [G-11] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::203 => r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::205 => r += 600920179829731861736702779321621459595472258049074101567377883020018308;
2022-09-artgobblers/src/ArtGobblers.sol::439 => burnedMultipleTotal += getGobblerData[id].emissionMultiple;
2022-09-artgobblers/src/ArtGobblers.sol::456 => getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
2022-09-artgobblers/src/ArtGobblers.sol::464 => legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
2022-09-artgobblers/src/ArtGobblers.sol::662 => getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
2022-09-artgobblers/src/ArtGobblers.sol::844 => uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
2022-09-artgobblers/src/ArtGobblers.sol::905 => getUserData[from].emissionMultiple -= emissionMultiple;
2022-09-artgobblers/src/ArtGobblers.sol::906 => getUserData[from].gobblersOwned -= 1;
2022-09-artgobblers/src/ArtGobblers.sol::912 => getUserData[to].emissionMultiple += emissionMultiple;
2022-09-artgobblers/src/ArtGobblers.sol::913 => getUserData[to].gobblersOwned += 1;
2022-09-artgobblers/src/Pages.sol::244 => uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::184 => getUserData[to].gobblersOwned += uint32(amount);
```

## [G-12] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

```
2022-09-artgobblers/lib/VRGDAs/src/VRGDA.sol::32 => require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
2022-09-artgobblers/lib/solmate/src/auth/Owned.sol::20 => require(msg.sender == owner, "UNAUTHORIZED");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::36 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::40 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::69 => require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::87 => require(from == _ownerOf[id], "WRONG_FROM");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::89 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::158 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::160 => require(_ownerOf[id] == address(0), "ALREADY_MINTED");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::175 => require(owner != address(0), "NOT_MINTED");
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::142 => require(x > 0, "UNDEFINED");
2022-09-artgobblers/src/ArtGobblers.sol::437 => require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
2022-09-artgobblers/src/ArtGobblers.sol::885 => require(from == getGobblerData[id].owner, "WRONG_FROM");
2022-09-artgobblers/src/ArtGobblers.sol::887 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::107 => require(owners.length == ids.length, "LENGTH_MISMATCH");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::62 => require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::66 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::95 => require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::55 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::59 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::85 => require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::103 => require(from == _ownerOf[id], "WRONG_FROM");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::105 => require(to != address(0), "INVALID_RECIPIENT");
```

## [G-13] Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.
Consequences: each usage of a constant costs more gas on each access. Since these are not real constants, they can't be referenced from a real constant environment (e.g. from assembly, or from another library)

```
2022-09-artgobblers/src/ArtGobblers.sol::122 => uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
2022-09-artgobblers/src/ArtGobblers.sol::180 => uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
2022-09-artgobblers/src/ArtGobblers.sol::184 => uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```

## [G-14] Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

```
2022-09-artgobblers/lib/VRGDAs/src/LinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/VRGDAs/src/LogisticToLinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/VRGDAs/src/LogisticVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/VRGDAs/src/VRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/goo-issuance/src/LibGOO.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/solmate/src/auth/Owned.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/solmate/src/utils/LibString.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/solmate/src/utils/MerkleProofLib.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/ArtGobblers.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/Goo.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/Pages.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/GobblerReserve.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/rand/ChainlinkV1RandProvider.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/rand/RandProvider.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::2 => pragma solidity >=0.8.0;
```

## [G-15] Prefix increments cheaper than Postfix increments

++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)
Saves 5 gas PER LOOP

```
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
```

## [G-16] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.

```
2022-09-artgobblers/src/ArtGobblers.sol::693 => function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {
2022-09-artgobblers/src/Pages.sol::265 => function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
```

## [G-17] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::8 => function toWadUnsafe(uint256 x) pure returns (int256 r) {
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::18 => function toDaysWadUnsafe(uint256 x) pure returns (int256 r) {
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::28 => function fromDaysWadUnsafe(int256 x) pure returns (uint256 r) {
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::36 => function unsafeWadMul(int256 x, int256 y) pure returns (int256 r) {
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::45 => function unsafeWadDiv(int256 x, int256 y) pure returns (int256 r) {
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::52 => function wadMul(int256 x, int256 y) pure returns (int256 r) {
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::67 => function wadDiv(int256 x, int256 y) pure returns (int256 r) {
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::82 => function wadExp(int256 x) pure returns (int256 r) {
2022-09-artgobblers/src/utils/rand/ChainlinkV1RandProvider.sol::62 => function requestRandomBytes() external returns (bytes32 requestId) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::55 => function ownerOf(uint256 id) public view virtual returns (address owner) {
2022-09-artgobblers/src/utils/token/PagesERC721.sol::72 => function isApprovedForAll(address owner, address operator) public view returns (bool isApproved) {
```

## [G-18] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::33 => mapping(address => uint256) internal _balanceOf;
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::51 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/ArtGobblers.sol::149 => mapping(address => bool) public hasClaimedMintlistGobbler;
2022-09-artgobblers/src/ArtGobblers.sol::223 => mapping(uint256 => mapping(address => mapping(uint256 => uint256))) public getCopiesOfArtGobbledByGobbler;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::37 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::59 => mapping(address => UserData) public getUserData;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::77 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::52 => mapping(address => uint256) internal _balanceOf;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::70 => mapping(address => mapping(address => bool)) internal _isApprovedForAll;
```

## [G-19] Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, "zero address")
  revert(0x00, 0x20)
 }
}
```

instances:

```
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::36 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::40 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::89 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::158 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::175 => require(owner != address(0), "NOT_MINTED");
2022-09-artgobblers/src/ArtGobblers.sol::887 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::62 => require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::66 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::55 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::59 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::105 => require(to != address(0), "INVALID_RECIPIENT");
```

## [G-20] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::172 => function _burn(uint256 id) internal virtual {
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::193 => function _safeMint(address to, uint256 id) internal virtual {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::14 => function mulWadDown(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::18 => function mulWadUp(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::22 => function divWadDown(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::26 => function divWadUp(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::230 => function unsafeMod(uint256 x, uint256 y) internal pure returns (uint256 z) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::238 => function unsafeDiv(uint256 x, uint256 y) internal pure returns (uint256 r) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::246 => function unsafeDivUp(uint256 x, uint256 y) internal pure returns (uint256 z) {
2022-09-artgobblers/lib/solmate/src/utils/LibString.sol::8 => function toString(uint256 value) internal pure returns (string memory str) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::160 => function _mint(address to, uint256 id) internal {
2022-09-artgobblers/src/utils/token/PagesERC721.sol::173 => function _mint(address to, uint256 id) internal {
```

## [G-21] internal functions not called by the contract should be removed to save deployment gas

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

```
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::172 => function _burn(uint256 id) internal virtual {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::14 => function mulWadDown(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::18 => function mulWadUp(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::22 => function divWadDown(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::26 => function divWadUp(uint256 x, uint256 y) internal pure returns (uint256) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::230 => function unsafeMod(uint256 x, uint256 y) internal pure returns (uint256 z) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::238 => function unsafeDiv(uint256 x, uint256 y) internal pure returns (uint256 r) {
2022-09-artgobblers/lib/solmate/src/utils/FixedPointMathLib.sol::246 => function unsafeDivUp(uint256 x, uint256 y) internal pure returns (uint256 z) {
2022-09-artgobblers/lib/solmate/src/utils/LibString.sol::8 => function toString(uint256 value) internal pure returns (string memory str) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::160 => function _mint(address to, uint256 id) internal {
2022-09-artgobblers/src/utils/token/PagesERC721.sol::173 => function _mint(address to, uint256 id) internal {
```