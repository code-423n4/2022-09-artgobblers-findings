# QA
# Low
## Variable shadows another variable
###  Summary 
Name shadowing where two or more variables/functions share the same name could be confusing to developers and/or reviewers
###  Details 
Use of `owner` as local variable in `gobble()` shadows `Owned.owner`
###  Github Permalinks 
`ArtGobblers.gobble().owner` shadows `Owned.owner`
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L730
###  Mitigation
Replace `owner` variable in the function parameter to `_owner`, `gobble_owner` or a similar substitution

## Missing checks for address(0x0) when assigning values to `address` state or `immutable` variables 
### Summary
Zero address should be checked for state variables, immutable variables. A zero address can lead into problems.
### Github Permalinks
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/auth/Owned.sol#L29-L33
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/script/deploy/DeployBase.s.sol#L38-L42
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/src/Goo.sol#L83-L84
https://github.com/code-423n4/2022-09-artgobblers/blob/6e0df2e5e82b51856e451d028a44593ef18c74b1/src/Pages.sol#L181
### Mitigation
Check zero address before assigning or using it


## Missing checks for address(0x0) on `setOwner` function 
### Summary
Zero address should be checked for some function parameters. For example in functions like mints, withdrawals... 

A zero address can lead into serious problems as locking eth or correct functioning.

### Details
The `setOwner` uses an address parameter, this means it can be called from wherever, so using an incorrect address can be performed.

### Github Permalinks
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/auth/Owned.sol#L40
### Mitigation
Check zero address before assigning or using it

## block.timestamp used as time proxy 
### Summary
Risk of using block.timestamp for time should be considered. 
### Details
block.timestamp is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

This kind of issue may affect the code allowing or reverting the code before the expected deadline, modifying the normal functioning or reverting sometimes.
### References
SWC ID: 116

### Github Permalinks 
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L341
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L513
### Mitigation
- Consider the risk of using block.timestamp as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 
- Consider using an oracle for precision


## Incorrect shift
### Proof of concept
```
// Moduloing by 1 << 64 (2 ** 64) is equivalent to a uint64 cast.
                    randomSeed := mod(keccak256(0, 32), shl(64, 1))
```
### Github permalink
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L674
### Tools
Slither
### Recommendation
Swap the order of parameters.

# Informational
##  Use of magic values is confusing and risky
### Summary
Magic values are hardcoded numbers or string used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.
### Details
Values are hardcoded and would be more readable and maintainable if declared as a constant

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L304-L308
https://github.com/transmissions11/goo-issuance/blob/5fe1e7d8a0c42a97c2a95d0547209f28dcbedb0b/src/LibGOO.sol#L38
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L148-L150
https://github.com/transmissions11/solmate/blob/b8853da1373e0eae1aac75a8ae083c65fb54e4ed/src/utils/SignedWadMath.sol#L86-L119
https://github.com/transmissions11/solmate/blob/b8853da1373e0eae1aac75a8ae083c65fb54e4ed/src/utils/SignedWadMath.sol#L136
https://github.com/transmissions11/solmate/blob/b8853da1373e0eae1aac75a8ae083c65fb54e4ed/src/utils/SignedWadMath.sol#L162-L184
https://github.com/transmissions11/solmate/blob/b8853da1373e0eae1aac75a8ae083c65fb54e4ed/src/utils/SignedWadMath.sol#L201-L207
https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticVRGDA.sol#L44-L47
https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticVRGDA.sol#L62
https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/VRGDA.sol#L29
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/script/deploy/DeployBase.s.sol#L66-L67
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/script/deploy/DeployRinkeby.s.sol#L26-L32
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L327
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L535
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L632
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L848
https://github.com/code-423n4/2022-09-artgobblers/blob/6e0df2e5e82b51856e451d028a44593ef18c74b1/src/Pages.sol#L168-L174
https://github.com/code-423n4/2022-09-artgobblers/blob/6e0df2e5e82b51856e451d028a44593ef18c74b1/src/Pages.sol#L248
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/GobblersERC721.sol#L151-L153
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/src/utils/token/GobblersERC1155B.sol#L126-L128
### Mitigation
Replace magic hardcoded values with declared constants.

