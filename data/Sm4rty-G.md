

## GAS-01. Pre-increment costs less gas as compared to Post-increment :

++i costs less gas as compared to i++ for unsigned integer, as per-increment is cheaper(its about 5 gas per iteration cheaper)

i++ increments i and returns initial value of i. Which means

```
uint i =  1;
i++; // ==1 but i ==2
```

But ++i returns the actual incremented value:

```
uint i = 1;
++i; // ==2 and i ==2 , no need for temporary variable here
```

In the first case, the compiler has create a temporary variable (when used) for returning 1 instead of 2.

### Instances:

```
src/utils/token/PagesERC721.sol:117:            _balanceOf[to]++;
src/utils/token/PagesERC721.sol:181:            _balanceOf[to]++;
lib/solmate/src/tokens/ERC721.sol:101:            _balanceOf[to]++;
lib/solmate/src/tokens/ERC721.sol:164:            _balanceOf[to]++;
src/utils/token/PagesERC721.sol:115:            _balanceOf[from]--;
lib/solmate/src/tokens/ERC721.sol:99:            _balanceOf[from]--;
lib/solmate/src/tokens/ERC721.sol:179:            _balanceOf[owner]--;
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
src/ArtGobblers.sol:464:            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
src/ArtGobblers.sol:913:            getUserData[to].gobblersOwned += 1;
src/ArtGobblers.sol:906:            getUserData[from].gobblersOwned -= 1;
``` 

### Recommendations:
Change post-increment to pre-increment.

-----

## GAS-02. ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)

### Instances:

```
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
``` 

### Reference:

[https://code4rena.com/reports/2022-05-cally#g-11-ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for--and-while-loops](https://code4rena.com/reports/2022-05-cally#g-11-ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for--and-while-loops)

-----

## GAS-03. Using > 0 costs more gas than != 0 when used on a uint in a require() statement

> 0 is less efficient than != 0 for unsigned integers (with proof)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas) Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proof: https://twitter.com/gzeon/status/1485428085885640706

### Instances

```
lib/solmate/src/utils/SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
``` 

### Reference:

[https://twitter.com/gzeon/status/1485428085885640706](https://twitter.com/gzeon/status/1485428085885640706)

### Remediation:

I suggest changing > 0 with != 0. Also, please enable the Optimizer.

-----

## GAS-04. An array's length should be cached to save gas in for-loops

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

### Instances:

```
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
``` 


### Remediation:
Here, I suggest storing the array's length in a variable before the for-loop, and use it instead.

-----

## GAS-05. Splitting require() statements that use && saves gas

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

### Instances

```
lib/solmate/src/utils/FixedPointMathLib.sol:43:            // Equivalent to require(denominator != 0 && (x == 0 || (x * y) / x == y))
lib/solmate/src/utils/FixedPointMathLib.sol:62:            // Equivalent to require(denominator != 0 && (x == 0 || (x * y) / x == y))
lib/solmate/src/utils/SignedWadMath.sol:72:        // Equivalent to require(y != 0 && ((x * 1e18) / 1e18 == x))
``` 

### Mitigation:
Breakdown each condition in a separate require statement (though require statements should be replaced with custom errors)

-----

## GAS-06. Custom Errors instead of Revert Strings to save Gas

Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met). Starting from Solidity v0.8.4,there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");),but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.
Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

### Instances

```
src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
src/utils/token/GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
src/utils/token/PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
src/ArtGobblers.sol:437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
src/ArtGobblers.sol:885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
src/ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/utils/SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
lib/solmate/src/auth/Owned.sol:20:        require(msg.sender == owner, "UNAUTHORIZED");
lib/solmate/src/tokens/ERC721.sol:36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:40:        require(owner != address(0), "ZERO_ADDRESS");
lib/solmate/src/tokens/ERC721.sol:69:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
lib/solmate/src/tokens/ERC721.sol:87:        require(from == _ownerOf[id], "WRONG_FROM");
lib/solmate/src/tokens/ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:160:        require(_ownerOf[id] == address(0), "ALREADY_MINTED");
lib/solmate/src/tokens/ERC721.sol:175:        require(owner != address(0), "NOT_MINTED");
lib/VRGDAs/src/VRGDA.sol:32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
``` 


### Remediation:
I suggest replacing revert strings with custom errors.

