- use `mul` instead of `<< 1`
    
    [https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L844](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L844)
    
    left shift doesn’t save any gas in Solidity. It is more readable to use `mul` or `*`
    
    ```json
    function testMyTest() public {
      uint res = 1 << 1;
    }
    
    ==
    
    function testMyTest() public {
      uint res = 1 * 2;
    }
    
    ```
    
- use `.div(4)` instead of `>>2`
    
    `>> 2` does not have any gas savings in favour of .`div` or `/ 4`.
    
    ```json
    function testMyTest() public {
      // gas: 3109
      uint res = 4 >> 2;
      console2.log(res);
    } 
    
    ==
    
    function testMyTest() public {
      // gas: 3109
      uint res = 4 / 4;
      console2.log(res);
    }
    ```