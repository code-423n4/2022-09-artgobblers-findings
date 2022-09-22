# [G-01] Redundant zero initialization

Solidity does not recognize null as a value, so uint variables are initialized to zero. Setting a uint variable to zero is redundant and can waste gas.

Locations where this was found include
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L251
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L114
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L173
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L186
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L592

## Recommended Mitigation Steps

Remove the redundant zero initialization
`uint256 i;` instead of `uint256 i = 0;`

# [G-02] Use prefix not postfix in loops

Using a prefix increment (++i) instead of a postfix increment (i++) saves gas for each loop cycle and so can have a big gas impact when the loop executes on a large number of elements.

Locations where this was found include
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37

## Recommended Mitigation Steps

Use prefix not postfix to increment in a loop

# [G-03] Use iszero assembly for zero checks

Comparing a value to zero can be done using the `iszero` EVM opcode. This can save gas

Source from t11s https://twitter.com/transmissions11/status/1474465495243898885

Locations where this was found include
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L266
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L122
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L138
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L528
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L615
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L623
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L696

## Recommended Mitigation Steps

Use the assembly `iszero` evm opcode to compare values to zero

# [G-04] Add payable to functions that won't receive ETH

Identifying a function as payable saves gas. Functions that have a modifier like onlyOwner or auth cannot be called by normal users and will not mistakenly receive ETH. These functions can be payable to save gas.

There are many functions that have access modifiers in the contracts. Some examples are
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L34
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L560
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L101
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L108
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L115

## Recommended Mitigation Steps

Add payable to these functions for gas savings

# [G-05] Use internal function in place of modifier

An internal function can save gas vs. a modifier. A modifier inlines the code of the original function but an internal function does not.

Source https://blog.polymath.network/solidity-tips-and-tricks-to-save-gas-and-reduce-bytecode-size-c44580b218e6#dde7

Locations where this was found include
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L34
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L560
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L101
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L108
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L115

## Recommended Mitigation Steps

Use internal functions in place of modifiers to save gas.

# [G-06] Use uint not bool

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

Locations where this was found include
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L368
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L727

## Recommended Mitigation Steps

Replace bool variables with uints

# [G-07] Use newer solidity version

A solidity version before 0.8.X is used in this project. The latest release of solidity includes changes that can provide gas savings. The improvements include:

* [Low level inliner](https://blog.soliditylang.org/2021/03/02/saving-gas-with-simple-inliner/) from `0.8.2`, leads to cheaper runtime gas. Especially relevant when the contract has small functions. For example, OpenZeppelin libraries typically have a lot of small helper functions and if they are not inlined, they cost an additional 20 to 40 gas because of 2 extra `jump` instructions and additional stack operations needed for function calls.
* [Optimizer improvements in packed structs](https://blog.soliditylang.org/2021/03/23/solidity-0.8.3-release-announcement/#optimizer-improvements): Before `0.8.3`, storing packed structs, in some cases used an additional storage read operation. After [EIP-2929](https://eips.ethereum.org/EIPS/eip-2929), if the slot was already cold, this means unnecessary stack operations and extra deploy time costs. However, if the slot was already warm, this means additional cost of `100` gas alongside the same unnecessary stack operations and extra deploy time costs.
* [Custom errors](https://blog.soliditylang.org/2021/04/21/custom-errors) from `0.8.4`, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.
* Solidity version 0.8.13 can save more gas with [Yul IR pipeline](https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/#yul-ir-pipeline-production-ready)

Source https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#upgrade-to-at-least-084

## Recommended Mitigation Steps

Use solidity release 0.8.13 with Yul IR pipeline and other improvements for gas savings

# [G-08] Use Solidity errors instead of require

Solidity errors introduced in version 0.8.4 can save gas on revert conditions
https://blog.soliditylang.org/2021/04/21/custom-errors/
https://twitter.com/PatrickAlphaC/status/1505197417884528640

Locations where this was found include
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L107
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L149
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L185
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L55
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L59
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L85
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L103
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L105
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L107
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L135
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L151
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L62
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L66
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L95
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L121
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L137
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L437
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L885
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L887
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L889

## Recommended Mitigation Steps

Replace require blocks with new solidity errors described in https://blog.soliditylang.org/2021/04/21/custom-errors/

# [G-09] Use simple comparison in trinary logic

The comparison operators >= and <= use more gas than >, <, or ==. Replacing the  >= and ≤ operators with a comparison operator that has an opcode in the EVM saves gas

The existing code is 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L462
```
cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

A simple comparison can be used for gas savings by reversing the logic
```
cost > LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? cost * 2 : LEGENDARY_GOBBLER_INITIAL_START_PRICE
```

## Recommended Mitigation Steps

Replace the comparison operator and reverse the logic to save gas using the suggestions above

# [G-10] Bitshift for divide or multiply by 2

When multiply or dividing by a power of two, it is cheaper to bitshift than to use standard math operations.

Using a biteshift instead of multiplication by 2 is already used in one place: https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L844

The existing code is with a divide and multiply by 2 operation is on this line:
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L462
```
cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

Bitshifting can save gas compared to division
```
cost <= (LEGENDARY_GOBBLER_INITIAL_START_PRICE >> 1) ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : (cost << 1)
```

## Recommended Mitigation Steps

Bitshift right by one bit instead of dividing by 2 to save gas

# [G-11] Non-public variables save gas

Many constant variables are public, but changing the visibility of these variables to private or internal can save gas.

Locations where this was found include
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L112
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L115
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L118
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L122
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L126
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L177
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L180
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L184
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L86
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L89
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L103
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L92
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L95
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L98
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L101
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L146
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L156
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L64
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L67

## Recommended Mitigation Steps

Declare some public variables as private or internal to save gas

# [G-12] Write contracts in vyper

The contracts are all written entirely in solidity. Writing contracts with vyper instead of solidity can save gas.

Source https://twitter.com/eiber_david/status/1515737811881807876
doggo demonstrates https://twitter.com/fubuloubu/status/1528179581974417414?t=-hcq_26JFDaHdAQZ-wYxCA&s=19

## Recommended Mitigation Steps

Write some or all of the contracts in vyper to save gas