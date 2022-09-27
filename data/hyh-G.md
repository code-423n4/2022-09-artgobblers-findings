### numMintedFromGoo second usage is extra SLOAD

Storage is read the second time when it's already saved to memory and isn't modified:

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L482-L493

```solidity
        uint256 mintedFromGoo = numMintedFromGoo;

        unchecked {
            // The number of gobblers minted at the start of the auction is computed by multiplying the # of
            // intervals that must pass before the next auction begins by the number of gobblers in each interval.
            uint256 numMintedAtStart = (numSold + 1) * LEGENDARY_AUCTION_INTERVAL;

            // If not enough gobblers have been minted to start the auction yet, return how many need to be minted.
            if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);

            // Compute how many gobblers were minted since the auction began.
            uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
```

## Recommended Mitigation Steps

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L482-L493

```solidity
        uint256 mintedFromGoo = numMintedFromGoo;

        unchecked {
            // The number of gobblers minted at the start of the auction is computed by multiplying the # of
            // intervals that must pass before the next auction begins by the number of gobblers in each interval.
            uint256 numMintedAtStart = (numSold + 1) * LEGENDARY_AUCTION_INTERVAL;

            // If not enough gobblers have been minted to start the auction yet, return how many need to be minted.
            if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);

            // Compute how many gobblers were minted since the auction began.
-           uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
+           uint256 numMintedSinceStart = mintedFromGoo - numMintedAtStart;
```