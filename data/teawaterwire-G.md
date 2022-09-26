it's meager optim but in `mintLegendaryGobbler()` the first lines can also be wrapped in the `unchecked` since there's no risk of overflow either

```solidity
    function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) {
        unchecked {
            gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold; // Assign id.

            // If the gobbler id would be greater than the max supply, there are no remaining legendaries.
            if (gobblerId > MAX_SUPPLY) revert NoRemainingLegendaryGobblers();

            // This will revert if the auction hasn't started yet, no need to check here as well.
            uint256 cost = legendaryGobblerPrice();

            if (gobblerIds.length < cost) revert InsufficientGobblerAmount(cost);

            // Overflow should not occur in here, as most math is on emission multiples, which are inherently small.
            uint256 burnedMultipleTotal; // The legendary's emissionMultiple will be 2x the sum of the gobblers burned.
```

which gives the following benchmark:

```bash
forge test -vv --match-test testMintLegendaryGobbler --match-contract Benchmark

Running 1 test for test/Benchmarks.t.sol:BenchmarksTest
[PASS] testMintLegendaryGobbler() (gas: 476803)

```

compared to:

```
Running 1 test for test/Benchmarks.t.sol:BenchmarksTest
[PASS] testMintLegendaryGobbler() (gas: 476972)
```