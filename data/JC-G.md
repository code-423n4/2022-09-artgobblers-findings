
## Summary

G-01 State variables only set in the constructor should be declared immutable | 4 instances 
G-02 <x> += <y> costs more gas than <x> = <x> + <y> for state variables | 9 instances 
G-03 internal functions only called once can be inlined to save gas | 2 instances 
G-04 Cache array length before loop | 2 instances 
G-05 Using bools for storage incurs overhead | 2 instances 
G-06 Use a more recent version of solidity | 19 instances 
G-07 Use != 0 instead of > 0 | 1 instances 
G-08 Redundant zero initialization | 7 instances 
G-09 Use prefix not postfix in loops | 2 instances 
G-10 Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | 44 instances 
G-11 Using private rather than public for constants, saves gas | 13 instances 
G-12 Empty blocks should be removed or emit something | 3 instances 
G-13 Functions guaranteed to revert when called by normal users can be marked payable | 5 instances 
G-14 Public functions to external | 8 instances 
G-15 Use calldata instead of memory for function parameters | 3 instances 
G-16 Use simple comparison in trinary logic / in if statement  | 4 instances 
G-17 ++i costs less gas compared to i++ or i += 1 | 2 instances 

Total: 130 instances in 17 issues


## G-01 State variables only set in the constructor should be declared immutable

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

4 instances in 2 files:

utils/token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L23
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L25

utils/token/PagesERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L24
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L26


## G-02 <x> += <y> costs more gas than <x> = <x> + <y> for state variables

9 instances in 3 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L439
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L456
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L464
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L662
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L844
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L912
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L913

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L244

GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L184



## G-03 internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

2 instances in 2 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L813-L817

token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L174-L178



## G-04 Cache array length before loop (see <array>.length)

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack. 
Caching the array length in the stack saves around 3 gas per iteration. 

Recommended Mitigation Steps: 
I suggest storing the array’s length in a variable before the for-loop, and use it instead

2 instances in 2 files:

token/GobblersERC1155B.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114

/utils/GobblerReserve.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37




## G-05 Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. 

This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled. 
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 

Recommended Mitigation Steps: 
Use uint256(1) and uint256(2) for true/false

2 instances in 2 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L149

tokens/ERC721.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L51


## G-06 Use a more recent version of solidity

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


## G-07 Use != 0 instead of > 0

Using > 0 uses slightly more gas than using != 0. Use != 0 when comparing uint variables to zero, which cannot hold values below zero. 

Recommended Mitigation Steps: 
Use != 0 instead of > 0. This change saves 6 gas per instance

1 instance in 1 file:

utils/SignedWadMath.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L142


## G-08 Redundant zero initialization

Solidity does not recognize null as a value, so uint variables are initialized to zero. Setting a uint variable to zero is redundant and can waste gas. 
There are several places where an int is initialized to zero, which looks like: uint256 amount = 0; 

Recommended Mitigation Steps: 
Remove the redundant zero initialization uint256 amount; ( / or: If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). 
Explicitly initializing it with its default value is an anti-pattern and wastes gas.

7 instances in 5 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251

utils/token/GobblersERC1155B.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173

utils/GobblerReserve.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

utils/token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186


## G-09 Use prefix not postfix in loops

Using a prefix increment (++i) instead of a postfix increment (i++) saves gas for each loop cycle and so can have a big gas impact when the loop executes on a large number of elements. 
Saves 6 gas PER LOOP

2 instances in 2 files:

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251

utils/GobblerReserve.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37


## G-10 Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. 
This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. 
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html 

Use a larger size then downcast where needed

44 instances in 3 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L167
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L189
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L191
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L204
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L206
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L208
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L210
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L324
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L327
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L449
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L454
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L455
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L458
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L461
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L535
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L551
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L615
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L616
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L623
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L624
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L650
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L660
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L661
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L662
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L679
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L680
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L681
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L825
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L826
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L855
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L899
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L903
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L904
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L910
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L911

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L244
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L253

utils/token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L38
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L39
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L49
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L51
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L53
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L55
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L184



## G-11 Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

13 instances in 2 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L122
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L126
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L136
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L139
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L177
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L180
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L184

script/deploy/DeployRinkeby.s.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L13
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L14
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L15


## G-12 Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. 
If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation.
 If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})

3 instances in 2 files:

utils/MerkleProofLib.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/MerkleProofLib.sol#L23 (2)

utils/LibString.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/LibString.sol#L30


## G-13 Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. 

The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

5 instances in 3 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L560

Goo.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L101
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L108
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L115

GobblerReserve.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L34


## G-14 Public functions to external

Public functions not called by the contract should be declared external instead. Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so. https://docs.soliditylang.org/en/latest/contracts.html#function-overriding

8 instances in 4 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L693

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L265

utils/token/GobblersERC1155B.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55

utils/token/GobblersERC1155B.so
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L79
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L85
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L93
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L101
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L124




## G-15 Use calldata instead of memory for function parameters

Having function arguments use calldata instead of memory can save gas. 

Recommended Mitigation Steps: 
Change function arguments from memory to calldata.

3 instances in 2 files:

utils/token/GobblersERC1155B.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L138
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L161

tokens/ERC721.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L207



## G-16 Use simple comparison in trinary logic / in if statement  

The comparison operators >= and <= use more gas than >, <, or ==. Replacing the >= and ≤ operators with a comparison operator that has an opcode in the EVM saves gas. 

Recommended Mitigation Steps: 
Replace the comparison operator and reverse the logic to save gas using the suggestions above.

4 instances in 2 files:

ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L435
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L497

utils/SignedWadMath.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L86
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L90


## G-17 ++i costs less gas compared to i++ or i += 1

++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). 
This statement is true even with the optimizer enabled. i++ increments i and returns the initial value of i. 
Which means: uint i = 1;  i++; // == 1 but i == 2 But ++i returns the actual incremented value: uint i = 1;  ++i; // == 2 and i == 2 too, so no need for a temporary variable . 
In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2. 

Recommended Mitigation Steps: 
I suggest using ++i instead of i++ to increment the value of an uint variable.

2 instances in 2 files:

Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251

utils/GobblerReserve.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37