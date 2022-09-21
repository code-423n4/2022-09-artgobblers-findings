# QA REPORT

## [QA 00] Timelock is missing for the following functions


### Proof of concept:
- [Goo.t.sol#L16](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Goo.t.sol#L16)
- [GobblersERC721.sol#L102](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L102)
- [DeployRinkeby.t.sol#L17](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/deploy/DeployRinkeby.t.sol#L17)
- [Pages.t.sol#L27](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Pages.t.sol#L27)
- [Benchmarks.t.sol#L34](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Benchmarks.t.sol#L34)

## [QA 01] Remove the name from the following unused function parameters


### Proof of concept:
- [GobblerReserve.sol#L23](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L23)
- [ChainlinkV1RandProvider.sol#L54](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol#L54)
- [Pages.sol#L176](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L176)

## [QA 02] Not emitted event for state changes


### Proof of concept:
- [DeployBase.s.sol#L61](https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L61)
- [Pages.t.sol#L27](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Pages.t.sol#L27)
- [DeployBase.s.sol#L48](https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L48)
- [GobblerReserve.sol#L23](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L23)
- [Benchmarks.t.sol#L34](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Benchmarks.t.sol#L34)
