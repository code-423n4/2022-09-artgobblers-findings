## [L-01] CALLING `mintReservedGobblers` FUNCTION WITH `numGobblersEach` INPUT BEING 0 FOR MANY TIMES CAN CAUSE EVENT LOG POISONING
An attacker can call the following `mintReservedGobblers` function with the `numGobblersEach` input being 0 for many times to emit useless `ReservedGobblersMinted` events. This causes event log poisoning in which the potential monitor systems that consume the `ReservedGobblersMinted` event can be confused and spammed. To prevent this issue, please consider requiring that `numGobblersEach > 0` in this function before executing the current function body code.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L839-L858
```solidity
    function mintReservedGobblers(uint256 numGobblersEach) external returns (uint256 lastMintedGobblerId) {
        unchecked {
            // Optimistically increment numMintedForReserves, may be reverted below. Overflow in this
            // calculation is possible but numGobblersEach would have to be so large that it would cause the
            // loop in _batchMint to run out of gas quickly. Shift left by 1 is equivalent to multiplying by 2.
            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);

            // Ensure that after this mint gobblers minted to reserves won't comprise more than 20% of
            // the sum of the supply of goo minted gobblers and the supply of gobblers minted to reserves.
            if (newNumMintedForReserves > (numMintedFromGoo + newNumMintedForReserves) / 5) revert ReserveImbalance();
        }

        // Mint numGobblersEach gobblers to both the team and community reserve.
        lastMintedGobblerId = _batchMint(team, numGobblersEach, currentNonLegendaryId);
        lastMintedGobblerId = _batchMint(community, numGobblersEach, lastMintedGobblerId);

        currentNonLegendaryId = uint128(lastMintedGobblerId); // Set currentNonLegendaryId.

        emit ReservedGobblersMinted(msg.sender, lastMintedGobblerId, numGobblersEach);
    }
```

## [L-02] CALLING `mintCommunityPages` FUNCTION WITH `numPages` INPUT BEING 0 FOR MANY TIMES CAN CAUSE EVENT LOG POISONING
The following `mintCommunityPages` function can be called with the `numPages` input being 0 for many times to emit useless `CommunityPagesMinted` events. By poisoning the event logs, the malicious user confuses and spams the potential monitor systems that consume the `CommunityPagesMinted` event. To mitigate this risk, please consider requiring that `numPages > 0` in this function before executing the code in the current function body.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L239-L257
```solidity
    function mintCommunityPages(uint256 numPages) external returns (uint256 lastMintedPageId) {
        unchecked {
            // Optimistically increment numMintedForCommunity, may be reverted below.
            // Overflow in this calculation is possible but numPages would have to
            // be so large that it would cause the loop to run out of gas quickly.
            uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);

            // Ensure that after this mint pages minted to the community reserve won't comprise more than
            // 10% of the new total page supply. currentId is equivalent to the current total supply of pages.
            if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();

            // Mint the pages to the community reserve while updating lastMintedPageId.
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

            currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id.

            emit CommunityPagesMinted(msg.sender, lastMintedPageId, numPages);
        }
    }
```

## [L-03] INCORRECT COMMENT FOR `unsafeDivUp` FUNCTION
The comment for the following `unsafeDivUp` function states: `Add 1 to x * y if x % y > 0`. Yet, the code is adding 1 to x / y if  x % y > 0. Hence, this comment is incorrect. To avoid confusion, please update this comment to `Add 1 to x / y if x % y > 0`.

https://github.com/transmissions11/solmate/blob/main/src/utils/FixedPointMathLib.sol#L246-L252
```solidity
    function unsafeDivUp(uint256 x, uint256 y) internal pure returns (uint256 z) {
        assembly {
            // Add 1 to x * y if x % y > 0. Note this will
            // return 0 instead of reverting if y is zero.
            z := add(gt(mod(x, y), 0), div(x, y))
        }
    }
```

## [L-04] MISSING ZERO-ADDRESS CHECKS FOR CRITICAL ADDRESSES
To prevent unintended behaviors, critical address inputs should be checked against `address(0)`.

Please consider checking `_teamColdWallet`, `_vrfCoordinator`, and `_linkToken` in the following `constructor`.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L37-L59
```solidity
    constructor(
        address _teamColdWallet,
        bytes32 _merkleRoot,
        uint256 _mintStart,
        address _vrfCoordinator,
        address _linkToken,
        bytes32 _chainlinkKeyHash,
        uint256 _chainlinkFee,
        string memory _gobblerBaseUri,
        string memory _gobblerUnrevealedUri,
        string memory _pagesBaseUri
    ) {
        teamColdWallet = _teamColdWallet;
        ...
        vrfCoordinator = _vrfCoordinator;
        linkToken = _linkToken;
        ...
    }
```

