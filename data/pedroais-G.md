1- i++ is cheaper than ++i
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

2-When feeding a gobble with various ERC1155's the gobble function needs to be called multiple times since it only transfers 1 at a time. A new parameter called amount should be created for ERC1155's to allow adding a lot of them in one tx which saves gas.
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L747

3-A left shift can be done instead of multiplying by 2 to save gas. This is correctly done in some parts of the code but not done here : 
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L449

4-The following addition could be unchecked since numSold is capped by max supply
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L412

5-The following variables should be immutable to save gas since they never change after they are set in the constructor :

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L136
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L139