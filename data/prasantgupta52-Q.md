# 1. Avoid using Floating Pragma:
### Description:
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
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
# 2. Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom
### Description
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.
### Links to github files
[ArtGobblers.sol:L748](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L748)
[GobblerReserve.sol:L38](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L38)
### Instances
```
src/ArtGobblers.sol:748:            : ERC721(nft).transferFrom(msg.sender, address(this), id);
src/utils/GobblerReserve.sol:38:                artGobblers.transferFrom(address(this), to, ids[i]);
```
### Recommended Mitigation Steps
Consider using safeTransfer/safeTransferFrom or require() consistently.

----
# 3. _safeMint() should be used rather than _mint() wherever possible
### Description:
`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.
### Links to github files
[ArtGobblers.sol:L356](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L356)
[ArtGobblers.sol:L389](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L389)
[ArtGobblers.sol:L469](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L469)
[Goo.sol:L102](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Goo.sol#L102)
[Pages.sol:L211](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L211)
[Pages.sol:L251](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251)
[PagesERC721.sol:L173](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L173)
[GobblersERC1155B.sol:L135](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L135)
[GobblersERC721.sol:L160](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L160)
[ERC721.sol:L157](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L157)
[ERC721.sol:L194](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L194)
[ERC721.sol:L209](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L209)
### Instances
```
src/ArtGobblers.sol:356:        _mint(msg.sender, gobblerId);
src/ArtGobblers.sol:389:        _mint(msg.sender, gobblerId);
src/ArtGobblers.sol:469:            _mint(msg.sender, gobblerId);
src/Goo.sol:102:        _mint(to, amount);
src/Pages.sol:211:            _mint(msg.sender, pageId);
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
src/utils/token/PagesERC721.sol:173:    function _mint(address to, uint256 id) internal {
src/utils/token/GobblersERC1155B.sol:135:    function _mint(
src/utils/token/GobblersERC721.sol:160:    function _mint(address to, uint256 id) internal {
lib/solmate/src/tokens/ERC721.sol:157:    function _mint(address to, uint256 id) internal virtual {
lib/solmate/src/tokens/ERC721.sol:194:        _mint(to, id);
lib/solmate/src/tokens/ERC721.sol:209:        _mint(to, id);
```
### Recommendations:
Use _safeMint() instead of _mint().

----
# 4. Event is missing `indexed` fields
Impact - Non-Critical
Each `event` should use three `indexed` fields if there are three or more fields
### Links to github files
[ArtGobblers.sol:L232](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L232)
[ArtGobblers.sol:L233](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L233)
[ArtGobblers.sol:L234](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L234)
[ArtGobblers.sol:L240](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L240)
[Pages.sol:L134](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L134)
[Pages.sol:L136](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L136)
[PagesERC721.sol:L18](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L18)
[GobblersERC1155B.sol:L29](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L29)
[GobblersERC721.sol:L17](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L17)
[ERC721.sol:L15](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L15)
### Instances
```
src/ArtGobblers.sol:232:    event GobblerPurchased(address indexed user, uint256 indexed gobblerId, uint256 price);
src/ArtGobblers.sol:233:    event LegendaryGobblerMinted(address indexed user, uint256 indexed gobblerId, uint256[] burnedGobblerIds);
src/ArtGobblers.sol:234:    event ReservedGobblersMinted(address indexed user, uint256 lastMintedGobblerId, uint256 numGobblersEach);
src/ArtGobblers.sol:240:    event GobblersRevealed(address indexed user, uint256 numGobblers, uint256 lastRevealedId);
src/Pages.sol:134:    event PagePurchased(address indexed user, uint256 indexed pageId, uint256 price);
src/Pages.sol:136:    event CommunityPagesMinted(address indexed user, uint256 lastMintedPageId, uint256 numPages);
src/utils/token/PagesERC721.sol:18:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
src/utils/token/GobblersERC1155B.sol:29:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
src/utils/token/GobblersERC721.sol:17:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
lib/solmate/src/tokens/ERC721.sol:15:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
```
----
# 5. Use of Block.timestamp
Impact - Non-Critical
### Description
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
### Links to github files
[ArtGobblers.sol:L341](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L341)
[ArtGobblers.sol:L399](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L399)
[ArtGobblers.sol:L455](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L455)
[ArtGobblers.sol:L513](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L513)
[ArtGobblers.sol:L661](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L661)
[ArtGobblers.sol:L763](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L763)
[ArtGobblers.sol:L826](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L826)
[ArtGobblers.sol:L904](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L904)
[ArtGobblers.sol:L911](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L911)
[Pages.sol:L222](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L222)
### Instances
```
src/ArtGobblers.sol:341:        if (mintStart > block.timestamp) revert MintStartPending();
src/ArtGobblers.sol:399:        uint256 timeSinceStart = block.timestamp - mintStart;
src/ArtGobblers.sol:455:            getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
src/ArtGobblers.sol:513:        if (block.timestamp < nextRevealTimestamp) revert RequestTooEarly();
src/ArtGobblers.sol:661:                getUserData[currentIdOwner].lastTimestamp = uint64(block.timestamp);
src/ArtGobblers.sol:763:            uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
src/ArtGobblers.sol:826:        getUserData[user].lastTimestamp = uint64(block.timestamp);
src/ArtGobblers.sol:904:            getUserData[from].lastTimestamp = uint64(block.timestamp);
src/ArtGobblers.sol:911:            getUserData[to].lastTimestamp = uint64(block.timestamp);
src/Pages.sol:222:        uint256 timeSinceStart = block.timestamp - mintStart;
```

### Recommended Mitigation Steps

Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.

----
# 6. Variable names that consist of all capital letters should be reserved for const/immutable variables
### Description
If the variable needs to be different based on which class it comes from, a `view`/`pure` function should be used instead
### Links to github files
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
[ChainlinkV1RandProvider.sol:L27](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L27)
[ChainlinkV1RandProvider.sol:L30](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L30)
[GobblerReserve.sol:L18](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L18)
[PagesERC721.sol:L34](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L34)
[DeployRinkeby.s.sol:L7](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L7)
[DeployRinkeby.s.sol:L9](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L9)
[DeployRinkeby.s.sol:L11](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L11)
[DeployBase.s.sol:L18](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L18)
[DeployBase.s.sol:L19](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L19)
[DeployBase.s.sol:L20](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L20)
[DeployBase.s.sol:L21](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L21)
[DeployBase.s.sol:L22](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L22)
[DeployBase.s.sol:L23](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L23)
[DeployBase.s.sol:L24](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployBase.s.sol#L24)
[LogisticToLinearVRGDA.sol:L20](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticToLinearVRGDA.sol#L20)
[LogisticToLinearVRGDA.sol:L24](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticToLinearVRGDA.sol#L24)
[LogisticToLinearVRGDA.sol:L28](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticToLinearVRGDA.sol#L28)
[LogisticVRGDA.sol:L20](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L20)
[LogisticVRGDA.sol:L25](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L25)
[LogisticVRGDA.sol:L30](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L30)
[LinearVRGDA.sol:L19](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol#L19)
[VRGDA.sol:L17](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L17)
[VRGDA.sol:L21](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L21)

### Instances
```
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
src/utils/rand/ChainlinkV1RandProvider.sol:27:    bytes32 internal immutable chainlinkKeyHash;
src/utils/rand/ChainlinkV1RandProvider.sol:30:    uint256 internal immutable chainlinkFee;
src/utils/GobblerReserve.sol:18:    ArtGobblers public immutable artGobblers;
src/utils/token/PagesERC721.sol:34:    ArtGobblers public immutable artGobblers;
script/deploy/DeployRinkeby.s.sol:7:    address public immutable coldWallet = 0x126620598A797e6D9d2C280b5dB91b46F27A8330;
script/deploy/DeployRinkeby.s.sol:9:    address public immutable root = 0x1D18077167c1177253555e45B4b5448B11E30b4b;
script/deploy/DeployRinkeby.s.sol:11:    uint256 public immutable mintStart = 1656369768;
script/deploy/DeployBase.s.sol:18:    address private immutable teamColdWallet;
script/deploy/DeployBase.s.sol:19:    bytes32 private immutable merkleRoot;
script/deploy/DeployBase.s.sol:20:    uint256 private immutable mintStart;
script/deploy/DeployBase.s.sol:21:    address private immutable vrfCoordinator;
script/deploy/DeployBase.s.sol:22:    address private immutable linkToken;
script/deploy/DeployBase.s.sol:23:    bytes32 private immutable chainlinkKeyHash;
script/deploy/DeployBase.s.sol:24:    uint256 private immutable chainlinkFee;
lib/VRGDAs/src/LogisticToLinearVRGDA.sol:20:    int256 internal immutable soldBySwitch;
lib/VRGDAs/src/LogisticToLinearVRGDA.sol:24:    int256 internal immutable switchTime;
lib/VRGDAs/src/LogisticToLinearVRGDA.sol:28:    int256 internal immutable perTimeUnit;
lib/VRGDAs/src/LogisticVRGDA.sol:20:    int256 public immutable logisticLimit;
lib/VRGDAs/src/LogisticVRGDA.sol:25:    int256 public immutable logisticLimitDoubled;
lib/VRGDAs/src/LogisticVRGDA.sol:30:    int256 internal immutable timeScale;
lib/VRGDAs/src/LinearVRGDA.sol:19:    int256 internal immutable perTimeUnit;
lib/VRGDAs/src/VRGDA.sol:17:    int256 public immutable targetPrice;
lib/VRGDAs/src/VRGDA.sol:21:    int256 internal immutable decayConstant;
```
----