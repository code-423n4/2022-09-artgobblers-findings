
ISSUE 1.
unnecessary use of bytes32 ... results to wastage of gas while calling a function...

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L546

remove bytes32...

ISSUE 2.
Use revert to save gas instead of using require...
use custom error it is helpful in saving gas rather than using require statement!!!!!

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887 

ISSUE 3.
Refactor a modifier to call a local function instead of directly having the code in the 
modifier, saving bytecode size and thereby deployment cost..

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L19

reference: https://0xmacro.com/blog/solidity-gas-optimizations-cheat-sheet/ ..8. point