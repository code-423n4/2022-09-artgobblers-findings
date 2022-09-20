## 1 Unpacked structures
In ethereum, you pay gas for every storage slot you use. A slot is of 256 bits, and you can pack as many variables as you want in it. Packing is done by solidity compiler and optimizer automatically, you just need to declare the packable functions consecutively.

#Code references:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L202
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L34
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L47
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L44

# Mitigation
In above code references, pack the bigger uint variables towards end of struct.

e.g.
uint8 numberOne;
uint256 bigNumber;
uint8 numberTwo;

uint8 numberOne;
uint8 numberTwo;
uint256 bigNumber;

## 2 Use of custom errors
Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

# Code references
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L437
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885-889
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L62
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L66
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L95
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L121
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L137
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L107
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L149
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L185
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L55
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L59
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L85
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L103-107
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L135
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L151

# Mitigation
Change the require() to if (a != b) revert ERROR()
