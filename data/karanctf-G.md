## [G-1] Use unchecked preincrement to save gas
```solidity
GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
## [G-2] Cache .length in loops

```solidity
GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
```

## [G-3] Donot use default values Explicit initialization with zero is not required for variable declaration of numTokens because `uints are 0` by default.removeing this will reduce contract size and save a bit of gas.

```solidity
ArtGobblers.sol:432:            for (uint256 i = 0; i < cost; ++i) {
ArtGobblers.sol:592:            for (uint256 i = 0; i < numGobblers; ++i) {
GobblersERC721.sol:186:            for (uint256 i = 0; i < amount; ++i) {
GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
GobblersERC1155B.sol:173:            for (uint256 i = 0; i < amount; ++i) {
Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```


## [G-4] Variable == false|0 -> !variable or variable ==  true -> variable
```solidity
tGobblers.sol:528:            if (toBeRevealed == 0) revert ZeroToBeRevealed();
ArtGobblers.sol:615:                uint64 swapIndex = getGobblerData[swapId].idx == 0
ArtGobblers.sol:623:                uint64 currentIndex = getGobblerData[currentId].idx == 0
ArtGobblers.sol:696:            if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.
GobblersERC721.sol:122:            to.code.length == 0 ||
GobblersERC721.sol:138:            to.code.length == 0 ||
ERC721.sol:119:            to.code.length == 0 ||
ERC721.sol:135:            to.code.length == 0 ||
ERC721.sol:197:            to.code.length == 0 ||
ERC721.sol:212:            to.code.length == 0 ||
Pages.sol:266:        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```

## [G-05] Use `!= 0` instead of `> 0`
```solidity
SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
```
## [G-06]` <x> += <y>` costs more gas than `<x> = <x> + <y>` 
```solidity
SignedWadMath.sol:201:        r *= 1677202110996718588342820967067443963516166;
SignedWadMath.sol:203:        r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
SignedWadMath.sol:205:        r += 600920179829731861736702779321621459595472258049074101567377883020018308;
ArtGobblers.sol:439:                burnedMultipleTotal += getGobblerData[id].emissionMultiple;
ArtGobblers.sol:456:            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
ArtGobblers.sol:464:            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
ArtGobblers.sol:662:                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
ArtGobblers.sol:844:            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
ArtGobblers.sol:905:            getUserData[from].emissionMultiple -= emissionMultiple;
ArtGobblers.sol:906:            getUserData[from].gobblersOwned -= 1;
ArtGobblers.sol:912:            getUserData[to].emissionMultiple += emissionMultiple;
ArtGobblers.sol:913:            getUserData[to].gobblersOwned += 1;
GobblersERC721.sol:184:            getUserData[to].gobblersOwned += uint32(amount);
Pages.sol:244:            uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);
```


