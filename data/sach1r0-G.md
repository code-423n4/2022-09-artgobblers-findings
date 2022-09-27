# Functions that are not called within the contract must set its visibility to `external` instead of `public`

## Details
Setting function's visibility to external when it is only called externally can save gas because external function’s parameters are not copied into memory and are instead read from calldata directly.
see reference: https://github.com/code-423n4/2021-06-gro-findings/issues/37

## Mitigation
Set function visibility to `external`

## Line of code
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L693-L712
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L265-L269
___
# Pre-increment/Pre-decrement cost less gas than post-increment/post-decrement

## Details
`_balanceOf[to]++` costs more gas than `++_balanceOf[to]` , for uint pre-decrement is cheaper than post-decrement
see reference: https://github.com/code-423n4/2021-12-nftx-findings/issues/195

## Mitigation
change `_balanceOf[to]++` to `++_balanceOf[to]`

## Line of code
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L115-L117
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L181
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

___
# Solidity compiler will always read the length of the array during each iteration
## Details
.length in a loop can be extracted into a variable and used where necessary to reduce the number of storage reads
see reference: https://github.com/code-423n4/2021-10-union-findings/issues/92
## Mitigation:
This extra costs can be avoided by caching the array length. 
Example:
`uint idsLength = ids.length;`
`for (uint i = 0; i < idsLength; i++) {
}`
## Line of code
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37