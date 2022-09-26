## 1.DEFAULT VALUE INITIALIZATION

### PROBLEM
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

### PROOF OF CONCEPT
Instances include:
[ArtGobblers.sol#L432](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432)
[ArtGobblers.sol#L592](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592)
[Pages.sol#L251](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)
[GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
[GobblersERC721.sol#L186](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186)
[GobblersERC1155B.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114)
[GobblersERC1155B.sol#L173](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173)

### MITIGATION
```
for (uint256 i; i < numPages; ++i)
```

### 2.Caching the length in for loops

### PROBLEM
In the context of a for-loop that iterates over an array, it costs less gas to cache the array's length in a variable and read from this variable rather than use the arrays .length property. Reading the .length property for on the array will cause a recalculation of the array's length on each iteration of the loop which is a more expensive operation than reading from a stack variable.

For example, the following code:
```
for (uint i; i < arr.length; ++i) {
	// ...
}
````
This extra costs can be avoided by caching the array length (in stack): should be changed to:
```
uint length = arr.length;
for (uint i; i < length; ++i) {
	// ...
}
```
### INSTANCES
[GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
[GobblersERC1155B.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114)