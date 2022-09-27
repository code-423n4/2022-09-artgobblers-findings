# 1. Use a more recent version of solidity
### Description
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
### Links to github files
[ArtGobblers.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L2)
[Goo.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Goo.sol#L2)
[Pages.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L2)
[ChainlinkV1RandProvider.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L2)
[GobblerReserve.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L2)
[PagesERC721.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L2)
[GobblersERC1155B.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L2)
[GobblersERC721.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L2)
[DeployRinkeby.s.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L2)
[DeployBase.s.sol:L2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L2)
[LogisticToLinearVRGDA.sol:L2](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticToLinearVRGDA.sol#L2)
[LogisticVRGDA.sol:L2](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L2)
[LinearVRGDA.sol:L2](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol#L2)
[VRGDA.sol:L2](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L2)
[Owned.sol:L2](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L2)
[ERC721.sol:L2](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L2)
[LibString.sol:L2](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/LibString.sol#L2)
[SignedWadMath.sol:L2](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L2)
[MerkleProofLib.sol:L2](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/MerkleProofLib.sol#L2)
[FixedPointMathLib.sol:L2](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L2)
[LibGOO.sol:L2](https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L2)
### Instances
```
src/ArtGobblers.sol:2:pragma solidity >=0.8.0;
src/Goo.sol:2:pragma solidity >=0.8.0;
src/Pages.sol:2:pragma solidity >=0.8.0;
src/utils/rand/ChainlinkV1RandProvider.sol:2:pragma solidity >=0.8.0;
src/utils/GobblerReserve.sol:2:pragma solidity >=0.8.0;
src/utils/token/PagesERC721.sol:2:pragma solidity >=0.8.0;
src/utils/token/GobblersERC1155B.sol:2:pragma solidity >=0.8.0;
src/utils/token/GobblersERC721.sol:2:pragma solidity >=0.8.0;
script/deploy/DeployRinkeby.s.sol:2:pragma solidity >=0.8.0;
script/deploy/DeployBase.s.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LogisticToLinearVRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LogisticVRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/LinearVRGDA.sol:2:pragma solidity >=0.8.0;
lib/VRGDAs/src/VRGDA.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/auth/Owned.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/tokens/ERC721.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/LibString.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/SignedWadMath.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/MerkleProofLib.sol:2:pragma solidity >=0.8.0;
lib/solmate/src/utils/FixedPointMathLib.sol:2:pragma solidity >=0.8.0;
lib/goo-issuance/src/LibGOO.sol:2:pragma solidity >=0.8.0;
```
----
# 2. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
`<x> += <y>` costs more gas as compared to `<x> = <x> + <y>`
### Links to github files
[ArtGobblers.sol:L439](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L439)
[ArtGobblers.sol:L456](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L456)
[ArtGobblers.sol:L464](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L464)
[ArtGobblers.sol:L662](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L662)
[ArtGobblers.sol:L912](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L912)
[GobblersERC721.sol:L184](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L184)
[SignedWadMath.sol:L203](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L203)
[SignedWadMath.sol:L205](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L205)
### Instances
```
src/ArtGobblers.sol:439:                burnedMultipleTotal += getGobblerData[id].emissionMultiple;
src/ArtGobblers.sol:456:            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
src/ArtGobblers.sol:464:            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
src/ArtGobblers.sol:662:                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
src/ArtGobblers.sol:912:            getUserData[to].emissionMultiple += emissionMultiple;
src/utils/token/GobblersERC721.sol:184:            getUserData[to].gobblersOwned += uint32(amount);
lib/solmate/src/utils/SignedWadMath.sol:203:        r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
lib/solmate/src/utils/SignedWadMath.sol:205:        r += 600920179829731861736702779321621459595472258049074101567377883020018308;
```
`<x> -= <y>` costs more gas as compared to `<x> = <x> - <y>`
### Links to github files
[ArtGobblers.sol:L905](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L905)
### Instances
```
src/ArtGobblers.sol:905:            getUserData[from].emissionMultiple -= emissionMultiple;
```
`<x> *= <y>` costs more gas as compared to `<x> = <x> * <y>`
### Links to github files
[SignedWadMath.sol:L201](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L201)
### Instances
```
lib/solmate/src/utils/SignedWadMath.sol:201:        r *= 1677202110996718588342820967067443963516166;
```
----
# 3. Using > 0 costs more gas than != 0 when used on a uint in a require() statement
### Description
0 is less efficient than != 0 for unsigned integers (with proof)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas) Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proof: https://twitter.com/gzeon/status/1485428085885640706
### Links to github files
[SignedWadMath.sol:L142](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L142)
### Instances
```
lib/solmate/src/utils/SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
```
### Recommended Mitigation Steps:
I suggest changing > 0 with != 0. Also, please enable the Optimizer.

----
# 4. ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops
### Description
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)
### Links to github files
[ArtGobblers.sol:L432](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432)
[ArtGobblers.sol:L592](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592)
[Pages.sol:L251](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251)
[GobblerReserve.sol:L37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37)
[GobblersERC1155B.sol:L114](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114)
[GobblersERC1155B.sol:L173](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L173)
[GobblersERC721.sol:L186](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186)
### Instances
```
src/ArtGobblers.sol:432:            for (uint256 i = 0; i < cost; ++i) {
src/ArtGobblers.sol:592:            for (uint256 i = 0; i < numGobblers; ++i) {
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
src/utils/token/GobblersERC1155B.sol:173:            for (uint256 i = 0; i < amount; ++i) {
src/utils/token/GobblersERC721.sol:186:            for (uint256 i = 0; i < amount; ++i) {
```
----
# 5. IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED
### Description
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for `(uint256 i = 0; i < numIterations; ++i)` { should be replaced with for `(uint256 i; i < numIterations; ++i) {`
### Links to github files
[ArtGobblers.sol:L432](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432)
[ArtGobblers.sol:L592](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592)
[Pages.sol:L251](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251)
[GobblerReserve.sol:L37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37)
[GobblersERC1155B.sol:L114](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114)
[GobblersERC1155B.sol:L173](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L173)
[GobblersERC721.sol:L186](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186)
### Instances
```
src/ArtGobblers.sol:432:            for (uint256 i = 0; i < cost; ++i) {
src/ArtGobblers.sol:592:            for (uint256 i = 0; i < numGobblers; ++i) {
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
src/utils/token/GobblersERC1155B.sol:173:            for (uint256 i = 0; i < amount; ++i) {
src/utils/token/GobblersERC721.sol:186:            for (uint256 i = 0; i < amount; ++i) {
```
----
# 6. ++I COSTS LESS GAS COMPARED TO I++ OR I += 1
*Pre-increments and pre-decrements are cheaper.*

For a `uint256` i variable, the following is true with the Optimizer enabled at 10k:
Increment:
`i += 1` is the most expensive form
`i++` costs `6` `gas` less than `i += 1`
`++i` costs `5 gas` less than `i++` (11 gas less than i += 1)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name post-increment:
### Links to github files
[Pages.sol:L251](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251)
[GobblerReserve.sol:L37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37)
[PagesERC721.sol:L117](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L117)
[PagesERC721.sol:L181](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L181)
[ERC721.sol:L101](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L101)
[ERC721.sol:L164](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L164)
[ArtGobblers.sol:L913](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L913)
### Instances
```
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/token/PagesERC721.sol:117:            _balanceOf[to]++;
src/utils/token/PagesERC721.sol:181:            _balanceOf[to]++;
lib/solmate/src/tokens/ERC721.sol:101:            _balanceOf[to]++;
lib/solmate/src/tokens/ERC721.sol:164:            _balanceOf[to]++;
src/ArtGobblers.sol:913:            getUserData[to].gobblersOwned += 1;
```
Similarly
`i -= 1` is the most expensive form `i--`
`i--` is more expennsive than `--i`
### Links to github files
[PagesERC721.sol:L115](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L115)
[ERC721.sol:L99](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L99)
[ERC721.sol:L179](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L179)
[ArtGobblers.sol:L906](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L906)
### Instances
```
src/utils/token/PagesERC721.sol:115:            _balanceOf[from]--;
lib/solmate/src/tokens/ERC721.sol:99:            _balanceOf[from]--;
lib/solmate/src/tokens/ERC721.sol:179:            _balanceOf[owner]--;
src/ArtGobblers.sol:906:            getUserData[from].gobblersOwned -= 1;
```
----
# 7.`<ARRAY>.LENGTH` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP
### Description
The overheads outlined below are `PER LOOP`, excluding the first loop
storage arrays incur a Gwarmaccess (100 gas) memory arrays use `MLOAD` (3 gas)
calldata arrays use `CALLDATALOAD` (3 gas) Caching the length changes each of these to a `DUP<N>` (3 gas), and gets rid of the extra `DUP<N>` needed to store the stack offset
### Links to github files
[GobblerReserve.sol:L37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37)
[GobblersERC1155B.sol:L114](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114)
### Instances
```
src/utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
src/utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
```
----
# 8. Custom Errors instead of Revert Strings to save Gas
### Description
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

Starting from Solidity v0.8.4,there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");),but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).
### Links to github files
[ArtGobblers.sol:L437](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L437)
[ArtGobblers.sol:L885](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L885)
[ArtGobblers.sol:L887](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L887)
[ArtGobblers.sol:L889](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L889)
[PagesERC721.sol:L55](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L55)
[PagesERC721.sol:L59](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L59)
[PagesERC721.sol:L85](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L85)
[PagesERC721.sol:L103](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L103)
[PagesERC721.sol:L105](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L105)
[PagesERC721.sol:L107](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L107)
[PagesERC721.sol:L135](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L135)
[PagesERC721.sol:L151](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L151)
[GobblersERC1155B.sol:L107](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L107)
[GobblersERC1155B.sol:L149](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L149)
[GobblersERC1155B.sol:L185](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L185)
[GobblersERC721.sol:L62](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L62)
[GobblersERC721.sol:L66](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L66)
[GobblersERC721.sol:L95](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L95)
[GobblersERC721.sol:L121](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L121)
[GobblersERC721.sol:L137](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L137)
[VRGDA.sol:L32](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L32)
[Owned.sol:L20](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L20)
[ERC721.sol:L36](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L36)
[ERC721.sol:L40](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L40)
[ERC721.sol:L69](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L69)
[ERC721.sol:L87](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L87)
[ERC721.sol:L89](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L89)
[ERC721.sol:L91](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L91)
[ERC721.sol:L118](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L118)
[ERC721.sol:L134](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L134)
[ERC721.sol:L158](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L158)
[ERC721.sol:L160](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L160)
[ERC721.sol:L175](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L175)
[ERC721.sol:L196](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L196)
[ERC721.sol:L211](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L211)
[SignedWadMath.sol:L142](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L142)
### Instances
```
src/ArtGobblers.sol:437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
src/ArtGobblers.sol:885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
src/ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
src/ArtGobblers.sol:889:        require(
src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
src/utils/token/PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
src/utils/token/PagesERC721.sol:107:        require(
src/utils/token/PagesERC721.sol:135:            require(
src/utils/token/PagesERC721.sol:151:            require(
src/utils/token/GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
src/utils/token/GobblersERC1155B.sol:149:            require(
src/utils/token/GobblersERC1155B.sol:185:            require(
src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
src/utils/token/GobblersERC721.sol:121:        require(
src/utils/token/GobblersERC721.sol:137:        require(
lib/VRGDAs/src/VRGDA.sol:32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
lib/solmate/src/auth/Owned.sol:20:        require(msg.sender == owner, "UNAUTHORIZED");
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
lib/solmate/src/utils/SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
```
### Recommended Mitigation Steps:
I suggest replacing revert strings with custom errors.

----
# 9. Use Shift Right/Left instead of Division/Multiplication 2X
A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left.

While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.
### Links to github files
[ArtGobblers.sol:L462](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L462)
### Instances
```
src/ArtGobblers.sol:462:                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```
### Recommended Mitigation Steps:
You should change multiplication and division by powers of two to bit shift.

----
# 10. Strict inequalities (>) are more expensive than non-strict ones (>=)

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas. I suggest using >= instead of > to avoid some opcodes here:
### Links to github files
[ArtGobblers.sol:L341](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L341)
[ArtGobblers.sol:L375](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L375)
[ArtGobblers.sol:L415](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L415)
[ArtGobblers.sol:L490](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L490)
[ArtGobblers.sol:L587](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L587)
[ArtGobblers.sol:L848](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L848)
[Pages.sol:L200](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L200)
[Pages.sol:L248](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L248)
[Pages.sol:L266](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L266)
### Instances
```
src/ArtGobblers.sol:341:        if (mintStart > block.timestamp) revert MintStartPending();
src/ArtGobblers.sol:375:        if (currentPrice > maxPrice) revert PriceExceededMax(currentPrice);
src/ArtGobblers.sol:415:        if (gobblerId > MAX_SUPPLY) revert NoRemainingLegendaryGobblers();
src/ArtGobblers.sol:490:            if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);
src/ArtGobblers.sol:587:        if (numGobblers > totalRemainingToBeRevealed) revert NotEnoughRemainingToBeRevealed(totalRemainingToBeRevealed);
src/ArtGobblers.sol:848:            if (newNumMintedForReserves > (numMintedFromGoo + newNumMintedForReserves) / 5) revert ReserveImbalance();
src/Pages.sol:200:        if (currentPrice > maxPrice) revert PriceExceededMax(currentPrice);
src/Pages.sol:248:            if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();
src/Pages.sol:266:        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```
----
# 11. Using private rather than public for constants / immutable, saves gas
### Description
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves **3406-3606** gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table
### Links to github files
[ArtGobblers.sol:L112](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L112)
[ArtGobblers.sol:L115](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L115)
[ArtGobblers.sol:L118](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L118)
[ArtGobblers.sol:L122](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L122)
[ArtGobblers.sol:L126](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L126)
[ArtGobblers.sol:L177](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L177)
[ArtGobblers.sol:L180](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L180)
[ArtGobblers.sol:L184](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L184)
[DeployRinkeby.s.sol:L13](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L13)
[DeployRinkeby.s.sol:L14](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L14)
[DeployRinkeby.s.sol:L15](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L15)
[ArtGobblers.sol:L92](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L92)
[ArtGobblers.sol:L95](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L95)
[ArtGobblers.sol:L98](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L98)
[ArtGobblers.sol:L101](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L101)
[ArtGobblers.sol:L146](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L146)
[ArtGobblers.sol:L156](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L156)
[Goo.sol:L64](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Goo.sol#L64)
[Goo.sol:L67](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Goo.sol#L67)
[Pages.sol:L86](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L86)
[Pages.sol:L89](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L89)
[Pages.sol:L103](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L103)
[ChainlinkV1RandProvider.sol:L20](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L20)
[GobblerReserve.sol:L18](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L18)
[PagesERC721.sol:L34](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L34)
[DeployRinkeby.s.sol:L7](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L7)
[DeployRinkeby.s.sol:L9](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L9)
[DeployRinkeby.s.sol:L11](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L11)
[LogisticVRGDA.sol:L20](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L20)
[LogisticVRGDA.sol:L25](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L25)
[VRGDA.sol:L17](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L17)
### Instances
```
src/ArtGobblers.sol:112:    uint256 public constant MAX_SUPPLY = 10000;
src/ArtGobblers.sol:115:    uint256 public constant MINTLIST_SUPPLY = 2000;
src/ArtGobblers.sol:118:    uint256 public constant LEGENDARY_SUPPLY = 10;
src/ArtGobblers.sol:122:    uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
src/ArtGobblers.sol:126:    uint256 public constant MAX_MINTABLE = MAX_SUPPLY
src/ArtGobblers.sol:177:    uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
src/ArtGobblers.sol:180:    uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
src/ArtGobblers.sol:184:    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
script/deploy/DeployRinkeby.s.sol:13:    string public constant gobblerBaseUri = "https://testnet.ag.xyz/api/nfts/gobblers/";
script/deploy/DeployRinkeby.s.sol:14:    string public constant gobblerUnrevealedUri = "https://testnet.ag.xyz/api/nfts/unrevealed";
script/deploy/DeployRinkeby.s.sol:15:    string public constant pagesBaseUri = "https://testnet.ag.xyz/api/nfts/pages/";
src/ArtGobblers.sol:92:    Goo public immutable goo;
src/ArtGobblers.sol:95:    Pages public immutable pages;
src/ArtGobblers.sol:98:    address public immutable team;
src/ArtGobblers.sol:101:    address public immutable community;
src/ArtGobblers.sol:146:    bytes32 public immutable merkleRoot;
src/ArtGobblers.sol:156:    uint256 public immutable mintStart;
src/Goo.sol:64:    address public immutable artGobblers;
src/Goo.sol:67:    address public immutable pages;
src/Pages.sol:86:    Goo public immutable goo;
src/Pages.sol:89:    address public immutable community;
src/Pages.sol:103:    uint256 public immutable mintStart;
src/utils/rand/ChainlinkV1RandProvider.sol:20:    ArtGobblers public immutable artGobblers;
src/utils/GobblerReserve.sol:18:    ArtGobblers public immutable artGobblers;
src/utils/token/PagesERC721.sol:34:    ArtGobblers public immutable artGobblers;
script/deploy/DeployRinkeby.s.sol:7:    address public immutable coldWallet = 0x126620598A797e6D9d2C280b5dB91b46F27A8330;
script/deploy/DeployRinkeby.s.sol:9:    address public immutable root = 0x1D18077167c1177253555e45B4b5448B11E30b4b;
script/deploy/DeployRinkeby.s.sol:11:    uint256 public immutable mintStart = 1656369768;
lib/VRGDAs/src/LogisticVRGDA.sol:20:    int256 public immutable logisticLimit;
lib/VRGDAs/src/LogisticVRGDA.sol:25:    int256 public immutable logisticLimitDoubled;
lib/VRGDAs/src/VRGDA.sol:17:    int256 public immutable targetPrice;
```
----
