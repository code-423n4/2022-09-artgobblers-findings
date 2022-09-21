# GAS REPORT

## [GAS] Use assembly opcodes iszero in the following locations


### Proof of concept:
- [GobblersERC721.sol#L136](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L136)
- [Pages.sol#L265](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L265)

## [GAS] Mark as payable If has onlyOwner modifier
In order to save gas you can put a payable modifier for functions that are called by protocol owners.

### Proof of concept:
- [Goo.sol#L101](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L101)
- [GobblerReserve.sol#L34](https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L34)

--