## 1. state variables only set in the constructor should be declared immutable

- [Pages.sol#L96](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L96)

- [ArtGobblers.sol#L136](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L136)
- [ArtGobblers.sol#L139](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L139)

- [PagesERC721.sol#L24](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L24)
- [PagesERC721.sol#L26](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L26)

- [GobblersERC721.sol#L23](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L23)
- [GobblersERC721.sol#L25](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L25)


## 2. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

- [Pages.sol#L244](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L244)

- [ArtGobblers.sol#L905](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L905)
- [ArtGobblers.sol#L906](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L906)
- [ArtGobblers.sol#L456](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L456)
- [ArtGobblers.sol#L464](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L464)
- [ArtGobblers.sol#L662](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L662)
- [ArtGobblers.sol#L844](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L844)
- [ArtGobblers.sol#L912](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L912)
- [ArtGobblers.sol#L913](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L913)

- [GobblersERC721.sol#L184](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L184)


## 3. not using the named return variables when a function returns, wastes deployment gas

- [PagesERC721.sol#L72](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L72)

- [GobblersERC1155B.sol#L55](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55)


## 4. can make the variable outside the loop to save gas

- [ArtGobblers.sol#L599](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L599)
- [ArtGobblers.sol#L602](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L602)
- [ArtGobblers.sol#L605](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L605)
- [ArtGobblers.sol#L608](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L608)
- [ArtGobblers.sol#L615](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L615)
- [ArtGobblers.sol#L620](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L620)
- [ArtGobblers.sol#L623](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L623)
- [ArtGobblers.sol#L632](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L632)


## 5. `<array>.length` should not be looked up in every loop of a for-loop
This reduce gas cost as show here https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/5

1- if it is a storage array, this is an extra sload operation (100 additional extra gas (EIP-2929 2) for each iteration except for the first),
2- if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first),
3- if it is a calldata array, this is an extra calldataload operation (3 additional gas for each iteration except for the first)

- [GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)

- [GobblersERC1155B.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114)


## 6. `++i` costs less gas than `i++`, especially when it’s used in for-loops (--i/i-- too)
Saves 6 gas per loop

- [Pages.sol#L251](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)

- [GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)

- [PagesERC721.sol#L115](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L115)
- [PagesERC721.sol#L117](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L117)
- [PagesERC721.sol#L181](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L181)


## 7. it costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied

- [Pages.sol#L251](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)

- [ArtGobblers.sol#L432](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432)
- [ArtGobblers.sol#L592](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592)

- [GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)

- [GobblersERC721.sol#L186](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186)

- [GobblersERC1155B.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114)
- [GobblersERC1155B.sol#L73](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L73)


## 8. use custom errors rather than revert()/require() strings to save deployment gas

https://blog.soliditylang.org/2021/04/21/custom-errors/

- [ArtGobblers.sol#L437](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L437)
- [ArtGobblers.sol#L885](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885)
- [ArtGobblers.sol#L887](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887)
- [ArtGobblers.sol#L889](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L889)

- [PagesERC721.sol#L55](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L55)
- [PagesERC721.sol#L59](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L59)
- [PagesERC721.sol#L85](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L85)
- [PagesERC721.sol#L103](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L103)
- [PagesERC721.sol#L105](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L105)
- [PagesERC721.sol#L107](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L107)
- [PagesERC721.sol#L135](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L135)
- [PagesERC721.sol#L151](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L151)

- [GobblersERC721.sol#L62](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L62)
- [GobblersERC721.sol#L66](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L66)
- [GobblersERC721.sol#L95](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L95)
- [GobblersERC721.sol#L121](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L121)
- [GobblersERC721.sol#L137](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L137)

- [GobblersERC1155B.sol#L107](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L107)
- [GobblersERC1155B.sol#L149](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L149)
- [GobblersERC1155B.sol#L185](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L185)


## 9. usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

- [Pages.sol#L107](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L107)
- [Pages.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L114)

- [ArtGobblers.sol#L159](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L159)
- [ArtGobblers.sol#L167](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L167)
- [ArtGobblers.sol#L899](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L899)
- [ArtGobblers.sol#L615](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L615)
- [ArtGobblers.sol#L623](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L623)


## 10. using private rather than public for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table

- [ArtGobblers.sol#L112](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112)
- [ArtGobblers.sol#L115](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115)
- [ArtGobblers.sol#L118](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118)
- [ArtGobblers.sol#L122](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L122)
- [ArtGobblers.sol#L126](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L126)
- [ArtGobblers.sol#L177](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L177)
- [ArtGobblers.sol#L180](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L180)
- [ArtGobblers.sol#L184](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L184)


## 11. bytes constants are more efficient than string constants
If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity.

- [Pages.sol#L96](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L96)
- [Pages.sol#L164](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L164)

- [ArtGobblers.sol#L136](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L136)
- [ArtGobblers.sol#L139](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L139)
- [ArtGobblers.sol#L298](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L298)
- [ArtGobblers.sol#L299](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L299)

- [PagesERC721.sol#L24](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L24)
- [PagesERC721.sol#L26](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L26)
- [PagesERC721.sol#L38](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L38)
- [PagesERC721.sol#L39](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L39)

- [GobblersERC721.sol#L23](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L23)
- [GobblersERC721.sol#L25](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L25)
- [GobblersERC721.sol#L83](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L83)
