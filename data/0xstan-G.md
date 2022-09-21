Have change this line, use `mintedFromGoo` replace `numMintedFromGoo` to save gas (MLOAD instead SLOAD).

```bash
git diff src/ArtGobblers.sol

diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..f149e6a 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -490,7 +490,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);
 
             // Compute how many gobblers were minted since the auction began.
-            uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
+            uint256 numMintedSinceStart = mintedFromGoo - numMintedAtStart;
 
             // prettier-ignore
             // If we've minted the full interval or beyond it, the price has decayed to 0.
```

.gas-snapshot diff is :

```bash
forge snapshot --diff .gas-snapshot2
diff .gas-snapshot .gas-snapshot2 


4c4
< ArtGobblersTest:testCannotMintLegendaryWithLegendary() (gas: 78518641)
---
> ArtGobblersTest:testCannotMintLegendaryWithLegendary() (gas: 78518380)
29,31c29,31
< ArtGobblersTest:testLegendaryGobblerFinalPrice() (gas: 75838529)
< ArtGobblersTest:testLegendaryGobblerMidPrice() (gas: 56917944)
< ArtGobblersTest:testLegendaryGobblerMinStartPrice() (gas: 75885780)
---
> ArtGobblersTest:testLegendaryGobblerFinalPrice() (gas: 75838442)
> ArtGobblersTest:testLegendaryGobblerMidPrice() (gas: 56917857)
> ArtGobblersTest:testLegendaryGobblerMinStartPrice() (gas: 75885606)
33,36c33,36
< ArtGobblersTest:testLegendaryGobblerPastFinalPrice() (gas: 71635852)
< ArtGobblersTest:testLegendaryGobblerTargetPrice() (gas: 37936155)
< ArtGobblersTest:testMintFreeLegendaryGobbler() (gas: 75872939)
< ArtGobblersTest:testMintFreeLegendaryGobblerPastInterval() (gas: 113792206)
---
> ArtGobblersTest:testLegendaryGobblerPastFinalPrice() (gas: 71635743)
> ArtGobblersTest:testLegendaryGobblerTargetPrice() (gas: 37936068)
> ArtGobblersTest:testMintFreeLegendaryGobbler() (gas: 75872764)
> ArtGobblersTest:testMintFreeLegendaryGobblerPastInterval() (gas: 113792032)
43,47c43,47
< ArtGobblersTest:testMintLegendaryGobbler() (gas: 41075776)
< ArtGobblersTest:testMintLegendaryGobblerWithInsufficientCost() (gas: 40766132)
< ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41267419)
< ArtGobblersTest:testMintLegendaryGobblerWithUnownedId() (gas: 40922878)
< ArtGobblersTest:testMintLegendaryGobblersExpectedIds() (gas: 264182200)
---
> ArtGobblersTest:testMintLegendaryGobbler() (gas: 41075601)
> ArtGobblersTest:testMintLegendaryGobblerWithInsufficientCost() (gas: 40765958)
> ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41267244)
> ArtGobblersTest:testMintLegendaryGobblerWithUnownedId() (gas: 40922704)
> ArtGobblersTest:testMintLegendaryGobblersExpectedIds() (gas: 264181110)
51c51
< ArtGobblersTest:testMintedLegendaryURI() (gas: 75895832)
---
> ArtGobblersTest:testMintedLegendaryURI() (gas: 75895744)
66c66
< BenchmarksTest:testLegendaryGobblersPrice() (gas: 10005)
---
> BenchmarksTest:testLegendaryGobblersPrice() (gas: 9896)
70c70
< BenchmarksTest:testMintLegendaryGobbler() (gas: 476972)
---
> BenchmarksTest:testMintLegendaryGobbler() (gas: 476885)
84c84
< OptimizationsTest:testFuzzCurrentIdMultipleBranchlessOptimization(uint256) (runs: 256, μ: 408, ~: 423)
---
> OptimizationsTest:testFuzzCurrentIdMultipleBranchlessOptimization(uint256) (runs: 256, μ: 415, ~: 423)
108,109c108,109
< VRGDAsTest:testNoOverflowForFirst8465Pages(uint256,uint256) (runs: 256, μ: 11477, ~: 11326)
< VRGDAsTest:testNoOverflowForMostGobblers(uint256,uint256) (runs: 256, μ: 11374, ~: 11218)
---
> VRGDAsTest:testNoOverflowForFirst8465Pages(uint256,uint256) (runs: 256, μ: 11486, ~: 11326)
> VRGDAsTest:testNoOverflowForMostGobblers(uint256,uint256) (runs: 256, μ: 11381, ~: 11218)
```