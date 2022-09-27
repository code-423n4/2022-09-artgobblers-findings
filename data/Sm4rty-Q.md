
## _safeMint() should be used rather than _mint() wherever possible

`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

### Instances

```
src/Goo.sol:102:        _mint(to, amount);
src/ArtGobblers.sol:356:        _mint(msg.sender, gobblerId);
src/ArtGobblers.sol:389:        _mint(msg.sender, gobblerId);
src/ArtGobblers.sol:469:            _mint(msg.sender, gobblerId);
src/Pages.sol:211:            _mint(msg.sender, pageId);
src/Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
lib/solmate/src/tokens/ERC721.sol:194:        _mint(to, id);
lib/solmate/src/tokens/ERC721.sol:209:        _mint(to, id);
``` 
### Recommendations:

Use _safeMint() instead of _mint().


-----

## Avoid using Floating Pragma:

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

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
Recommend using fixed solidity version

### References:

[https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity)


-----