## Missing indexed event parameters
### Summary
Events without indexed event parameters make it harder and
inefficient for off-chain tools to analyze them.
### Details
Indexed parameters (“topics”) are searchable event parameters.
They are stored separately from unindexed event parameters in an efficient manner to allow for faster access. This is useful for efficient off-chain-analysis, but it is also more costly gas-wise.
 
### Github Permalinks
https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L236
    event RandomnessFulfilled(uint256 randomness);
https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/utils/rand/RandProvider.sol#L13
    event RandomBytesRequested(bytes32 requestId);

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/utils/rand/RandProvider.sol#L14
    event RandomBytesReturned(bytes32 requestId, uint256 randomness);

### Mitigation
Consider which event parameters could be particularly useful to off-chain tools and should be indexed.

## Naming convention of constants
### Summary
Constant naming convention is all upper case. 
### Details
Some constants are not using proper style.
Constant should be in UPPER_CASE_WITH_UNDERSCORES as per Solidity Style Guide. 
### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/script/deploy/DeployRinkeby.s.sol#L13-L15
### Mitigation
Rename the constant to uppercase style: CONSTANTS_WITH_UNDERSCORES. 

## Naming convention of state variable non constant
### Summary
Only constants are suggested to use style CONSTANTS_WITH_UNDERSCORES, other variables are suggested to use camelCase
### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/6e0df2e5e82b51856e451d028a44593ef18c74b1/src/Pages.sol#L96
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L136-L139
### Mitigation
Rename to camelCase

## Different versions of pragma
### Summary
Some of the contracts include an unlocked pragma, e.g., pragma solidity >=0.8.0. 

Locking the pragma helps ensure that contracts are not accidentally deployed using an old compiler version with unfixed bugs.

### Github Permalinks


### Mitigation
Lock pragmas to a specific Solidity version. 
Consider converting >= 0.8.0 into 0.8.13
Consider converting ^0.8.0 into 0.8.13

## Bad order of code
### Summary
Clearness of the code is important for the readability and maintainability.
As Solidity guidelines says about declaration order:
1.Type declarations
2.State variables
3.Events
4.Modifiers
5.Functions
Also, state variables order affects to gas in the same way as ordering structs for saving storage slots

### github permalink
- events, variables and functions disordered
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L11-L33

- modifier should be declared before constructor
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/src/Goo.sol#L81-L96

- events, variables and functions disordered
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/GobblersERC721.sol#L11-L77

- events, variables, constructor and functions disordered
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/PagesERC721.sol#L14-L70

### Mitigation
Follow solidity style guidelines https://docs.soliditylang.org/en/v0.8.15/style-guide.html

