G-1 Public function visibility can be made external
Functions should have the strictest visibility possible. Public functions may lead to more gas usage by forcing the copy of their parameters to memory from calldata.
If a function is never called from the contract it should be marked as external. This will save gas
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55

G-2 Using private rather than public for constants saves gas
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L122
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L126

G-3 If a variable is not set/initialized, it is assumed to have the default value ( 0 for uint, false for bool, address(0) for address…).

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186

G-4 .length should no be looked up in every loop of a for-loop
The overheads outlined below are PER LOOP, excluding the first loop storage arrays incur a Gwarmaccess (100 gas) memory arrays use MLOAD (3 gas) calldata arrays use CALLDATALOAD (3 gas)

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

G-5 Shifting Is cheaper than dividing by 2

A division by 2 can be calculated by shifting one to the right. While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity’s division operation also includes a division-by-0 prevention which is bypassed using shifting.
Consider replacing / 2 with >> 1 here

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L462
