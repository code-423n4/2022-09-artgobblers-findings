# Gas Optimizations Report for Art Gobblers contest

## Overview
During the audit, 8 gas issues were found.

№ | Title | Instance Count
--- | --- | --- 
G-1 | [Postfix increment and decrement](#g-1-postfix-increment-and-decrement) | 9
G-2 | [<>.length in loops](#g-2-length-in-loops) | 2
G-3 | [Initializing variables with default value](#g-3-initializing-variables-with-default-value) | 5
G-4 | [> 0 is more expensive than =! 0](#g-4--0-is--more-expensive-than--0) | 2
G-5 | [x += y is more expensive than x = x + y](#g-5-x--y-is--more-expensive-than-x--x--y) | 6
G-6 | [Public is more expensive than private for constants](#g-6-public-is-more-expensive-than-private-for-constants) | 8
G-7 | [Custom errors may be used](#g-7-custom-errors-may-be-used) | 33
G-8 | [Elements that are smaller than 32 bytes (256 bits) may increase gas usage](#g-8-elements-that-are-smaller-than-32-bytes-256-bits-may-increase-gas-usage) | 9

## Gas Optimizations Findings (8)
### G-1. Postfix increment and decrement
##### Description
Prefix increment and decrement cost less gas than postfix.

##### Instances
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L99
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L101
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L164
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L179
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L115
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L117
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L181
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

##### Recommendation
Consider using prefix increment and decrement where it is relevant. 

#
### G-2. <>.length in loops
##### Description
Reading the length of an array at each iteration of the loop consumes extra gas.

##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

##### Recommendation
Store the length of an array in a variable before the loop, and use it.

#
### G-3. Initializing variables with default value
##### Description
It costs gas to initialize integer variables with 0 or bool variables with false but it is not necessary.

##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

##### Recommendation
Remove initialization for default values.  
For example:
``` for (uint256 i; i < array.length; ++i) {```

#
### G-4. ```> 0``` is  more expensive than ```=! 0```
##### Instances
- https://github.com/code-423n4/2022-09-frax/blob/main/src/frxETHMinter.sol#L79
- https://github.com/code-423n4/2022-09-frax/blob/main/src/frxETHMinter.sol#L126

##### Recommendation
Use ```=! 0``` instead of ```> 0```, where possible.

#
### G-5. ```x += y``` is  more expensive than ```x = x + y```
##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L439
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L201
-  https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L203
-  https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L205
-  https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L244
-  https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L844

##### Recommendation
Use ```x = x + y``` instead of ```x += y```.
Use ```x = x - y``` instead of ```x -= y```.

#
### G-6. ```Public``` is more expensive than ```private``` for constants
##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L122
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L126
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L177
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L180
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L184

##### Recommendation
Use private constants instead of public, where possible.

#
### G-7. Custom errors may be used
##### Description
[Custom errors from Solidity 0.8.4 are cheaper than revert strings.](https://blog.soliditylang.org/2021/04/21/custom-errors/)

##### Instances
ZERO_ADDRESS
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L40
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L66
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L59

WRONG_FROM
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L437
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L87
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L103

INVALID_RECIPIENT
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L89
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L158
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L105

UNSAFE_RECIPIENT
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L118
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L134
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L196
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L211
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L149
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L185
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L121
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L137
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L135
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L151

NOT_AUTHORIZED
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L889
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L69
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L91
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L95
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L85
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L107

NOT_MINTED
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L36
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L175
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L62
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L55

ALREADY_MINTED
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L160

LENGTH_MISMATCH
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L107


##### Recommendation
For example, change:
```
require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
```
to:
```
error WrongFrom();
...
if (getGobblerData[id].owner != msg.sender) revert WrongFrom();
```

#
### G-8. Elements that are smaller than 32 bytes (256 bits) may increase gas usage
##### Description
According to [docs](https://docs.soliditylang.org/en/v0.8.16/internals/layout_in_storage.html#layout-of-state-variables-in-storage), when using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L159
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L167
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L187-L192
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L202-L213
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L44-L51
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L34-L40
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L47-L56
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L107
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L114


##### Recommendation
Use a larger size where needed.