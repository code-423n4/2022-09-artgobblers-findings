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
for (uint256 i; i < numPages; i++)
```