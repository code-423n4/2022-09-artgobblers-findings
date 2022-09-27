# My Findings :
## Saves Gas using these implementation
1.  `<array>.length` should not be looked up in every loop of a `for`-loop
2. Use custom errors rather than revert()/require() strings to save gas
3. It costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied
4.  `++i` costs less gas than `i++`, especially when it’s used in `for`-loops (`--i/i--` too)
5. Using `private` rather than `public` for constants, saves gas
6. Empty blocks should be removed or emit something
7. Use a more Recent version of Solidity
8. Functions guaranteed to `revert` when called by normal users can be marked payable
9. Using `bools` for storage incurs overhead
10. Multiplication/division by two should use bit shifting
11.++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops


# G-1 `<array>.length` should not be looked up in every loop of a `for`-loop

## Description

The overheads outlined below are PER LOOP, excluding the first loop

1. storage arrays incur a Gwarmaccess (100 gas)
2. memory arrays use `MLOAD` (3 gas)
3. calldata arrays use `CALLDATALOAD` (3 gas)

Caching the length changes each of these to a `DUP<N>` (3 gas), and gets rid of the extra `DUP<N>` needed to store the stack offset.

### Bugs in files:

```
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {

```

## Recommendation:
Using a fixed variable or value is appreciated.

# G-2 Use custom errors rather than revert()/require() strings to save gas
## Description
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

### Bugs in files:

```
lib/VRGDAs/src/VRGDA.sol:32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
```

```
lib/solmate/src/utils/SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
```

```
lib/solmate/src/tokens/ERC721.sol:36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:40:        require(owner != address(0), "ZERO_ADDRESS");
lib/solmate/src/tokens/ERC721.sol:69:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
lib/solmate/src/tokens/ERC721.sol:87:        require(from == _ownerOf[id], "WRONG_FROM");
lib/solmate/src/tokens/ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:91:        require(
lib/solmate/src/tokens/ERC721.sol:118:        require(
lib/solmate/src/tokens/ERC721.sol:134:        require(
lib/solmate/src/tokens/ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:160:        require(_ownerOf[id] == address(0), "ALREADY_MINTED");
lib/solmate/src/tokens/ERC721.sol:175:        require(owner != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:196:        require(
lib/solmate/src/tokens/ERC721.sol:211:        require(
```

```
lib/solmate/src/auth/Owned.sol:20:        require(msg.sender == owner, "UNAUTHORIZED");
```

```
src/ArtGobblers.sol:437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
src/ArtGobblers.sol:885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
src/ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
src/ArtGobblers.sol:889:        require(
```

```
src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
src/utils/token/GobblersERC721.sol:121:        require(
src/utils/token/GobblersERC721.sol:137:        require(
```

```
src/utils/token/GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
src/utils/token/GobblersERC1155B.sol:149:            require(
src/utils/token/GobblersERC1155B.sol:185:            require(
```

```
src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
src/utils/token/PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
```

### [Reference](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth)
----

# G-03 It costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied

### Description
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for `(uint256 i = 0; i < numIterations; ++i)` { should be replaced with for `(uint256 i; i < numIterations; ++i) {`

### Bugs in Files


```
src/ArtGobblers.sol:432:            for (uint256 i = 0; i < cost; ++i) {
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/token/GobblersERC721.sol:186:            for (uint256 i = 0; i < amount; ++i) {
src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
src/utils/token/GobblersERC1155B.sol:173:            for (uint256 i = 0; i < amount; ++i) {
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

```

# G-04  `++i` costs less gas than `i++`, especially when it’s used in `for`-loops (`--i/i--` too)

**Saves 6 gas PER LOOP**

```
src/utils/token/PagesERC721.sol:117:            _balanceOf[to]++;
src/utils/token/PagesERC721.sol:181:            _balanceOf[to]++;
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) 
lib/solmate/src/tokens/ERC721.sol:101:            _balanceOf[to]++;
lib/solmate/src/tokens/ERC721.sol:164:            _balanceOf[to]++;

```

# G-05 Using `private` rather than `public` for constants, saves gas
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

```

src/ArtGobblers.sol:112:    uint256 public constant MAX_SUPPLY = 10000;
src/ArtGobblers.sol:115:    uint256 public constant MINTLIST_SUPPLY = 2000;
src/ArtGobblers.sol:118:    uint256 public constant LEGENDARY_SUPPLY = 10;
src/ArtGobblers.sol:122:    uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
src/ArtGobblers.sol:126:    uint256 public constant MAX_MINTABLE = MAX_SUPPLY
src/ArtGobblers.sol:177:    uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
src/ArtGobblers.sol:180:    uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
src/ArtGobblers.sol:184:    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```

# G-06 Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty `if`-statement block to avoid doing subsequent checks in the `else-if/else` conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified `(if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})`

### Bugs in files

```
script/deploy/DeployRinkeby.s.sol:40:    {}
```

# G-07 Use a more Recent version of Solidity

### Description

Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

```
lib/goo-issuance/src/LibGOO.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/VRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LinearVRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LogisticVRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LogisticToLinearVRGDA.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/SignedWadMath.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/MerkleProofLib.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/LibString.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/FixedPointMathLib.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/tokens/ERC721.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/auth/Owned.sol:2:pragma solidity >=0.8.0;
src/ArtGobblers.sol:2:pragma solidity >=0.8.0;
src/utils/GobblerReserve.sol:2:pragma solidity >=0.8.0;
src/utils/token/GobblersERC721.sol:2:pragma solidity >=0.8.0;
src/utils/token/GobblersERC1155B.sol:2:pragma solidity >=0.8.0;
src/utils/token/PagesERC721.sol:2:pragma solidity >=0.8.0;
src/utils/rand/ChainlinkV1RandProvider.sol:2:pragma solidity >=0.8.0;
src/Goo.sol:2:pragma solidity >=0.8.0;
src/Pages.sol:2:pragma solidity >=0.8.0;
script/deploy/DeployRinkeby.s.sol:2:pragma solidity >=0.8.0;
script/deploy/DeployBase.s.sol:2:pragma solidity >=0.8.0;
```

# G- 08 Functions guaranteed to `revert` when called by normal users can be marked payable

### Description
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

## Bugs in file
```
src/ArtGobblers.sol:560:    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
```

----
# G-09 Using `bools` for storage incurs overhead

### Description
Booleans are more expensive than uint256 or any type that takes up a full
word because each write operation emits an extra SLOAD to first read the
slot's contents, replace the bits taken up by the boolean, and then write
back. This is the compiler's defense against contract upgrades and
pointer aliasing, and it cannot be disabled.

### Bugs in files
```
src/ArtGobblers.sol:149:    mapping(address => bool) public hasClaimedMintlistGobbler;
```

# G-10 Multiplication/division by two should use bit shifting

### Description
`<x> * 2` is equivalent to `<x> << 1` and `<x> / 2` is the same as `<x> >> 1`. The `MUL` and `DIV` opcodes cost `5 gas`, whereas SHL and SHR only cost `3 gas`

### Bugs in files
```

src/ArtGobblers.sol:449:            getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
src/ArtGobblers.sol:452:            // emission multiple (not burnedMultipleTotal * 2) to account for the standard gobblers that
src/ArtGobblers.sol:460:            // New start price is the max of LEGENDARY_GOBBLER_INITIAL_START_PRICE and cost * 2.
src/ArtGobblers.sol:462:                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2

```

# G-11 ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)

## Bugs in files
```
src/ArtGobblers.sol:432:            for (uint256 i = 0; i < cost; ++i) {
src/ArtGobblers.sol:592:            for (uint256 i = 0; i < numGobblers; ++i) {
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/token/GobblersERC721.sol:186:            for (uint256 i = 0; i < amount; ++i) {
src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
src/utils/token/GobblersERC1155B.sol:173:            for (uint256 i = 0; i < amount; ++i) {
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
