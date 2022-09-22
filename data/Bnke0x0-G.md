
 
### [G01] State variables only set in the constructor should be declared `immutable`


#### Findings:
```
2022-09-artgobblers/script/deploy/DeployBase.s.sol::25 => string private gobblerBaseUri;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::26 => string private gobblerUnrevealedUri;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::27 => string private pagesBaseUri;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::30 => GobblerReserve public teamReserve;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::31 => GobblerReserve public communityReserve;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::32 => Goo public goo;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::33 => RandProvider public randProvider;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::34 => ArtGobblers public artGobblers;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::35 => Pages public pages;
2022-09-artgobblers/solmate/src/auth/Owned.sol::17 => address public owner;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::21 => string public name;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::23 => string public symbol;
2022-09-artgobblers/src/ArtGobblers.sol::105 => RandProvider public randProvider;
2022-09-artgobblers/src/ArtGobblers.sol::136 => string public UNREVEALED_URI;
2022-09-artgobblers/src/ArtGobblers.sol::139 => string public BASE_URI;
2022-09-artgobblers/src/ArtGobblers.sol::159 => uint128 public numMintedFromGoo;
2022-09-artgobblers/src/ArtGobblers.sol::167 => uint128 public currentNonLegendaryId;
2022-09-artgobblers/src/ArtGobblers.sol::170 => uint256 public numMintedForReserves;
2022-09-artgobblers/src/ArtGobblers.sol::177 => uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
2022-09-artgobblers/src/ArtGobblers.sol::180 => uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
2022-09-artgobblers/src/ArtGobblers.sol::184 => uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
2022-09-artgobblers/src/ArtGobblers.sol::195 => LegendaryGobblerAuctionData public legendaryGobblerAuctionData;
2022-09-artgobblers/src/ArtGobblers.sol::216 => GobblerRevealsData public gobblerRevealsData;
2022-09-artgobblers/src/Pages.sol::96 => string public BASE_URI;
2022-09-artgobblers/src/Pages.sol::107 => uint128 public currentId;
2022-09-artgobblers/src/Pages.sol::114 => uint128 public numMintedForCommunity;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::23 => string public name;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::25 => string public symbol;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::24 => string public name;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::26 => string public symbol;
```





### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor, avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:
```
2022-09-artgobblers/solmate/src/auth/Owned.sol::17 => address public owner;
```





### [G03] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:
```
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
```





### [G04] `++i/i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for` and `while` loops


#### Findings:
```

2022-09-artgobblers/src/ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {
2022-09-artgobblers/src/ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```


### [G05] Not using the named return variables when a function returns, wastes deployment gas


#### Findings:
```
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::42 => return _balanceOf[owner];
2022-09-artgobblers/src/utils/token/PagesERC721.sol::61 => return _balanceOf[owner];
2022-09-artgobblers/src/utils/token/PagesERC721.sol::75 => return _isApprovedForAll[owner][operator];
```





### [G06] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::142 => require(x > 0, "UNDEFINED");
```





### [G07] It costs more gas to initialize variables to zero than to let the default of zero be applied


#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {
2022-09-artgobblers/src/ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```





### [G08] `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)


#### Findings:
```
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
```







### [G09] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

