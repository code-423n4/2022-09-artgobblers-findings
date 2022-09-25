
ISSUE 1.
unnecessary use of bytes32 ... results to wastage of gas while calling a function...

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L546

remove bytes32...

ISSUE 2.
Use revert to save gas instead of using require...
use custome error it is helpful in saving gas rather than using require statement!!!!!

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887 