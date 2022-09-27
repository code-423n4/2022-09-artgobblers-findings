#1 Use  x = x + y or  x = x - y more cheap than x += y or x -= y for state variables

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L439

Change the state to x = x + y or x = x - y for saving gas when possible.

#2 Cache the gooBalance(user).

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L821-L822

caching gooBalance(user) to memory because its call multiple times. it can save gas fee.

#3  Increment

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

pre increment e.g ++i more cheaper gas than post increment e.g i++. i suggest to use pre increment.

#4 Looping

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37

default uint is 0 so remove unnecassary explicit can reduce gas.
pre increment e.g ++i more cheaper gas than post increment e.g i++. i suggest to use pre increment.
caching the array length can reduce gas it caused access to a local variable is more cheap than query storage / calldata / memory in solidity.

#5 Visibility

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L13-L15

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L112-L126

change visibility from public to private or internal can save the gas when possible.