#### Impact
> When using elements that are smaller than 32 bytes, your 
contract’s gas usage may be higher. This is because the EVM operates on 
32 bytes at a time. Therefore, if the element is smaller than that, the 
EVM must use more operations in order to reduce the size of the element 
from 32 bytes to the desired size.
> 

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed
#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::159 => uint128 public numMintedFromGoo;
2022-09-artgobblers/src/ArtGobblers.sol::167 => uint128 public currentNonLegendaryId;
2022-09-artgobblers/src/ArtGobblers.sol::189 => uint128 startPrice;
2022-09-artgobblers/src/ArtGobblers.sol::191 => uint128 numSold;
2022-09-artgobblers/src/ArtGobblers.sol::204 => uint64 randomSeed;
2022-09-artgobblers/src/ArtGobblers.sol::206 => uint64 nextRevealTimestamp;
2022-09-artgobblers/src/ArtGobblers.sol::208 => uint56 lastRevealedId;
2022-09-artgobblers/src/ArtGobblers.sol::210 => uint56 toBeRevealed;
2022-09-artgobblers/src/ArtGobblers.sol::324 => legendaryGobblerAuctionData.startPrice = uint128(LEGENDARY_GOBBLER_INITIAL_START_PRICE);
2022-09-artgobblers/src/ArtGobblers.sol::327 => gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days);
2022-09-artgobblers/src/ArtGobblers.sol::449 => getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
2022-09-artgobblers/src/ArtGobblers.sol::454 => getUserData[msg.sender].lastBalance = uint128(gooBalance(msg.sender)); // Checkpoint balance.
2022-09-artgobblers/src/ArtGobblers.sol::455 => getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
2022-09-artgobblers/src/ArtGobblers.sol::456 => getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
2022-09-artgobblers/src/ArtGobblers.sol::458 => getUserData[msg.sender].gobblersOwned = uint32(getUserData[msg.sender].gobblersOwned - cost + 1);
2022-09-artgobblers/src/ArtGobblers.sol::461 => legendaryGobblerAuctionData.startPrice = uint120(
2022-09-artgobblers/src/ArtGobblers.sol::531 => gobblerRevealsData.toBeRevealed = uint56(toBeRevealed);
2022-09-artgobblers/src/ArtGobblers.sol::535 => gobblerRevealsData.nextRevealTimestamp = uint64(nextRevealTimestamp + 1 days);
2022-09-artgobblers/src/ArtGobblers.sol::550 => // The unchecked cast to uint64 is equivalent to moduloing the randomness by 2**64.
2022-09-artgobblers/src/ArtGobblers.sol::551 => gobblerRevealsData.randomSeed = uint64(randomness); // 64 bits of randomness is plenty.
2022-09-artgobblers/src/ArtGobblers.sol::615 => uint64 swapIndex = getGobblerData[swapId].idx == 0
2022-09-artgobblers/src/ArtGobblers.sol::616 => ? uint64(swapId) // Hasn't been shuffled before.
2022-09-artgobblers/src/ArtGobblers.sol::623 => uint64 currentIndex = getGobblerData[currentId].idx == 0
2022-09-artgobblers/src/ArtGobblers.sol::624 => ? uint64(currentId) // Hasn't been shuffled before.
2022-09-artgobblers/src/ArtGobblers.sol::650 => getGobblerData[currentId].emissionMultiple = uint32(newCurrentIdMultiple);
2022-09-artgobblers/src/ArtGobblers.sol::660 => getUserData[currentIdOwner].lastBalance = uint128(gooBalance(currentIdOwner));
2022-09-artgobblers/src/ArtGobblers.sol::661 => getUserData[currentIdOwner].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/ArtGobblers.sol::662 => getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
2022-09-artgobblers/src/ArtGobblers.sol::665 => // It is critical that we cast to uint64 here, as otherwise the random seed
2022-09-artgobblers/src/ArtGobblers.sol::673 => // Moduloing by 1 << 64 (2 ** 64) is equivalent to a uint64 cast.
2022-09-artgobblers/src/ArtGobblers.sol::679 => gobblerRevealsData.randomSeed = uint64(randomSeed);
2022-09-artgobblers/src/ArtGobblers.sol::680 => gobblerRevealsData.lastRevealedId = uint56(lastRevealedId);
2022-09-artgobblers/src/ArtGobblers.sol::681 => gobblerRevealsData.toBeRevealed = uint56(totalRemainingToBeRevealed - numGobblers);
2022-09-artgobblers/src/ArtGobblers.sol::825 => getUserData[user].lastBalance = uint128(updatedBalance);
2022-09-artgobblers/src/ArtGobblers.sol::826 => getUserData[user].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/ArtGobblers.sol::899 => uint32 emissionMultiple = getGobblerData[id].emissionMultiple; // Caching saves gas.
2022-09-artgobblers/src/ArtGobblers.sol::903 => getUserData[from].lastBalance = uint128(gooBalance(from));
2022-09-artgobblers/src/ArtGobblers.sol::904 => getUserData[from].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/ArtGobblers.sol::910 => getUserData[to].lastBalance = uint128(gooBalance(to));
2022-09-artgobblers/src/ArtGobblers.sol::911 => getUserData[to].lastTimestamp = uint64(block.timestamp);
2022-09-artgobblers/src/Pages.sol::107 => uint128 public currentId;
2022-09-artgobblers/src/Pages.sol::114 => uint128 public numMintedForCommunity;
2022-09-artgobblers/src/Pages.sol::253 => currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id.
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::48 => uint64 idx;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::50 => uint32 emissionMultiple;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::38 => uint64 idx;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::40 => uint32 emissionMultiple;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::49 => uint32 gobblersOwned;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::51 => uint32 emissionMultiple;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::53 => uint128 lastBalance;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::55 => uint64 lastTimestamp;
```







### [G10] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function


#### Findings:
```
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::151 => revert(0, 0)

2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::74 => revert(0, 0)
```





### [G11] `require()` or `revert()` statements that check input arguments should be at the top of the function

#### Impact
Checks that involve constants should come before checks that involve state variables
#### Findings:
```
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::36 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::40 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::89 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::158 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::175 => require(owner != address(0), "NOT_MINTED");
2022-09-artgobblers/src/ArtGobblers.sol::887 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::62 => require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::66 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::55 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::59 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::105 => require(to != address(0), "INVALID_RECIPIENT");
```





### [G12] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::32 => require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
2022-09-artgobblers/solmate/src/auth/Owned.sol::20 => require(msg.sender == owner, "UNAUTHORIZED");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::36 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::40 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::69 => require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::87 => require(from == _ownerOf[id], "WRONG_FROM");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::89 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::91 => require(
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::118 => require(
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::134 => require(
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::158 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::160 => require(_ownerOf[id] == address(0), "ALREADY_MINTED");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::175 => require(owner != address(0), "NOT_MINTED");
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::196 => require(
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::211 => require(
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::142 => require(x > 0, "UNDEFINED");
2022-09-artgobblers/src/ArtGobblers.sol::437 => require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
2022-09-artgobblers/src/ArtGobblers.sol::711 => revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.
2022-09-artgobblers/src/ArtGobblers.sol::885 => require(from == getGobblerData[id].owner, "WRONG_FROM");
2022-09-artgobblers/src/ArtGobblers.sol::887 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/src/ArtGobblers.sol::889 => require(
2022-09-artgobblers/src/Pages.sol::266 => if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::107 => require(owners.length == ids.length, "LENGTH_MISMATCH");
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::149 => require(
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::185 => require(
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::62 => require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::66 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::95 => require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::121 => require(
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::137 => require(
2022-09-artgobblers/src/utils/token/PagesERC721.sol::55 => require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::59 => require(owner != address(0), "ZERO_ADDRESS");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::85 => require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::103 => require(from == _ownerOf[id], "WRONG_FROM");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::105 => require(to != address(0), "INVALID_RECIPIENT");
2022-09-artgobblers/src/utils/token/PagesERC721.sol::107 => require(
2022-09-artgobblers/src/utils/token/PagesERC721.sol::135 => require(
2022-09-artgobblers/src/utils/token/PagesERC721.sol::151 => require(
```





### [G13] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
2022-09-artgobblers/solmate/src/auth/Owned.sol::39 => function setOwner(address newOwner) public virtual onlyOwner {
2022-09-artgobblers/src/ArtGobblers.sol::560 => function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
2022-09-artgobblers/src/Goo.sol::101 => function mintForGobblers(address to, uint256 amount) external only(artGobblers) {
2022-09-artgobblers/src/Goo.sol::108 => function burnForGobblers(address from, uint256 amount) external only(artGobblers) {
2022-09-artgobblers/src/Goo.sol::115 => function burnForPages(address from, uint256 amount) external only(pages) {
2022-09-artgobblers/src/utils/GobblerReserve.sol::34 => function withdraw(address to, uint256[] calldata ids) external onlyOwner {
```





### [G14] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.15 to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/goo-issuance/src/ibGOO.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/auth/Owned.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/LibString.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::2 => pragma solidity >=0.8.0;
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





### [G15] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done at some places, it’s not consistently done in the solution.

I suggest adding a non-zero-value
#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::748 => : ERC721(nft).transferFrom(msg.sender, address(this), id);
2022-09-artgobblers/src/utils/GobblerReserve.sol::38 => artGobblers.transferFrom(address(this), to, ids[i]);
```



### [G16] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::31 => mapping(uint256 => address) internal _ownerOf;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::33 => mapping(address => uint256) internal _balanceOf;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::49 => mapping(uint256 => address) public getApproved;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::51 => mapping(address => mapping(address => bool)) public isApprovedForAll;

2022-09-artgobblers/src/utils/token/GobblersERC721.sol::44 => mapping(uint256 => GobblerData) public getGobblerData;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::59 => mapping(address => UserData) public getUserData;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::75 => mapping(uint256 => address) public getApproved;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::77 => mapping(address => mapping(address => bool)) public isApprovedForAll;

2022-09-artgobblers/src/utils/token/PagesERC721.sol::50 => mapping(uint256 => address) internal _ownerOf;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::52 => mapping(address => uint256) internal _balanceOf;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::68 => mapping(uint256 => address) public getApproved;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::70 => mapping(address => mapping(address => bool)) internal _isApprovedForAll;
```


### [G17] Using `bools` for storage incurs overhead

#### Impact

#### Findings:
```
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::51 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/ArtGobblers.sol::149 => mapping(address => bool) public hasClaimedMintlistGobbler;
2022-09-artgobblers/src/ArtGobblers.sol::212 => bool waitingForSeed;
2022-09-artgobblers/src/ArtGobblers.sol::727 => bool isERC1155
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::37 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::77 => mapping(address => mapping(address => bool)) public isApprovedForAll;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::70 => mapping(address => mapping(address => bool)) internal _isApprovedForAll;
```


### [G18] Using `private` rather than `public` for constants, saves gas

#### Impact
If needed, the value can be read from the verified contract source 
code. Savings are due to the compiler not having to create non-payable 
getter functions for deployment calldata, and not adding another entry 
to the method ID table
#### Findings:
```
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::13 => string public constant gobblerBaseUri = "https://testnet.ag.xyz/api/nfts/gobblers/";
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::14 => string public constant gobblerUnrevealedUri = "https://testnet.ag.xyz/api/nfts/unrevealed";
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::15 => string public constant pagesBaseUri = "https://testnet.ag.xyz/api/nfts/pages/";
2022-09-artgobblers/src/ArtGobblers.sol::112 => uint256 public constant MAX_SUPPLY = 10000;
2022-09-artgobblers/src/ArtGobblers.sol::115 => uint256 public constant MINTLIST_SUPPLY = 2000;
2022-09-artgobblers/src/ArtGobblers.sol::118 => uint256 public constant LEGENDARY_SUPPLY = 10;
2022-09-artgobblers/src/ArtGobblers.sol::122 => uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
2022-09-artgobblers/src/ArtGobblers.sol::126 => uint256 public constant MAX_MINTABLE = MAX_SUPPLY
2022-09-artgobblers/src/ArtGobblers.sol::177 => uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
2022-09-artgobblers/src/ArtGobblers.sol::180 => uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
2022-09-artgobblers/src/ArtGobblers.sol::184 => uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```



### [G19] Empty blocks should be removed or emit something

#### Impact
The code should be refactored such that they no longer exist, or the 
block should do something useful, such as emitting an event or 
reverting. If the contract is meant to be extended, the contract should 
be abstract and the function signatures be added without 
any default implementation. If the block is an empty if-statement block 
to avoid doing subsequent checks in the else-if/else conditions, the 
else-if/else conditions should be nested under the negation of the 
if-statement, because they involve different classes of checks, which 
may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})
#### Findings:
```
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::40 => {}
2022-09-artgobblers/solmate/src/utils/LibString.sol::30 => for { let temp := value } 1 {} {
```





### [G20] Remove or replace unused state variables

#### Impact
Saves a storage slot. If the variable is assigned a non-zero value, 
saves Gsset (20000 gas). If it’s assigned a zero value, saves Gsreset 
(2900 gas). If the variable remains unassigned, there is no gas savings 
unless the variable is public, in which case the 
compiler-generated non-payable getter deployment cost is saved. If the 
state variable is overriding an interface’s public function, mark the 
variable as constant or immutable so that it does not use a storage slot
#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::233 => event LegendaryGobblerMinted(address indexed user, uint256 indexed gobblerId, uint256[] burnedGobblerIds);
2022-09-artgobblers/src/ArtGobblers.sol::411 => function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) {
2022-09-artgobblers/src/utils/GobblerReserve.sol::34 => function withdraw(address to, uint256[] calldata ids) external onlyOwner {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::25 => uint256[] ids,
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::26 => uint256[] amounts
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::96 => uint256[] calldata ids,
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::97 => uint256[] calldata amounts,
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::101 => function balanceOfBatch(address[] calldata owners, uint256[] calldata ids)
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::105 => returns (uint256[] memory balances)
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::109 => balances = new uint256[](owners.length);
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::168 => uint256[] memory ids = new uint256[](amount);
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::169 => uint256[] memory amounts = new uint256[](amount);
```



### [G21] Multiplication/division by two should use bit shifting

#### Impact
<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas
#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::462 => cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```





### [G22] Optimize names to save gas

#### Impact
public/external function names and public member variable names can be optimized to save gas. See this
 link for an example of how it works. Below are the interfaces/abstract 
contracts that can be optimized so that the most frequently-called 
functions use the least amount of gas possible during method lookup. 
Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted
#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LinearVRGDA.sol::12 => abstract contract LinearVRGDA is VRGDA {
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::13 => abstract contract LogisticToLinearVRGDA is LogisticVRGDA {
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::12 => abstract contract LogisticVRGDA is VRGDA {
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::10 => abstract contract VRGDA {
2022-09-artgobblers/script/deploy/DeployBase.s.sol::16 => abstract contract DeployBase is Script {
2022-09-artgobblers/solmate/src/auth/Owned.sol::6 => abstract contract Owned {
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::6 => abstract contract ERC721 {
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::222 => abstract contract ERC721TokenReceiver {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::8 => abstract contract GobblersERC1155B {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::8 => abstract contract GobblersERC721 {
2022-09-artgobblers/src/utils/token/PagesERC721.sol::9 => abstract contract PagesERC721 {
```