## Missing Natspec 
### Summary 
Missing Natspec and regular comments affect readability and maintainability of a codebase. 
### Details 
Contracts has partial or full lack of comments
### Github Permalinks 
#### Natspec @param
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L878-L918
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/auth/Owned.sol#L28-L44
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/src/utils/token/GobblersERC1155B.sol#L54-L194
https://github.com/transmissions11/solmate/blob/b8853da1373e0eae1aac75a8ae083c65fb54e4ed/src/utils/SignedWadMath.sol#L1-L217
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/GobblersERC721.sol#L1-L195
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/PagesERC721.sol#L1-L188
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/src/utils/rand/ChainlinkV1RandProvider.sol#L61-L78
https://github.com/transmissions11/solmate/blob/a13635d8220f56ea61f4ccd8aaf03335179ce540/src/utils/MerkleProofLib.sol#L8-L13
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L1-L231
https://github.com/transmissions11/solmate/blob/1d50fa00985c1d9671861fa6ac2a90a7816ca974/src/utils/LibString.sol#L8
https://github.com/transmissions11/solmate/blob/26572802743101f160f2d07556edfc162896115e/src/utils/FixedPointMathLib.sol#L1-L253
#### Natspec @return value
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L509
    function requestRandomSeed() external returns (bytes32) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L693
    function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L757
    function gooBalance(address user) public view returns (uint256) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L839
    function mintReservedGobblers(uint256 numGobblersEach) external returns (uint256 lastMintedGobblerId) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L866
    function getGobblerEmissionMultiple(uint256 gobblerId) external view returns (uint256) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L872
    function getUserEmissionMultiple(address user) external view returns (uint256) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L14
    function mulWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L18
    function mulWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L22
    function divWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L26
    function divWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L38
    ) internal pure returns (uint256 z) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L57
    ) internal pure returns (uint256 z) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L78
    ) internal pure returns (uint256 z) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L166
    function sqrt(uint256 x) internal pure returns (uint256 z) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L230
    function unsafeMod(uint256 x, uint256 y) internal pure returns (uint256 z) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L238
    function unsafeDiv(uint256 x, uint256 y) internal pure returns (uint256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L246
    function unsafeDivUp(uint256 x, uint256 y) internal pure returns (uint256 z) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L25
    function tokenURI(uint256 id) public view virtual returns (string memory);

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L35
    function ownerOf(uint256 id) public view virtual returns (address owner) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L39
    function balanceOf(address owner) public view virtual returns (uint256) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L146
    function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L228
    ) external virtual returns (bytes4) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L55
    function ownerOf(uint256 id) public view virtual returns (address owner) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L59
    function balanceOf(address owner, uint256 id) public view virtual returns (uint256 bal) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L73
    function uri(uint256 id) public view virtual returns (string memory);

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L105
        returns (uint256[] memory balances)

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L124
    function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L162
    ) internal returns (uint256) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L8
function toWadUnsafe(uint256 x) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L18
function toDaysWadUnsafe(uint256 x) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L28
function fromDaysWadUnsafe(int256 x) pure returns (uint256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L36
function unsafeWadMul(int256 x, int256 y) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L45
function unsafeWadDiv(int256 x, int256 y) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L52
function wadMul(int256 x, int256 y) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L67
function wadDiv(int256 x, int256 y) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L82
function wadExp(int256 x) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L140
function wadLn(int256 x) pure returns (int256 r) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L212
function unsafeDiv(int256 x, int256 y) pure returns (int256 r) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L27
    function tokenURI(uint256 id) external view virtual returns (string memory);

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L61
    function ownerOf(uint256 id) external view returns (address owner) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L65
    function balanceOf(address owner) external view returns (uint256) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L149
    function supportsInterface(bytes4 interfaceId) external pure returns (bool) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L178
    ) internal returns (uint256) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L28
    function tokenURI(uint256 id) external view virtual returns (string memory);

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L54
    function ownerOf(uint256 id) external view returns (address owner) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L58
    function balanceOf(address owner) external view returns (uint256) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L72
    function isApprovedForAll(address owner, address operator) public view returns (bool isApproved) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L162
    function supportsInterface(bytes4 interfaceId) external pure returns (bool) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L219
    function pagePrice() public view returns (uint256) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L239
    function mintCommunityPages(uint256 numPages) external returns (uint256 lastMintedPageId) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L265
    function tokenURI(uint256 pageId) public view virtual override returns (string memory) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/rand/ChainlinkV1RandProvider.sol#L62
    function requestRandomBytes() external returns (bytes32 requestId) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/MerkleProofLib.sol#L12
    ) internal pure returns (bool isValid) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/LibString.sol#L8
    function toString(uint256 value) internal pure returns (string memory str) {

https://github.com/transmissions11/goo-issuance/blob/5fe1e7d8a0c42a97c2a95d0547209f28dcbedb0b/src/LibGOO.sol#L21
    ) public pure returns (uint256) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/rand/RandProvider.sol#L21
    function requestRandomBytes() external returns (bytes32 requestId);

 ### mitigation
 - Add @param descriptors
 - Add @return descriptors

## ERC721 doesn't implement tokenURI
### Impact 
ERC721.tokenURI(uint256) is expected to be implemented.
### Github Permalinks
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L25

