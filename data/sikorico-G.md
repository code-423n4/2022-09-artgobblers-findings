# GAS REPORT

## [GAS 00] transferFrom(address(this), to, amount) can be changed to transfer(to, amount) to save gas


### Proof of concept:
- [Benchmarks.t.sol#L119](https://github.com/code-423n4/2022-09-artgobblers/tree/main/test/Benchmarks.t.sol#L119)
- [GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37)
