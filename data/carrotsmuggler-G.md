# 1. ++i is more efficient than i++ (saves ~6 gas)

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

```solidity
for (uint256 i = 0; i < numPages; i++)
```
should be
```solidity
for (uint256 i = 0; i < numPages; ++i)
```