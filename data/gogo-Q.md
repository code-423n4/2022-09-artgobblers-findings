# 2022-09-ARTGOBBLERS
## Low Risk and Non-Critical Issues

### Events not emmited on important state changes
Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

_There are **1** instances of this issue:_

```solidity
File: script/deploy/DeployBase.s.sol

61:   function run() external {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol

### `public` functions not called by the contract should be declared `external` instead


_There are **9** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

473:        /// @notice Calculate the legendary gobbler price in terms of gobblers, according to a linear decay function.

693:  function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

265:  function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

55:   function ownerOf(uint256 id) public view virtual returns (address owner) {

73:   function uri(uint256 id) public view virtual returns (string memory);

79:   function setApprovalForAll(address operator, bool approved) public virtual {

85:   function safeTransferFrom(

93:   function safeBatchTransferFrom(

124:  function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

### Non-library/interface files should use fixed compiler versions, not floating ones


_There are **10** instances of this issue:_

```solidity
File: script/deploy/DeployBase.s.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol

```solidity
File: script/deploy/DeployRinkeby.s.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployRinkeby.s.sol

```solidity
File: src/ArtGobblers.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Goo.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol

```solidity
File: src/Pages.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/GobblerReserve.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol

```solidity
File: src/utils/rand/ChainlinkV1RandProvider.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

```solidity
File: src/utils/token/GobblersERC721.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

```solidity
File: src/utils/token/PagesERC721.sol

2:    pragma solidity >=0.8.0;
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol

### Natspec is missing


_There are **2** instances of this issue:_

```solidity
File: script/deploy/DeployBase.s.sol

      /// @audit
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol

```solidity
File: script/deploy/DeployRinkeby.s.sol

      /// @audit
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployRinkeby.s.sol

### Event is missing `indexed` fields


_There are **9** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

event      GobblerPurchased(address indexed user, uint256 indexed gobblerId, uint256 price)

event      LegendaryGobblerMinted(address indexed user, uint256 indexed gobblerId, uint256[] burnedGobblerIds)

event      ReservedGobblersMinted(address indexed user, uint256 lastMintedGobblerId, uint256 numGobblersEach)

event      GobblersRevealed(address indexed user, uint256 numGobblers, uint256 lastRevealedId)
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

event      PagePurchased(address indexed user, uint256 indexed pageId, uint256 price)

event      CommunityPagesMinted(address indexed user, uint256 lastMintedPageId, uint256 numPages)
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol

```solidity
File: src/utils/token/GobblersERC1155B.sol

event      ApprovalForAll(address indexed owner, address indexed operator, bool approved)
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol

```solidity
File: src/utils/token/GobblersERC721.sol

event      ApprovalForAll(address indexed owner, address indexed operator, bool approved)
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol

```solidity
File: src/utils/token/PagesERC721.sol

event      ApprovalForAll(address indexed owner, address indexed operator, bool approved)
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol

### Not used import


_There are **3** instances of this issue:_

```solidity
File: script/deploy/DeployRinkeby.s.sol

4:    import {DeployBase} from "./DeployBase.s.sol";
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployRinkeby.s.sol

```solidity
File: src/ArtGobblers.sol

68:   import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol

```solidity
File: src/Pages.sol

5:    import {toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol
