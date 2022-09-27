  # Gas Optimizations

## Summary Of Findings:

  | Issue 
-- | -- 
1 | Optimizing for loop's in `mintLegendaryGobbler` and `revealGobblers` method
2 | Cache state variable in `legendaryGobblerPrice()` method
3 | Optimizing formula in `mintReservedGobblers` method

## Detailed Report on Gas Optimization Findings:

### 1. <ins>Optimizing for loop's in `mintLegendaryGobbler` and `revealGobblers` method</ins>
The [mintLegendaryGobbler](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L411) function in `ArtGobblers.sol` can save gas by modifying the `for loop` in the following way:
 
 The following `diff` shows the mitigation:
```diff
diff --git "a/.\\orig.txt" "b/.\\modified.txt"
index 1d26328..a09619f 100644
--- "a/.\\orig.txt"
+++ "b/.\\modified.txt"
@@ -429,7 +429,9 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
             uint256 id; // Storing outside the loop saves ~7 gas per iteration.
 
-            for (uint256 i = 0; i < cost; ++i) {
+            for (uint256 i = 0; ; ++i) {
+		if(i == cost) break;  //gas saving here.
+
                 id = gobblerIds[i];
 
                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
```

The forge gas report shows an average saving of 36 (72774 - 72738) and a maximum saving of 168 (456707 - 456539) from this single change.

The same kind of change could be made in the `for loop` in the [revealGobblers](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592) function as well. The diff information shows the exact change:
```diff
diff --git "a/.\\orig.txt" "b/.\\modified.txt"
index 1d26328..f755227 100644
--- "a/.\\orig.txt"
+++ "b/.\\modified.txt"
@@ -589,7 +589,8 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         // Implements a Knuth shuffle. If something in
         // here can overflow, we've got bigger problems.
         unchecked {
-            for (uint256 i = 0; i < numGobblers; ++i) {
+            for (uint256 i = 0; ; ++i) {
+		if(i == numGobblers) break;
                 /*//////////////////////////////////////////////////////////////
                                       DETERMINE RANDOM SWAP
                 //////////////////////////////////////////////////////////////*/
```

The forge gas report shows an average saving of 64 (525805 - 525741) and a maximum saving of 303 (2907828 - 2907525) gas for the `revealGobblers` function from this single change.

### 2. <ins>Cache state variable in `legendaryGobblerPrice()` method</ins>
The [legendaryGobblerPrice](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L478) function in `ArtGobblers.sol` can save gas by using the cached state variable in one of the formulas. The diff below shows the change.
 
```diff
diff --git "a/.\\orig.txt" "b/.\\modified.txt"
index 1d26328..39f7a95 100644
--- "a/.\\orig.txt"
+++ "b/.\\modified.txt"
@@ -490,7 +490,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);
 
             // Compute how many gobblers were minted since the auction began.
-            uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
+            uint256 numMintedSinceStart = mintedFromGoo - numMintedAtStart;
 
             // prettier-ignore
             // If we've minted the full interval or beyond it, the price has decayed to 0.
```

The forge gas report shows an average saving of 109 (1646 - 1537) gas for `legendaryGobblerPrice` function from this single change.

### 3. <ins>Optimizing formula in `mintReservedGobblers` method</ins>
The [mintReservedGobblers](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L839) function in `ArtGobblers.sol` can save gas by modifying one of the formulas. The diff below shows the change.
 
```diff
diff --git "a/.\\orig.txt" "b/.\\modified.txt"
index 1d26328..2e5a111 100644
--- "a/.\\orig.txt"
+++ "b/.\\modified.txt"
@@ -841,7 +841,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             // Optimistically increment numMintedForReserves, may be reverted below. Overflow in this
             // calculation is possible but numGobblersEach would have to be so large that it would cause the
             // loop in _batchMint to run out of gas quickly. Shift left by 1 is equivalent to multiplying by 2.
-            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
+            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach + numGobblersEach);
 
             // Ensure that after this mint gobblers minted to reserves won't comprise more than 20% of
             // the sum of the supply of goo minted gobblers and the supply of gobblers minted to reserves.
```
The forge gas report shows an average saving of 3 gas (65982 - 65979) from this single change.

## Conclusions:

  | Issue | Max Gas Saved
-- | -- | -- 
1 | Optimizing for loop in `mintLegendaryGobbler` method | 168
2 | Optimizing for loop in `revealGobblers` method | 303
3 | Cache state variable in `legendaryGobblerPrice()` method | 109
4 | Optimizing formula in `mintReservedGobblers` method | 3

### TOTAL GAS SAVED = 168 + 303 + 109 + 3 = <ins>583</ins>.
