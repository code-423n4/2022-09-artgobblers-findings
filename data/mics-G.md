Table Of Content
================

* [GAS REPORT](#gas-report)
        * [Caching array size](#caching-array-size)
        * [Use custom errors](#use-custom-errors)

# GAS REPORT

## Caching array size
In the following for loops consider caching the array size instead of loading it every iteration.

### Code Instances:
- [GobblerReserve.sol#L36](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L36)
- [ArtGobblers.t.sol#L446](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/ArtGobblers.t.sol#L446)

## Use custom errors
In the following require statements you can use custom errors to save gas and improve code quality.

### Code Instances:
- [GobblersERC1155B.sol#L184](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L184)
- [GobblersERC1155B.sol#L148](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L148)
- [GobblersERC1155B.sol#L106](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L106)
- [PagesERC721.sol#L84](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L84)
