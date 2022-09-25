## `++i` COSTS LESS GAS COMPARED TO `i++` OR `i += 1`  
  
`++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.  
  
`i++` increments `i` and returns the initial value of `i`. Which means:  
  
`uint i = 1;  
i++; // == 1 but i == 2  `  
But `++i` returns the actual incremented value:  
  
`uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable `  
In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`  
  
Instance includes:  
  
[`src/Pages.sol:251`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)
[`src/utils/GobblerReserve.sol:37`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
I suggest using ++i instead of i++ to increment the value of a uint variable.
___
## Cache array length in for loops can save gas  
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.  
Caching the array length in the stack saves around 3 gas per iteration.  
  
Instances include:  
[`src/utils/GobblerReserve.sol:37`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
___
## Not initializing `unit` variable to default value of zero  
  
Instance includes:
[`src/Pages.sol:251`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)
[`src/utils/GobblerReserve.sol:37`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
[`src/utils/token/GobblersERC721.sol:186`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186)