## Maximum line length exceeded
### Summary
Long lines should be wrapped to conform with Solidity Style guidelines. 
### Details 
Lines that exceed the 99 character length suggested by the Solidity Style guidelines. Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length
### Github Permalinks 

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L6

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L7

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L8

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L9

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L10

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L11

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L12

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L13

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L14

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L15

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L16

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L17

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L18

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L19

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L20

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L21

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L22

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L23

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L24

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L25

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L26

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L27

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L28

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L29

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L30

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L31

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L32

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L33

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L34

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L35

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L36

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L37

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L38

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L39

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L40

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L41

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L42

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L43

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L44

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L45

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L46

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L47

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L48

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L49

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L50

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L51

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L52

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L53

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L54

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L55

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L56

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L57

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L58

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L59

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L60

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L122

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L182

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L183

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L222

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L223

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L233

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L234

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L242

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L347

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L368

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L411

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L414

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L422

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L424

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L448

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L451

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L452

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L453

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L454

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L455

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L456

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L457

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L458

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L462

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L473

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L474

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L475

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L476

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L485

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L486

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L489

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L490

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L498

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L499

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L515

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L516

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L544

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L587

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L669

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L707

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L818

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L819

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L839

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L841

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L842

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L843

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L846

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L847

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L848

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L890

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L5

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L172

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L173

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L203

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L204

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L206

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L207

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L212

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L224

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/FixedPointMathLib.sol#L225

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L92

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L6

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L7

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L186

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L5

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L136

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L6

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L7

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L7

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L8

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L72

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L73

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L108

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L18

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L19

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L20

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L21

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L22

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L23

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L24

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L25

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L26

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L27

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L28

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L29

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L30

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L31

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L32

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L33

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L34

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L35

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L36

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L37

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L38

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L39

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L40

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L41

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L42

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L43

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L44

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L45

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L46

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L47

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L48

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L49

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L50

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L51

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L52

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L53

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L54

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L55

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L56

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L57

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L58

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L59

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L60

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L61

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L62

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L63

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L64

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L65

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L66

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L67

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L68

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L69

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L70

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L71

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L72

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L195

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L227

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L246

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L247

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L248

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L253

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/rand/ChainlinkV1RandProvider.sol#L61

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticToLinearVRGDA.sol#L32

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticToLinearVRGDA.sol#L33

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticToLinearVRGDA.sol#L37

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticToLinearVRGDA.sol#L58

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticToLinearVRGDA.sol#L59

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/MerkleProofLib.sol#L5

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/MerkleProofLib.sol#L6

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L13

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L14

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L15

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L16

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L17

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L18

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L19

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L20

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L21

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L22

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L23

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L24

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L25

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L26

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L27

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L28

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L29

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L30

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L31

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L32

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L33

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L34

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L35

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L36

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L37

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L38

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L39

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L40

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L41

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L42

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L43

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L44

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L45

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L46

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Goo.sol#L47

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticVRGDA.sol#L34

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticVRGDA.sol#L56

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticVRGDA.sol#L57

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LogisticVRGDA.sol#L62

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/LibString.sol#L6

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/LibString.sol#L10

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/LibString.sol#L11

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/LibString.sol#L12

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/VRGDA.sol#L25

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/VRGDA.sol#L43

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/VRGDA.sol#L49

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/VRGDA.sol#L55

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/VRGDA.sol#L56

https://github.com/transmissions11/goo-issuance/blob/5fe1e7d8a0c42a97c2a95d0547209f28dcbedb0b/src/LibGOO.sol#L15

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LinearVRGDA.sol#L23

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LinearVRGDA.sol#L24

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LinearVRGDA.sol#L37

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/LinearVRGDA.sol#L38
### Mitigation
Reduce line length to less than 99 at least to improve maintainability and readability of the code 

## State variables that do not change should be constant and written in UPPERCASE
### Summary
`constant` keyword helps with readability of the code and to make sure that they do not change. 

### Details
Code contains state variables that do not change and so they can be declared `constant`

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/script/deploy/DeployRinkeby.s.sol#L7-L11

### Mitigation
Add constant to these variables