Please consider checking `_team` and `_community` in the following `constructor`.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L287-L328
```solidity
    constructor(
        // Mint config:
        bytes32 _merkleRoot,
        uint256 _mintStart,
        // Addresses:
        Goo _goo,
        Pages _pages,
        address _team,
        address _community,
        RandProvider _randProvider,
        // URIs:
        string memory _baseUri,
        string memory _unrevealedUri
    )
        ...
    {
        ...
        team = _team;
        community = _community;
        ...
    }
```

Please consider checking `_artGobblers` and `_pages` in the following `constructor`.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L82-L85
```solidity
    constructor(address _artGobblers, address _pages) {
        artGobblers = _artGobblers;
        pages = _pages;
    }
```

Please consider checking `_community` in the following `constructor`.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L156-L184
```solidity
    constructor(
        // Mint config:
        uint256 _mintStart,
        // Addresses:
        Goo _goo,
        address _community,
        ArtGobblers _artGobblers,
        // URIs:
        string memory _baseUri
    )
        ...
    {
        ...

        community = _community;

        ...
    }
```

Please consider checking `_owner` in the following `constructor`.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L23-L25
```solidity
    constructor(ArtGobblers _artGobblers, address _owner) Owned(_owner) {
        ...
    }
```

Please consider checking `_vrfCoordinator` and `_linkToken` in the following `constructor`.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L48-L59
```solidity
    constructor(
        ArtGobblers _artGobblers,
        address _vrfCoordinator,
        address _linkToken,
        bytes32 _chainlinkKeyHash,
        uint256 _chainlinkFee
    ) VRFConsumerBase(_vrfCoordinator, _linkToken) {
        ...
    }
```

## [N-01] CONSTANTS CAN BE USED INSTEAD OF MAGIC NUMBERS
To improve readability and maintainability, constants can be used instead of magic numbers. Please consider replacing the magic numbers used in the following code with constants.

```solidity
lib\goo-issuance\src\LibGOO.sol
  38: (emissionMultiple * lastBalanceWad * 1e18).sqrt()   

lib\VRGDAs\src\LogisticVRGDA.sol
  44: logisticLimit = _maxSellable + 1e18;    
  47: logisticLimitDoubled = logisticLimit * 2e18;    
  62: return -unsafeWadDiv(wadLn(unsafeDiv(logisticLimitDoubled, sold + logisticLimit) - 1e18), timeScale);    

lib\VRGDAs\src\VRGDA.sol
  29: decayConstant = wadLn(1e18 - _priceDecayPercent);   

src\ArtGobblers.sol
  304: 69.42e18, // Target price.    
  305: 0.31e18, // Price decay percent.    
  308: 0.0023e18 // Time scale.    
  327: gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days);  
  535: gobblerRevealsData.nextRevealTimestamp = uint64(nextRevealTimestamp + 1 days);  
  632: uint256 newCurrentIdMultiple = 9; // For beyond 7963.   
  642: lt(swapIndex, 7964)),   
  643: lt(swapIndex, 5673)),   
  644: lt(swapIndex, 3055)   
```

## [N-02] UNDERSCORES IN NUMBER LITERALS OR SCIENTIFIC NOTATIONS WITH EXPONENTS CAN BE USED
Underscores in number literals or scientific notations with exponents can be used for improving readability and maintainability. Please consider using underscores or scientific notations with exponents for `1000000000000000000` in the following code.

```solidity
lib\solmate\src\utils\SignedWadMath.sol
  11: r := mul(x, 1000000000000000000)
  21: r := div(mul(x, 1000000000000000000), 86400)
  31: r := div(mul(x, 86400), 1000000000000000000)
  39: r := sdiv(mul(x, y), 1000000000000000000)
  48: r := sdiv(mul(x, 1000000000000000000), y)
  63: r := sdiv(r, 1000000000000000000)
  70: r := mul(x, 1000000000000000000)
  73: if iszero(and(iszero(iszero(y)), eq(sdiv(r, 1000000000000000000), x))) {
```

## [N-03] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. NatSpec comments are missing for the following functions. Please consider adding them.

