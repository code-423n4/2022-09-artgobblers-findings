# Report

## Low Risk ##

### [L-01]: Zero-address checks are missing
**Context:** 

1. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316
```
team = _team;
```
2. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L317
```
community = _community;
```
3. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L43
```
artGobblers = _artGobblers;
```
4. https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L49
```
teamColdWallet = _teamColdWallet;
```
5. https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L52
```
vrfCoordinator = _vrfCoordinator;
```
6. https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L53
```
linkToken = _linkToken;
```
7. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L179
```
goo = _goo;
```
8. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L181
```
community = _community;
```
9. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L55
```
artGobblers = _artGobblers;
```
10. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83
```
artGobblers = _artGobblers;
```
11. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L84
```
pages = _pages;
```
12. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L30
```
owner = _owner;
```
13. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L40
```
owner = newOwner;
```
14. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L24
```
artGobblers = _artGobblers;
```

**Recommendation:**

Add zero-address checks.


## Non-Critical Issues ##

### [N-01]: Unused named return variable

**Context:**
 
1. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55
2. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L62
3. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L72

### [N-02]: Floating Pragma 

**Context:**
 
 All Contracts

**Recommendation:**

https://swcregistry.io/docs/SWC-103

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### [N-03]: Public function can be external
**Context:** 

1. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L693
2. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L35
3. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L39
4. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L66
5. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L76
6. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L111
7. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L126
8. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L146
9. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55
10. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L79
11. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L85
12. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L93
13. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L101
14. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L124
15. https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L60
16. https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L43
17. https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L17
18. https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol#L41

**Description:**

Public functions can be declared external if they are not called by the contract.

**Recommendation:**

Declare these functions as external instead of public.


### [N-04]: NatSpec is missing

1. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol
2. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol
3. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol
4. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol
5. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol
6. https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol
7. https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol
8. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol

### [N-05]: NatSpec is incomplete 

1. https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L691 ('@param' tags and '@return' tags are missing where they are needed) 
2. https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol ('@param' tags and '@return' tags are missing where they are needed)
3. [L219](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L219), [L239](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L239), [L265](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L265) (no '@return' tags)
4. [L62](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L62) ('@return' tag is missing)
5. [L8](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/MerkleProofLib.sol#L8) (NatSpec is missing)
6. [L8](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/LibString.sol#L8) (NatSpec is missing)
7. [L17](https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L17) ('@return' tag is missing)
