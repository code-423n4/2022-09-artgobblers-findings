### [G-01] Using bools for storage incurs overhead.


#### Impact
 
```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 Use ```uint256(1)``` and ```uint256(2)``` for true/false to avoid a Gwarmaccess ([100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past


#### Findings:
```
src/ArtGobblers.sol:L149    mapping(address => bool) public hasClaimedMintlistGobbler;

src/utils/token/GobblersERC721.sol:L77    mapping(address => mapping(address => bool)) public isApprovedForAll;

src/utils/token/GobblersERC1155B.sol:L37    mapping(address => mapping(address => bool)) public isApprovedForAll;

src/utils/token/PagesERC721.sol:L70    mapping(address => mapping(address => bool)) internal _isApprovedForAll;

```

### [G-02] Cache Array Length Outside of Loop


#### Impact
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.


#### Findings:
```
src/utils/GobblerReserve.sol:L37            for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC1155B.sol:L114            for (uint256 i = 0; i < owners.length; ++i) {

```

### [G-03] Use custom errors rather than ```revert()```/```require()``` string to save gas


#### Impact
Custom errors are available from solidity version 0.8.4, it saves around 50 gas each time they are hit by avoiding having to allocate and store the revert string.


#### Findings:
```
src/ArtGobblers.sol:L437                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

src/ArtGobblers.sol:L696            if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.

src/ArtGobblers.sol:L705        if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");

src/ArtGobblers.sol:L711        revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.

src/ArtGobblers.sol:L885        require(from == getGobblerData[id].owner, "WRONG_FROM");

src/ArtGobblers.sol:L887        require(to != address(0), "INVALID_RECIPIENT");

src/Pages.sol:L266        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

src/utils/token/GobblersERC721.sol:L62        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

src/utils/token/GobblersERC721.sol:L66        require(owner != address(0), "ZERO_ADDRESS");

src/utils/token/GobblersERC721.sol:L95        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

src/utils/token/GobblersERC1155B.sol:L107        require(owners.length == ids.length, "LENGTH_MISMATCH");

src/utils/token/PagesERC721.sol:L55        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

src/utils/token/PagesERC721.sol:L59        require(owner != address(0), "ZERO_ADDRESS");

src/utils/token/PagesERC721.sol:L85        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");

src/utils/token/PagesERC721.sol:L103        require(from == _ownerOf[id], "WRONG_FROM");

src/utils/token/PagesERC721.sol:L105        require(to != address(0), "INVALID_RECIPIENT");

```

### [G-04] Use a more recent version of solidity.


#### Impact
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining 
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads 
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.


#### Findings:
```
src/ArtGobblers.sol:L2      pragma solidity >=0.8.0;

src/Pages.sol:L2      pragma solidity >=0.8.0;

src/Goo.sol:L2      pragma solidity >=0.8.0;

src/utils/GobblerReserve.sol:L2      pragma solidity >=0.8.0;

src/utils/token/GobblersERC721.sol:L2      pragma solidity >=0.8.0;

src/utils/token/GobblersERC1155B.sol:L2      pragma solidity >=0.8.0;

src/utils/token/PagesERC721.sol:L2      pragma solidity >=0.8.0;

src/utils/rand/ChainlinkV1RandProvider.sol:L2      pragma solidity >=0.8.0;

src/utils/rand/RandProvider.sol:L2      pragma solidity >=0.8.0;

```
### [G-05] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
src/ArtGobblers.sol:L560    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {

src/Goo.sol:L101    function mintForGobblers(address to, uint256 amount) external only(artGobblers) {

src/Goo.sol:L108    function burnForGobblers(address from, uint256 amount) external only(artGobblers) {

src/Goo.sol:L115    function burnForPages(address from, uint256 amount) external only(pages) {

src/utils/GobblerReserve.sol:L34    function withdraw(address to, uint256[] calldata ids) external onlyOwner {

```
### [G-06] ```X += Y``` costs more gas than ```X = X + Y``` for state variables.


#### Impact
Consider changing ```X += Y``` to ```X = X + Y``` to save gas.


#### Findings:
```
src/ArtGobblers.sol:L439                burnedMultipleTotal += getGobblerData[id].emissionMultiple;

src/ArtGobblers.sol:L456            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.

src/ArtGobblers.sol:L464            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.

src/ArtGobblers.sol:L662                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);

src/ArtGobblers.sol:L844            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);

src/ArtGobblers.sol:L912            getUserData[to].emissionMultiple += emissionMultiple;

src/ArtGobblers.sol:L913            getUserData[to].gobblersOwned += 1;

src/Pages.sol:L244            uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);

src/utils/token/GobblersERC721.sol:L184            getUserData[to].gobblersOwned += uint32(amount);

```

### [G-07] ++i costs less gas than i++, especially when it's used in for loops.


#### Impact
Saves 6 gas per loop.


#### Findings:
```
src/Pages.sol:L251            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/utils/GobblerReserve.sol:L37            for (uint256 i = 0; i < ids.length; i++) {

```
### [G-08] Using private rather than public for constants to saves gas.


#### Impact
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.


#### Findings:
```
src/ArtGobblers.sol:L112    uint256 public constant MAX_SUPPLY = 10000;

src/ArtGobblers.sol:L115    uint256 public constant MINTLIST_SUPPLY = 2000;

src/ArtGobblers.sol:L118    uint256 public constant LEGENDARY_SUPPLY = 10;

src/ArtGobblers.sol:L122    uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;

src/ArtGobblers.sol:L126    uint256 public constant MAX_MINTABLE = MAX_SUPPLY

src/ArtGobblers.sol:L177    uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;

src/ArtGobblers.sol:L180    uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;

src/ArtGobblers.sol:L184    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);

```

### [G-09] Explicit initialization with zero not required


#### Impact
Explicit initialization with zero is not required for variable declaration because uints are 0 by default. Removing this will reduce contract size and save a bit of gas.


#### Findings:
```
src/ArtGobblers.sol:L432            for (uint256 i = 0; i < cost; ++i) {

src/ArtGobblers.sol:L592            for (uint256 i = 0; i < numGobblers; ++i) {

src/Pages.sol:L251            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/utils/GobblerReserve.sol:L37            for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC721.sol:L186            for (uint256 i = 0; i < amount; ++i) {

src/utils/token/GobblersERC1155B.sol:L114            for (uint256 i = 0; i < owners.length; ++i) {

src/utils/token/GobblersERC1155B.sol:L173            for (uint256 i = 0; i < amount; ++i) {

```
