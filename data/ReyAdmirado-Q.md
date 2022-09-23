## 1. typo in comments

moduloing --> modulating (maybe moduleing)

- [ArtGobblers.sol#L550](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L550)
- [ArtGobblers.sol#L673](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L673)


## 2. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. 

- [Pages.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol)
- [Goo.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol)
- [ArtGobblers.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol)
- [GobblerReserve.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol)
- [PagesERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol)
- [GobblersERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol)
- [GobblersERC1155B.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol)
- [ChainlinkV1RandProvider.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol)
- [RandProvider.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/RandProvider.sol)
- [DeployBase.s.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol)
- [DeployRinkeby.s.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol)
- [ERC721.sol](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol)
- [Owned.sol](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol)
- [LogisticToLinearVRGDA.sol](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticToLinearVRGDA.sol)
- [LinearVRGDA.sol](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol)
- [LogisticVRGDA.sol](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol)


## 3. event is missing indexed fields
Each event should use three indexed fields if there are three or more fields

- [Pages.sol#L134](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L134)
- [Pages.sol#L136](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L136)

- [ArtGobblers.sol#L232](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L232)
- [ArtGobblers.sol#L233](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L233)
- [ArtGobblers.sol#L234](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L234)
- [ArtGobblers.sol#L240](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L240)

- [PagesERC721.sol#L18](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L18)

- [GobblersERC721.sol#L17](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L17)

- [GobblersERC1155B.sol#L29](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L29)


## 4. constants should be defined rather than using magic numbers 
Even assembly can benefit from using readable constants instead of hex/numeric literals

- [ArtGobblers.sol#L632](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L632)
- [ArtGobblers.sol#L642](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L642)
- [ArtGobblers.sol#L643](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L643)
- [ArtGobblers.sol#L644](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L644)
- [ArtGobblers.sol#L674](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L674)

- [LogisticVRGDA.sol#L44](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L44)
- [LogisticVRGDA.sol#L47](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L47)
- [LogisticVRGDA.sol#L62](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L62)
- [DeployBase.s.sol#L66](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L66)
- [DeployBase.s.sol#L67](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L67)


## 5. inconsistent use of named return variables
there is an inconsistent use of named return variables in the contract some functions return named variables, others return explicit values. consider adopting a consistent approach.
this would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.
