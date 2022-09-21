# QA REPORT

## [LOW] The project is compiled with different solidity versions


## [LOW] Use safeTransfer
Use safeTransfer in the following locations

### Proof of concept:
- [ArtGobblers.t.sol#L899](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/ArtGobblers.t.sol#L899)
- [GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37)

## [LOW] Missing pause functionality


### Proof of concept:
- [PagesERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol)
- [GobblerReserve.sol](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol)

## [LOW] Missing nonReentrancy modifier
The following functions allows attackers to try reentrancy since they are calling to external contracts / transferring eth. Consider adding a nonReentrancy modifier.

### Proof of concept:
- [Pages.t.sol#L27](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Pages.t.sol#L27)
- [Pages.t.sol#L52](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Pages.t.sol#L52)

## [LOW] Approve 0 first
At some tokens you can approve an amount (at USDT for instance) only after approving to 0. Consider using increase/decrease approve notation instead.

### Proof of concept:
- [GobblersERC1155B.sol#L81](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L81)
- [GobblersERC721.sol#L94](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L94)

## [LOW] Not verified input
At the following functions you should verify the parameters that are being assigned to a state variable.

### Proof of concept:
- [DeployBase.s.sol#L48](https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L48)
- [Pages.sol#L176](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L176)

## [LOW] In the following functions consider verifying the fee parameter
Where the fee parameter validation is checking greater than 0% (which may happen by mistake) and less than 100%

### Proof of concept:
- [ChainlinkV1RandProvider.sol#L54](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol#L54)
- [DeployBase.s.sol#L48](https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L48)

## [LOW] Out of bounds array access
Add a bounds check where accessing an array in the following locations.

### Proof of concept:
- [ArtGobblers.t.sol#L994](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/ArtGobblers.t.sol#L994)
- [Goo.t.sol#L16](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Goo.t.sol#L16)

## [NON CRITICAL] Floating pragma
Floating pragma is a bad practice, since it does not guaranty the same version at future deployments.

### Proof of concept:
- [ChainlinkV1RandProvider.sol](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol)
- [PagesERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol)

## [NON CRITICAL] transferFrom address(this)
The two statements ```transferFrom(address(this), to, amount)``` and ```transfer(to, amount)``` are the same while the right is more readable.

### Proof of concept:
- [Benchmarks.t.sol#L119](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Benchmarks.t.sol#L119)
- [GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37)

## [NON CRITICAL] Missing function spec comments


### Proof of concept:
- [Console.sol#L53](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/utils/Console.sol#L53)
- [Console.sol#L8](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/utils/Console.sol#L8)

## [NON CRITICAL] Unused function parameters should have name removed
If for any reason the following unused parameters are necessary then remove their naming (since only the type matters for function signature)

### Proof of concept:
- [ChainlinkV1RandProvider.sol#L54](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol#L54)
- [GobblerReserve.sol#L23](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L23)

## [NON CRITICAL] The following events are not indexed


### Proof of concept:
- [RandProvider.sol#L14](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/RandProvider.sol#L14)
- [RandProvider.sol#L13](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/RandProvider.sol#L13)

## [NON CRITICAL] Consider emitting an event at the following functions


### Proof of concept:
- [VRGDAs.t.sol#L36](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/VRGDAs.t.sol#L36)
- [Benchmarks.t.sol#L34](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Benchmarks.t.sol#L34)

--