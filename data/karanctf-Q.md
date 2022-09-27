# Non critical 

## [N-01] Use scientific notation (e.g. `10e18`) rather than exponentiation (e.g. `10**18`)
```solidity
SignedWadMath.sol:144:        // We want to convert x from 10**18 fixed point to 2**96 fixed point.
SignedWadMath.sol:145:        // We do this by multiplying by 2**96 / 10**18. But since
SignedWadMath.sol:147:        // and add ln(2**96 / 10**18) at the end.
SignedWadMath.sol:196:        // * add ln(2**96 / 10**18)
SignedWadMath.sol:198:        // * multiply by 10**18 / 2**96 = 5**18 >> 78
SignedWadMath.sol:204:        // add ln(2**96 / 10**18) * 5e18 * 2**192
```

# Low

## [L -1]Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Reference: This [similar medium-severity finding](https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call) from Consensys Diligence Audit of Fei Protocol.
```solidity
PagesERC721.sol:98:    function transferFrom(
PagesERC721.sol:132:        transferFrom(from, to, id);
PagesERC721.sol:148:        transferFrom(from, to, id);
ArtGobblers.sol:748:            : ERC721(nft).transferFrom(msg.sender, address(this), id);
ArtGobblers.sol:880:    function transferFrom(
GobblersERC721.sol:108:    function transferFrom(
GobblersERC721.sol:119:        transferFrom(from, to, id);
GobblersERC721.sol:135:        transferFrom(from, to, id);
GobblerReserve.sol:32:    /// @param to The address to transfer the gobblers to.
GobblerReserve.sol:33:    /// @param ids The ids of the gobblers to transfer.
GobblerReserve.sol:38:                artGobblers.transferFrom(address(this), to, ids[i]);
ERC721.sol:82:    function transferFrom(
ERC721.sol:116:        transferFrom(from, to, id);
ERC721.sol:132:        transferFrom(from, to, id);
```

## [L-2] USE SAFEERC20.SAFEAPPROVE INSTEAD OF APPROVE
This is probably an oversight since SafeERC20 was imported and safeTransfer() was used for ERC20 token transfers. Nevertheless, note that approve() will fail for certain token implementations that do not return a boolean value (). Hence it is recommend to use safeApprove().
```solidity
PagesERC721.sol:18:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
PagesERC721.sol:82:    function approve(address spender, uint256 id) external {
PagesERC721.sol:92:    function setApprovalForAll(address operator, bool approved) external {
PagesERC721.sol:93:        _isApprovedForAll[msg.sender][operator] = approved;
PagesERC721.sol:95:        emit ApprovalForAll(msg.sender, operator, approved);
GobblersERC721.sol:17:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
GobblersERC721.sol:92:    function approve(address spender, uint256 id) external {
GobblersERC721.sol:102:    function setApprovalForAll(address operator, bool approved) external {
GobblersERC721.sol:103:        isApprovedForAll[msg.sender][operator] = approved;
GobblersERC721.sol:105:        emit ApprovalForAll(msg.sender, operator, approved);
GobblersERC1155B.sol:29:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
GobblersERC1155B.sol:79:    function setApprovalForAll(address operator, bool approved) public virtual {
GobblersERC1155B.sol:80:        isApprovedForAll[msg.sender][operator] = approved;
GobblersERC1155B.sol:82:        emit ApprovalForAll(msg.sender, operator, approved);
ERC721.sol:15:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
ERC721.sol:66:    function approve(address spender, uint256 id) public virtual {
ERC721.sol:76:    function setApprovalForAll(address operator, bool approved) public virtual {
ERC721.sol:77:        isApprovedForAll[msg.sender][operator] = approved;
ERC721.sol:79:        emit ApprovalForAll(msg.sender, operator, approved);
```

