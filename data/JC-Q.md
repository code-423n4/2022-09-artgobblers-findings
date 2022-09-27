
## Summary

L-01 Missing checks for address(0x0) when assigning values to address state variables  | 15 instances
L-02 abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256() | 1 instance
L-03 Unspecific Compiler Version Pragma | 7 instances
L-04 Duplicated Code | 2 instances

N-01 Use a more recent version of solidity | 19 instances
N-02 File is missing NatSpec | 6 instances
N-03 NatSpec is incomplete | 1 instance
N-04 Event is missing indexed fields | 10 instances

Total: 61 instances in 8 issues


## L-01 Missing checks for address(0x0) when assigning values to address state variables 

15 instances in 8 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L314
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L315
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L317

Goo.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L84

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L179
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L181

utils/GobblerReserve.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L24

utils/token/PagesERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L43

auth/Owned.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L30

utils/rand/ChainlinkV1RandProvider.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L55

script/deploy/DeployBase.s.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L49
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L52
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L53


 

## L-02 abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions 
(e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456).
https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode
“Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.
https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739

1 instance in 1 file:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L347


## L-03 Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version, e.g. 'pragma solidity ^0.8.0;' -> 'pragma solidity 0.8.4;"

7 instances:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L2



## L-04 Duplicated Code

These functions are duplicated for no reason.

2 instances in 2 files:

utils/token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L129-L143

utils/token/PagesERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L142-L156





## N-01 Use a more recent version of solidity

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath 
Use a solidity version of at least 0.8.2 to get compiler automatic inlining 
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads 
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value. 
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>). 
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

19 instances in 19 files:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L2

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L2
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L2
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/MerkleProofLib.sol#L2
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/LibString.sol#L2
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L2
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L2

https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L2
https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L2
https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L2
https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol#L2

https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L2
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L2




## N-02 File is missing NatSpec

6 instances:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol


## N-03 NatSpec is incomplete

1 instance:

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol


## N-04 Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

10 instances in 6 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L233
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L234
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L240
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L242

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L134
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L135

utils/token/GobblersERC1155B.so
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L29

utils/token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L17

utils/token/PagesERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L18

src/tokens/ERC721.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L15

