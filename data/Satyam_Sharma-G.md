issue 1.
unneccesary wastage of gas...
NO need of using (getGobblerData[id].owner = address(0)) in function mintLegendaryGobbler in emit L441 since in loop it already declared pre-increment ..therefor it never be turned to zero..

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L441

issue 2.
unnecessary use of bytes32 ... results to wastage of gas while calling a function...

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L546

remove bytes32...