## make require into error to save gas 
```
utils/token/GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
utils/token/GobblersERC1155B.sol:149:            require(
utils/token/GobblersERC1155B.sol:185:            require(
utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
utils/token/PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
utils/token/PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
utils/token/PagesERC721.sol:107:        require(
utils/token/PagesERC721.sol:135:            require(
utils/token/PagesERC721.sol:151:            require(
utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
utils/token/GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
utils/token/GobblersERC721.sol:121:        require(
utils/token/GobblersERC721.sol:137:        require(
ArtGobblers.sol:437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
ArtGobblers.sol:885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
ArtGobblers.sol:889:        require(
```
##  make address(0) into long form:ex 0x00000000000000
```
ArtGobblers.sol:441:                emit Transfer(msg.sender, getGobblerData[id].owner = address(0), id);
ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
```
## make i++ into ++i to save gas 
```
Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
```
## make array.length into memory varaible
```
utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {

```
## make onlyOwner functions payable to save gas 
```
utils/GobblerReserve.sol:34:    function withdraw(address to, uint256[] calldata ids) external onlyOwner {
ArtGobblers.sol:560:    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
```
## make i into unchecked to save gas
```
utils/token/GobblersERC1155B.sol:114:            for (uint256 i = 0; i < owners.length; ++i) {
utils/GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {

```