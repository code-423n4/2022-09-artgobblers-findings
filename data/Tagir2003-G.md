# GAS REPORT

## [GAS] Consider using custom errors
You can utilize customized errors in require statements to save gas.

### Proof of concept:
- [PagesERC721.sol#L54](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L54)
- [PagesERC721.sol#L150](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L150)

## [GAS] Cache the following arrays size before the loop


### Proof of concept:
- [GobblersERC1155B.sol#L113](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L113)
- [GobblerReserve.sol#L36](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L36)

## [GAS] Caching msg.sender is unnecessary


### Proof of concept:
- [ArtGobblers.t.sol#L711](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/ArtGobblers.t.sol#L711)
- [Optimizations.t.sol#L30](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Optimizations.t.sol#L30)
