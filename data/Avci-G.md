Dont initilize ( i ) with 0 in for) operation

Impact: More gas costs.


The instances of this issue:

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114

============================================
Use ++i instead i++ in increments. 

POC: It saves about 57 gas for each and it means in total about 114 gas in all scopes.

Impact: More gas costs.

The instances of this issue:


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37






==================================================
Replace <= with <, and >= with >.

Note :  Do not forget to increment/decrement the compared variable (cause in EVM we don have opcode for >= its means > + = and its not good)
 
Instances of issue: 

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L435
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L497
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L462
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L695
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L702

mitigation: < instead <= with =1 or -1 





