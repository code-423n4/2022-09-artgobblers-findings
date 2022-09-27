# QA Report for Art Gobblers contest

## Overview
During the audit, 1 low and 5 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
L-1 | [Large number of elements may cause out-of-gas error](#l-1-large-number-of-elements-may-cause-out-of-gas-error) | Low | 6
NC-1 | [Order of Layout](#nc-1-order-of-layout) | Non-Critical | 6
NC-2 | [Floating pragma](#nc-2-floating-pragma) | Non-Critical | 20
NC-3 | [Missing NatSpec](#nc-3-missing-natspec) | Non-Critical | 53
NC-4 | [Public functions can be external](#nc-4-public-functions-can-be-external) | Non-Critical | 18
NC-5 | [Scientific notation may be used](#nc-5-scientific-notation-may-be-used) | Non-Critical | 2

## Low Risk Findings (1)
### L-1. Large number of elements may cause out-of-gas error
##### Description
[Loops](https://docs.soliditylang.org/en/develop/security-considerations.html#gas-limit-and-loops) that do not have a fixed number of iterations, for example, loops that depend on storage values, have to be used carefully: Due to the block gas limit, transactions can only consume a certain amount of gas. Either explicitly or just due to normal operation, the number of iterations in a loop can grow beyond the block gas limit, which can cause the complete contract to be stalled at a certain point. 

##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

##### Recommendation
Restrict the maximum number of elements.

## Non-Critical Risk Findings (5)
### NC-1. Order of Layout
##### Description
According to [Order of Layout](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout), inside each contract, library or interface, use the following order:
1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions

##### Instances
Events before state variables:
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L7-L15
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L9-L31
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L9-L17
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L10-L18
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L7-L11

Modifier after constructor:
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L92-L96

##### Recommendation
Place events after state variables, modifier - before constructor.

#
### NC-2. Floating pragma
##### Description
Contracts should be deployed with the same compiler version. It helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

##### Instances
All 20 contracts.

##### Recommendation
According to [SWC-103](https://swcregistry.io/docs/SWC-103), pragma version should be locked.

#
### NC-3. Missing NatSpec
##### Description 
NatSpec is missing for 53 functions in 8 contracts.

##### Instances
- [1 function in ArtGobblers.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L880)
- [14 functions in ERC721.sol](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol)
- [10 functions in GobblersERC1155B.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol)
- [4 functions in SignedWadMath.sol](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol)
- [11 functions in GobblersERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol)
- [11 functions in PagesERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol)
- [1 function in DeployBase.s.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L61)
- [1 function in MerkleProofLib.sol](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/MerkleProofLib.sol#L8)


##### Recommendation
Add NatSpec for all functions.

#
### NC-4. Public functions can be external
##### Description
If functions are not called by the contract where they are defined, they can be declared external.

##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L693
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L35
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L39
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L66
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L76
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L111
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L126
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L146
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L79
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L85
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L93
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L101
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L124
- https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L60
- https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L43
- https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L17
- https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol#L41

##### Recommendation
Make public functions external, where possible.

#
### NC-5. Scientific notation may be used
##### Description
For readability, it is better to use scientific notation.

##### Instances
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112
- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115

##### Recommendation
Replace ```10000``` with ```10e4```.