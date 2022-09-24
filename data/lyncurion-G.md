https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251
In the increment section of this for loop, using ++i instead of i++ could result in lower gas consumption.


https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L164
Using ++_balanceOf[to] instead of _balanceOf[to]++ could result in lower gas consumption.


https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L179
Using --_balanceOf[to] instead of _balanceOf[to]-- could result in lower gas consumption.


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37
In the increment section of this for loop, using ++i instead of i++ could result in lower gas consumption.
