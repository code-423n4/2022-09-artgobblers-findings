Use the local variable after assigning the global variable value to local varibale:

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L482

```uint256 mintedFromGoo = numMintedFromGoo;``` - use the `mintedFromGoo` is other places instead of using the  `numMintedFromGoo`

it seems the local varibale `mintedFromGoo` only once and in other places `numMintedFromGoo` is used.

Refer the line https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L493