```solidity
lib\solmate\src\utils\SignedWadMath.sol
  52: function wadMul(int256 x, int256 y) pure returns (int256 r) {   
  67: function wadDiv(int256 x, int256 y) pure returns (int256 r) {   
  82: function wadExp(int256 x) pure returns (int256 r) {   
  140: function wadLn(int256 x) pure returns (int256 r) {   

src\utils\token\GobblersERC721.sol
  61: function ownerOf(uint256 id) external view returns (address owner) {   
  65: function balanceOf(address owner) external view returns (uint256) {   
  92: function approve(address spender, uint256 id) external {   
  102: function setApprovalForAll(address operator, bool approved) external {   
  108: function transferFrom(   
  114: function safeTransferFrom(   
  129: function safeTransferFrom(   
  149: function supportsInterface(bytes4 interfaceId) external pure returns (bool) {   
  160: function _mint(address to, uint256 id) internal {   
  174: function _batchMint(    

src\utils\token\PagesERC721.sol
  54: function ownerOf(uint256 id) external view returns (address owner) {   
  58: function balanceOf(address owner) external view returns (uint256) {   
  72: function isApprovedForAll(address owner, address operator) public view returns (bool isApproved) {   
  82: function approve(address spender, uint256 id) external {   
  92: function setApprovalForAll(address operator, bool approved) external {   
  98: function transferFrom(   
  127: function safeTransferFrom(   
  142: function safeTransferFrom(   
  162: function supportsInterface(bytes4 interfaceId) external pure returns (bool) {   
  173: function _mint(address to, uint256 id) internal {   
```

## [N-04] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. @param or @return comments are missing for the following functions. Please consider completing NatSpec comments for them.

```solidity
lib\goo-issuance\src\LibGOO.sol
  17: function computeGOOBalance( 

lib\solmate\src\utils\SignedWadMath.sol
  8: function toWadUnsafe(uint256 x) pure returns (int256 r) {   
  18: function toDaysWadUnsafe(uint256 x) pure returns (int256 r) {   
  28: function fromDaysWadUnsafe(int256 x) pure returns (uint256 r) {   
  36: function unsafeWadMul(int256 x, int256 y) pure returns (int256 r) {   
  45: function unsafeWadDiv(int256 x, int256 y) pure returns (int256 r) {   
  212: function unsafeDiv(int256 x, int256 y) pure returns (int256 r) {   

src\ArtGobblers.sol
  509: function requestRandomSeed() external returns (bytes32) {   
  693: function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) { 
  757: function gooBalance(address user) public view returns (uint256) {   
  839: function mintReservedGobblers(uint256 numGobblersEach) external returns (uint256 lastMintedGobblerId) { 
  866: function getGobblerEmissionMultiple(uint256 gobblerId) external view returns (uint256) {    
  872: function getUserEmissionMultiple(address user) external view returns (uint256) {    

src\Pages.sol
  219: function pagePrice() public view returns (uint256) {    
  239: function mintCommunityPages(uint256 numPages) external returns (uint256 lastMintedPageId) { 
  265: function tokenURI(uint256 pageId) public view virtual override returns (string memory) { 

src\utils\rand\ChainlinkV1RandProvider.sol
  62: function requestRandomBytes() external returns (bytes32 requestId) {    
```

## [N-05] FLOATING PRAGMAS
It is a best practice to lock pragmas instead of using floating pragmas to ensure that contracts are tested and deployed with the intended compiler version. Accidentally deploying contracts with different compiler versions can lead to unexpected risks and undiscovered bugs. Please consider locking pragma for the following files.

```solidity
script\deploy\DeployBase.s.sol
  2: pragma solidity >=0.8.0;    

script\deploy\DeployRinkeby.s.sol
  2: pragma solidity >=0.8.0;    

src\ArtGobblers.sol
  2: pragma solidity >=0.8.0;    

src\Goo.sol
  2: pragma solidity >=0.8.0;    

src\Pages.sol
  2: pragma solidity >=0.8.0;    

src\utils\GobblerReserve.sol
  2: pragma solidity >=0.8.0;    

src\utils\rand\ChainlinkV1RandProvider.sol
  2: pragma solidity >=0.8.0;    

src\utils\rand\RandProvider.sol
  2: pragma solidity >=0.8.0;    

src\utils\token\GobblersERC721.sol
  2: pragma solidity >=0.8.0;    

src\utils\token\GobblersERC1155B.sol
  2: pragma solidity >=0.8.0;    

src\utils\token\PagesERC721.sol
  2: pragma solidity >=0.8.0;    

lib\solmate\src\auth\Owned.sol
  2: pragma solidity >=0.8.0;    

lib\VRGDAs\src\LogisticVRGDA.sol
  2: pragma solidity >=0.8.0;    

lib\VRGDAs\src\VRGDA.sol
  2: pragma solidity >=0.8.0;    
```