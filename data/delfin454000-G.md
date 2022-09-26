
### Array length should not be looked up in every iteration of a `for` loop
Since calculating the array length costs gas, it's best to read the length of the array from memory before executing the loop, as demonstrated below:

[GobblersERC1155B.sol: L114-116](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114-L116)
```solidity
            for (uint256 i = 0; i < owners.length; ++i) {
                balances[i] = balanceOf(owners[i], ids[i]);
            }
```
Suggestion:
```solidity
            uint256 totalOwnersLength = owners.length; 
            for (uint256 i = 0; i < totalOwnersLength; ++i) {
                balances[i] = balanceOf(owners[i], ids[i]);
            }
```
___
Similarly for the following `for` loop:

[GobblerReserve.sol: L37-39](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37-L39)
___
___


### Use `++i` instead of `i++` to increase count in a `for` loop
Since use of  `i++` (or equivalent counter) costs more gas, it is better to use `++i` in the `for` loops below:

[Pages.sol: L250-255](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L250-L255)

[GobblerReserve.sol: L37-39](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37-L39)
___
___
