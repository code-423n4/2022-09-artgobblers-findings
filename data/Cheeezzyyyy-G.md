## 1. ++i COSTS 5 GAS LESS THAN i++

For a `uint256 i` variable, `++i` cost 5 less gas than `i++` . Consider replacing `i++` instances with `++i` in the following lines:

- [src/Pages.sol#L251](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251)


## 2. USAGE OF BOOLS INCURS OVERHEAD COST - USE UINT FOR GAS SAVINGS

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and  pointer aliasing, and it cannot be disabled.

Use `uint256(1)` and `uint256(2)` to replace true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD.

Instances found: 

- [src/ArtGobblers.sol#L149](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L149)

## 3. MORE REQUIRE STATEMENTS INSTEAD OF ‘||’ CONDITIONAL CHECKS

Require statements including conditions with the || operator can be broken down in multiple require statements to save gas.

Instances found:

- [src/utils/token/PagesERC721.sol#L85](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L85)
- [src/utils/token/PagesERC721.sol#L107-L110](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L107-L110)
- [src/utils/token/GlobblersERC721.sol#L95](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L95)
- [src/utils/token/GlobbersERC721.sol#L121-L137](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L121-L127)
- [src/utils/token/GlobbersERC721.sol#L137-L142](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L137-L142)


