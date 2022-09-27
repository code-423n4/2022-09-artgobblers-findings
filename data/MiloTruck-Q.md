# Non-Critical Issues

|No.|Issue|Instances|
|:-|:-|:-:|
|1|Floating compiler versions|3|
|2|`constants` should be defined rather than using magic numbers|23|
|3|`event` is missing `indexed` fields|10|

Total: **36** instances over **3** issues

## Floating compiler versions
Non-library/interface files should use fixed compiler versions, not floating ones.

_There are **3** instances of this issue:_  
```solidity
src/Goo.sol:
   2:        pragma solidity >=0.8.0;

src/Pages.sol:
   2:        pragma solidity >=0.8.0;

src/ArtGobblers.sol:
   2:        pragma solidity >=0.8.0;
```

## `constants` should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) code can benefit from using readable constants instead of hex/numeric literals.

_There are **23** instances of this issue:_  
`4`:
```solidity
script/deploy/DeployBase.s.sol:
  66:        address gobblerAddress = LibRLP.computeAddress(tx.origin, vm.getNonce(tx.origin) + 4);
```
`5`:
```solidity
script/deploy/DeployBase.s.sol:
  67:        address pageAddress = LibRLP.computeAddress(tx.origin, vm.getNonce(tx.origin) + 5);
```
`1 days`:
```solidity
src/ArtGobblers.sol:
 535:        gobblerRevealsData.nextRevealTimestamp = uint64(nextRevealTimestamp + 1 days);
```
`9`:
```solidity
src/ArtGobblers.sol:
 632:        uint256 newCurrentIdMultiple = 9; // For beyond 7963.
```
`5`:
```solidity
src/ArtGobblers.sol:
 848:        if (newNumMintedForReserves > (numMintedFromGoo + newNumMintedForReserves) / 5) revert ReserveImbalance();
```
`1 days`:
```solidity
src/ArtGobblers.sol:
 327:        gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days);
```
`0x01ffc9a7`:
```solidity
src/utils/token/PagesERC721.sol:
 164:        interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
```
`0x80ac58cd`:
```solidity
src/utils/token/PagesERC721.sol:
 165:        interfaceId == 0x80ac58cd || // ERC165 Interface ID for ERC721
```
`0x5b5e139f`:
```solidity
src/utils/token/PagesERC721.sol:
 166:        interfaceId == 0x5b5e139f; // ERC165 Interface ID for ERC721Metadata
```
`0x01ffc9a7`:
```solidity
src/utils/token/GobblersERC721.sol:
 151:        interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
```
`0x80ac58cd`:
```solidity
src/utils/token/GobblersERC721.sol:
 152:        interfaceId == 0x80ac58cd || // ERC165 Interface ID for ERC721
```
`0x5b5e139f`:
```solidity
src/utils/token/GobblersERC721.sol:
 153:        interfaceId == 0x5b5e139f; // ERC165 Interface ID for ERC721Metadata
```
`0x01ffc9a7`:
```solidity
src/utils/token/GobblersERC1155B.sol:
 126:        interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
```
`0xd9b67a26`:
```solidity
src/utils/token/GobblersERC1155B.sol:
 127:        interfaceId == 0xd9b67a26 || // ERC165 Interface ID for ERC1155
```
`0x0e89341c`:
```solidity
src/utils/token/GobblersERC1155B.sol:
 128:        interfaceId == 0x0e89341c; // ERC165 Interface ID for ERC1155MetadataURI
```
`0x01ffc9a7`:
```solidity
lib/solmate/src/tokens/ERC721.sol:
 148:        interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
```
`0x80ac58cd`:
```solidity
lib/solmate/src/tokens/ERC721.sol:
 149:        interfaceId == 0x80ac58cd || // ERC165 Interface ID for ERC721
```
`0x5b5e139f`:
```solidity
lib/solmate/src/tokens/ERC721.sol:
 150:        interfaceId == 0x5b5e139f; // ERC165 Interface ID for ERC721Metadata
```
`1e18`:
```solidity
lib/VRGDAs/src/LogisticVRGDA.sol:
  62:        return -unsafeWadDiv(wadLn(unsafeDiv(logisticLimitDoubled, sold + logisticLimit) - 1e18), timeScale);
```
`1e18`:
```solidity
lib/VRGDAs/src/LogisticVRGDA.sol:
  44:        logisticLimit = _maxSellable + 1e18;
```
`2e18`:
```solidity
lib/VRGDAs/src/LogisticVRGDA.sol:
  47:        logisticLimitDoubled = logisticLimit * 2e18;
```
`1e18`:
```solidity
lib/VRGDAs/src/VRGDA.sol:
  29:        decayConstant = wadLn(1e18 - _priceDecayPercent);
```
`1e18`:
```solidity
lib/goo-issuance/src/LibGOO.sol:
  38:        (emissionMultiple * lastBalanceWad * 1e18).sqrt()
```

## `event` is missing `indexed` fields
Each `event` should use three `indexed` fields if there are three or more fields.
  
_There are **10** instances of this issue:_  
```solidity
src/Pages.sol:
 134:        event PagePurchased(address indexed user, uint256 indexed pageId, uint256 price);
 136:        event CommunityPagesMinted(address indexed user, uint256 lastMintedPageId, uint256 numPages);

src/ArtGobblers.sol:
 232:        event GobblerPurchased(address indexed user, uint256 indexed gobblerId, uint256 price);
 233:        event LegendaryGobblerMinted(address indexed user, uint256 indexed gobblerId, uint256[] burnedGobblerIds);
 234:        event ReservedGobblersMinted(address indexed user, uint256 lastMintedGobblerId, uint256 numGobblersEach);
 240:        event GobblersRevealed(address indexed user, uint256 numGobblers, uint256 lastRevealedId);

src/utils/token/PagesERC721.sol:
  18:        event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

src/utils/token/GobblersERC721.sol:
  17:        event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

src/utils/token/GobblersERC1155B.sol:
  29:        event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

lib/solmate/src/tokens/ERC721.sol:
  15:        event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
```
