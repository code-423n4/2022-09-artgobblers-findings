- In two places, `i++` can be replaced with `++i`:
	- https://github.com/code-423n4/2022-09-artgobblers/blob/6e0df2e5e82b51856e451d028a44593ef18c74b1/src/Pages.sol#L251
	- https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/GobblerReserve.sol#L37
- Replacing the following left shift with a multiplication by 2 slightly improves gas in some tests: https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L844

The improved results are:
```
ArtGobblersTest:testCanMintMultipleReserved() (gas: 1363062)
ArtGobblersTest:testCanMintReserved() (gas: 670319)
ArtGobblersTest:testCantMintTooFastReserved() (gas: 1239120)
ArtGobblersTest:testCantMintTooFastReservedOneByOne() (gas: 6429878)
ArtGobblersTest:testMintReservedGobblersFailsWithNoMints() (gas: 32660)
BenchmarksTest:testMintReservedGobblers() (gas: 105760)
GobblerReserveTest:testCanWithdraw() (gas: 763844)
```
- In `VRFConsumerBase`, custom errors could be used. Of course this would require to copy the library, but this would also reduce the gas consumption of Art Gobblers.