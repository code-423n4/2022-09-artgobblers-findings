# Low Risk

## [L-01] Non-fixed pragma

It's a good practice to avoid the use of non-fixed pragma. Code must be compiled with the same version it as been tested the most. It also avoids the use of any nightly builds which can have unexpected and unknown behaviors

8 instances:

 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L2
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L2
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L2
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L2
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L2
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L2
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/RandProvider.sol#L2
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L2

Consider replacing `>=0.8.0` by `0.8.0`

Low risk because the tremendous majority of the time there is any risk.