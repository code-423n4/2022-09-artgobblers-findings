## G001 `<ARRAY>.LENGTH` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A `FOR-LOOP`

Reading array length at each iteration of the loop consumes more gas than necessary.

In the best case scenario (length read on a memory variable), caching the array length in the stack saves around 3 gas per iteration. In the worst case scenario (external calls at each iteration), the amount of gas wasted can be massive.

Here, Consider storing the array’s length in a variable before the for-loop, and use this new variable instead:

```solidity
for (uint256 i = 0; i < ids.length; i++) {
```

[https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37)

```solidity
for (uint256 i = 0; i < owners.length; ++i) {
```

[https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114)

### **Background Information**

- [https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#caching-the-length-in-for-loops](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#caching-the-length-in-for-loops)