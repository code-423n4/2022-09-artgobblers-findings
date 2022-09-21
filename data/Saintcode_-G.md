## USE CALLDATA INSTEAD OF MEMORY

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

When arguments are read-only on external functions, the data location should be `calldata`

2 Instances:
-GobblersERC1155B.sol lines 138, 161


## <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

Reading array length at each iteration of the loop consumes more gas than necessary.
In the best case scenario (length read on a memory variable), caching the array length in the stack saves around 3 gas per iteration. In the worst case scenario (external calls at each iteration), the amount of gas wasted can be massive.

Consider storing the array’s length in a variable before the for-loop, and use this new variable instead

2 Instances: 
-GobblerReserve.sol line 38
-GobblersERC1155B.sol line 114


## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

8 Instances:
-ArtGobblers.sol lines 456, 464, 662, 912, 913, 905, 906
-GobblersERC721.sol line 187



## `++I` INSTEAD OF `I++` (OR USE ASSEMBLY WHEN APPLICABLE)

Use `++i` instead of ` i++`. This is especially useful in for loops but this optimization can be used anywhere in your code. 

2 Instances:
-Pages.sol line 251
-GobblerReserve.sol 37

## IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {

7 Instances:
-ArtGobblers.sol line 432, 592
-Pages.sol line 251
-GobblerReserve.sol line 37
-GlobblersERC721.sol line 186
-GobblersERC1155.sol lines 114, 173


## USING `PRIVATE` RATHER THAN `PUBLIC` FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

11 Instances:
-DeployRinkeby.sol lines 13, 14, 15
-ArtGobblers.sol lines 112, 115, 118, 122, 126, 177, 180, 184


## DUPLICATED REQUIRE()/REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION
Saves deployment costs
4 Intances:
-GobblersERC721.sol lines 121, 137
-Pages.sol lines 151, 135
## USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

20 Instances:
-ArtGobblers.sol lines 437, 885, 887, 889
-GobblersERC721.sol lines 62, 66, 95, 121, 137
-GobblersERC1155.sol lines 107, 149, 185
-PagesERC721.sol lines 55, 59, 85, 103, 105, 107, 135, 151