-----


## GAS-07. x += y costs more gas than x = x + y for state variables

### Instances:

```
src/utils/token/GobblersERC721.sol:184:            getUserData[to].gobblersOwned += uint32(amount);
src/ArtGobblers.sol:439:                burnedMultipleTotal += getGobblerData[id].emissionMultiple;
src/ArtGobblers.sol:456:            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
src/ArtGobblers.sol:464:            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
src/ArtGobblers.sol:662:                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
src/ArtGobblers.sol:844:            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
src/ArtGobblers.sol:912:            getUserData[to].emissionMultiple += emissionMultiple;
src/ArtGobblers.sol:913:            getUserData[to].gobblersOwned += 1;
src/Pages.sol:244:            uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);
src/ArtGobblers.sol:905:            getUserData[from].emissionMultiple -= emissionMultiple;
src/ArtGobblers.sol:906:            getUserData[from].gobblersOwned -= 1;
``` 
### References:

[https://github.com/code-423n4/2022-05-backd-findings/issues/108](https://github.com/code-423n4/2022-05-backd-findings/issues/108)


-----


## GAS-08. Using bools for storage incurs overhead

```
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
```

[Refer Here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

### Instances:

```
src/utils/token/GobblersERC721.sol:77:    mapping(address => mapping(address => bool)) public isApprovedForAll;
src/utils/token/GobblersERC1155B.sol:37:    mapping(address => mapping(address => bool)) public isApprovedForAll;
src/utils/token/PagesERC721.sol:70:    mapping(address => mapping(address => bool)) internal _isApprovedForAll;
src/ArtGobblers.sol:149:    mapping(address => bool) public hasClaimedMintlistGobbler;
src/ArtGobblers.sol:212:        bool waitingForSeed;
lib/solmate/src/tokens/ERC721.sol:51:    mapping(address => mapping(address => bool)) public isApprovedForAll;
``` 
### References:

[https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead](https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead)


-----



## GAS-09. Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

### Instances:

```
src/Goo.sol:2:pragma solidity >=0.8.0;
src/utils/rand/ChainlinkV1RandProvider.sol:2:pragma solidity >=0.8.0;
src/utils/GobblerReserve.sol:2:pragma solidity >=0.8.0;
src/utils/token/GobblersERC721.sol:2:pragma solidity >=0.8.0;
src/utils/token/GobblersERC1155B.sol:2:pragma solidity >=0.8.0;
src/utils/token/PagesERC721.sol:2:pragma solidity >=0.8.0;
src/ArtGobblers.sol:2:pragma solidity >=0.8.0;
src/Pages.sol:2:pragma solidity >=0.8.0;
script/deploy/DeployRinkeby.s.sol:2:pragma solidity >=0.8.0;
script/deploy/DeployBase.s.sol:2:pragma solidity >=0.8.0;
lib/goo-issuance/src/LibGOO.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/LibString.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/FixedPointMathLib.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/SignedWadMath.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/MerkleProofLib.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/auth/Owned.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/tokens/ERC721.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LogisticToLinearVRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LinearVRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/VRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LogisticVRGDA.sol:2:pragma solidity >=0.8.0;
``` 
### References:

[https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity)


-----

## GAS-10. Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, 'zero address')
  revert(0x00, 0x20)
 }
}
```

### Instances:
```
C4_Report/GAS.md:126:src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
C4_Report/GAS.md:127:src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
C4_Report/GAS.md:130:src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
C4_Report/GAS.md:131:src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
C4_Report/GAS.md:134:src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
C4_Report/GAS.md:137:src/ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
C4_Report/GAS.md:140:lib/solmate/src/tokens/ERC721.sol:36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
C4_Report/GAS.md:141:lib/solmate/src/tokens/ERC721.sol:40:        require(owner != address(0), "ZERO_ADDRESS");
C4_Report/GAS.md:144:lib/solmate/src/tokens/ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
C4_Report/GAS.md:145:lib/solmate/src/tokens/ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
C4_Report/GAS.md:147:lib/solmate/src/tokens/ERC721.sol:175:        require(owner != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
src/ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:40:        require(owner != address(0), "ZERO_ADDRESS");
lib/solmate/src/tokens/ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:175:        require(owner != address(0), "NOT_MINTED");
```

-----
