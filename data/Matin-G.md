1 - No need to re-initialize the variable to its default value:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173

2 - Pre-incrementing costs low gas than post-incrementing:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

3 - It's better to save the 'for' loop's length before the loop starts:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
