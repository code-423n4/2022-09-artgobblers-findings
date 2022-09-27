



# QA Reports

#### L-1 << 1 can be convert into * 2 


File : ArtGobblers.sol [Line-844](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L844)

Here << 1 doesn't save any gas compare to * 2. This happen due to optimizer_runs (1000000) very high so it automatically convert * 2 to << 1. It is reduce the readbility of code.

#### L-1 ++ can be convert into += 1

File : PagesERC721.sol [Line-844](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L-181)

Here ++ doesn't save any gas compare to += 1.It is reduce the readbility of code.
