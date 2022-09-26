## [NAZ-G1] Extra Bits Left In Struct
**Context**: [`ArtGobblers.sol#L202`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L202)

**Description**:
Currently in the struct there are 248 bits being used and this is a was of gas when the struct is read from as a whole. It would be cheaper to fill the struct to 256 bits.

**Recommendation**: 
Consider filling in the extra bits left in the struct.


## [NAZ-G2] Use `_batchMint` for Page Reserve Minting
**Context**: [`Pages.sol#L251`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)

**Description**:
There are benefits to using `_batchMint()` as such:
1. Reduce wasted storage of token metadata.
2. Limit ownership state updates to only once per batch mint

**Recommendation**: 
Consider using `_batchMint()` for page reserve minting to save gas.


## [NAZ-G3] Use `++index` instead of `index++` to increment a loop counter
**Context**: [`Pages.sol#L251`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251), [`GobblerReserve.sol#L37`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)

**Description**:
Due to reduced stack operations, using `++index` saves 5 gas per iteration.

**Recommendation**: 
Use `++index `to increment a loop counter.


## [NAZ-G4] Use `++index` instead of `index++` to increment a loop counter
**Context**: [`Pages.sol#L251`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251), [`GobblerReserve.sol#L37`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)

**Description**:
Due to reduced stack operations, using `++index` saves 5 gas per iteration.

**Recommendation**: 
Use `++index `to increment a loop counter.


## [NAZ-G5] Use of Custom Errors Instead of String
**Context**: [`ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol), [`GobblersERC1155B.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol), [`GobblersERC721.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol), [`PagesERC721.sol`](), [`VRGDA.sol`]()

**Description**:
To save some gas the use of custom errors leads to cheaper deploy time cost and run time cost. The run time cost is only relevant when the revert condition is met.

**Recommendation**: 
Use Custom Errors instead of strings.


## [NAZ-G6] Setting The Constructor To Payable
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-artgobblers)

**Description**:
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of `msg.value == 0` and saves 21 gas on deployment with no security risks.

**Recommendation**: 
Set the constructor to payable.


## [NAZ-G7] Function Ordering via Method ID
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-artgobblers)

**Description**:
Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. One could use [`This tool`](https://emn178.github.io/solidity-optimize-name/) to help find alternative function names with lower Method IDs while keeping the original name intact.

**Recommendation**: 
Find a lower method ID name for the most called functions for example ```mostCalled()``` vs. ```mostCalled_41q()``` is cheaper by 44 gas.
