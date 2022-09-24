## [L-01] Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

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

## [L-02] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
2022-09-artgobblers/src/ArtGobblers.sol::341 => if (mintStart > block.timestamp) revert MintStartPending();
2022-09-artgobblers/src/ArtGobblers.sol::399 => uint256 timeSinceStart = block.timestamp - mintStart;
2022-09-artgobblers/src/ArtGobblers.sol::455 => getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
2022-09-artgobblers/src/ArtGobblers.sol::513 => if (block.timestamp < nextRevealTimestamp) revert RequestTooEarly();
2022-09-artgobblers/src/ArtGobblers.sol::661 => getUserData[currentIdOwner].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/ArtGobblers.sol::763 => uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
2022-09-artgobblers/src/ArtGobblers.sol::826 => getUserData[user].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/ArtGobblers.sol::904 => getUserData[from].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/ArtGobblers.sol::911 => getUserData[to].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/Pages.sol::222 => uint256 timeSinceStart = block.timestamp - mintStart;
```

## [L-03] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
2022-09-artgobblers/src/ArtGobblers.sol::347 => if (!MerkleProofLib.verify(proof, merkleRoot, keccak256(abi.encodePacked(msg.sender)))) revert InvalidProof();
```

## [L-04] Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-09-artgobblers/src/ArtGobblers.sol::748 => : ERC721(nft).transferFrom(msg.sender, address(this), id);
2022-09-artgobblers/src/utils/GobblerReserve.sol::38 => artGobblers.transferFrom(address(this), to, ids[i]);
```

## [L-05] Missing Zero address checks

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters.
Impact: Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

```
2022-09-artgobblers/src/ArtGobblers.sol::314 => goo = _goo;
2022-09-artgobblers/src/ArtGobblers.sol::315 => pages = _pages;
2022-09-artgobblers/src/ArtGobblers.sol::316 => team = _team;
2022-09-artgobblers/src/ArtGobblers.sol::317 => community = _community;
2022-09-artgobblers/src/ArtGobblers.sol::318 => randProvider = _randProvider;
```

## [L-06] _safeMint() should be used rather than _mint() wherever possible

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::157 => function _mint(address to, uint256 id) internal virtual {
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::194 => _mint(to, id);
2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol::209 => _mint(to, id);
2022-09-artgobblers/src/ArtGobblers.sol::356 => _mint(msg.sender, gobblerId);
2022-09-artgobblers/src/ArtGobblers.sol::389 => _mint(msg.sender, gobblerId);
2022-09-artgobblers/src/ArtGobblers.sol::469 => _mint(msg.sender, gobblerId);
2022-09-artgobblers/src/Goo.sol::102 => _mint(to, amount);
2022-09-artgobblers/src/Pages.sol::211 => _mint(msg.sender, pageId);
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

## [N-01] Use a more recent version of solidity

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

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

## [N-02] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

```
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::11 => r := mul(x, 1000000000000000000)
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::21 => r := div(mul(x, 1000000000000000000), 86400)
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::31 => r := div(mul(x, 86400), 1000000000000000000)
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::39 => r := sdiv(mul(x, y), 1000000000000000000)
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::48 => r := sdiv(mul(x, 1000000000000000000), y)
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::63 => r := sdiv(r, 1000000000000000000)
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::70 => r := mul(x, 1000000000000000000)
2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol::73 => if iszero(and(iszero(iszero(y)), eq(sdiv(r, 1000000000000000000), x))) {
2022-09-artgobblers/src/ArtGobblers.sol::112 => uint256 public constant MAX_SUPPLY = 10000;
```

## [N-03] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
2022-09-artgobblers/src/ArtGobblers.sol::236 => event RandomnessFulfilled(uint256 randomness);
2022-09-artgobblers/src/utils/rand/RandProvider.sol::13 => event RandomBytesRequested(bytes32 requestId);
2022-09-artgobblers/src/utils/rand/RandProvider.sol::14 => event RandomBytesReturned(bytes32 requestId, uint256 randomness);
```

## [N-04] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public.

```
2022-09-artgobblers/src/ArtGobblers.sol::693 => function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {
2022-09-artgobblers/src/Pages.sol::265 => function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::55 => function ownerOf(uint256 id) public view virtual returns (address owner) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::73 => function uri(uint256 id) public view virtual returns (string memory);
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::79 => function setApprovalForAll(address operator, bool approved) public virtual {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::124 => function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {
```

## [N-05] Adding a return statement when the function defines a named return variable, is redundant

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
