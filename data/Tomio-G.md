1.
Title: Caching `length` for loop can save gas

Proof of Concept:
[GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
[GobblersERC1155B.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114)

Recommended Mitigation Steps:
Change to:

```
    uint256 Length = ids.length;
    for (uint256 i = 0; i < Length; i++) {
```
________________________________________________________________________

2.
Title: Default value initialization

Impact:
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Proof of Concept:
[GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
[GobblersERC1155B.sol#L114](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114)
[GobblersERC1155B.sol#L173](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173)

Recommended Mitigation Steps:
Remove explicit initialization for default values.
________________________________________________________________________

3.
Title: Increment gas optimization

Proof of Concept:
[PagesERC721.sol#L115-L117](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L115-L117)
[PagesERC721.sol#L181](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L181)

Recommended Mitigation Steps:
Consider change it to:

```
	--_balanceOf[from];

        ++_balanceOf[to];
```
________________________________________________________________________

4.
Title: Consider make constant as private to save gas

Proof of Concept:
[ArtGobblers.sol#L112-L126](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112-L126)
[ArtGobblers.sol#L177-L184](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L177-L184)
[DeployRinkeby.s.sol#L13-L15](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L13-L15)

Recommended Mitigation Steps:
I suggest changing the visibility from `public` to `internal` or `private`
________________________________________________________________________

5.
Title: Custom errors from Solidity 0.8.4 are cheaper than revert strings

Impact:
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information

Custom errors are defined using the error statement
reference: https://blog.soliditylang.org/2021/04/21/custom-errors/

Proof of Concept:
[GobblersERC1155B.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol) (various line)
[GobblersERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol) (various line)
[PagesERC721.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol) (various line)
[ArtGobblers.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol) (various line)

Recommended Mitigation Steps:
Replace require statements with custom errors.
________________________________________________________________________

6.
Title: Using bytes constant is more gas efficient

Reference: [Here](https://ethereum.stackexchange.com/questions/3795/why-do-solidity-examples-use-bytes32-type-instead-of-string)

Proof of Concept:
[DeployRinkeby.s.sol#L13-L15](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L13-L15)

Recommended Mitigation Steps:
Change it to: `bytes(1..32) constant`
________________________________________________________________________

7.
Title: Set as `immutable` can save gas

Proof of Concept:
[Owned.sol#L17](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L17)

Recommended Mitigation Steps:
can be set as immutable, which already set once in the constructor
________________________________________________________________________