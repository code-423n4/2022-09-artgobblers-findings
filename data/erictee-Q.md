### [L-01] Avoid floating pragmas for non-library contracts.


#### Impact
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### Findings:
```
src/ArtGobblers.sol:L2     pragma solidity >=0.8.0;

src/Pages.sol:L2     pragma solidity >=0.8.0;

src/Goo.sol:L2     pragma solidity >=0.8.0;

src/utils/GobblerReserve.sol:L2     pragma solidity >=0.8.0;

src/utils/token/GobblersERC721.sol:L2     pragma solidity >=0.8.0;

src/utils/token/GobblersERC1155B.sol:L2     pragma solidity >=0.8.0;

src/utils/token/PagesERC721.sol:L2     pragma solidity >=0.8.0;

src/utils/rand/ChainlinkV1RandProvider.sol:L2     pragma solidity >=0.8.0;

src/utils/rand/RandProvider.sol:L2     pragma solidity >=0.8.0;

```
### [L-02] ```_safemint()``` should be used rather than ```_mint()``` wherever possible


#### Impact
```_mint()``` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of ```_safeMint()``` which ensures that the recipient is either an EOA or implements ```IERC721Receiver```. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function


#### Findings:
```
src/ArtGobblers.sol:L356        _mint(msg.sender, gobblerId);

src/ArtGobblers.sol:L389        _mint(msg.sender, gobblerId);

src/ArtGobblers.sol:L469            _mint(msg.sender, gobblerId);

src/Pages.sol:L211            _mint(msg.sender, pageId);

src/Pages.sol:L251            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/Goo.sol:L102        _mint(to, amount);

src/utils/token/GobblersERC721.sol:L160    function _mint(address to, uint256 id) internal {

src/utils/token/GobblersERC1155B.sol:L135    function _mint(

src/utils/token/PagesERC721.sol:L173    function _mint(address to, uint256 id) internal {

```

### [L-08] zero-address checks are missing


#### Impact
Zero-address checks are a best practice for input validation of critical address parameters. Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

#### Findings:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316-L317
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83-L84
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#l181