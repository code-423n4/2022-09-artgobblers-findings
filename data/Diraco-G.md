In For-Loop :
1 - An array’s length should be cached to save gas in for-loops
Reading array length at each iteration of the loop takes 6 gas
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37 
2 - Increments can be unchecked and ++i costs less gas compared to i++



https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186