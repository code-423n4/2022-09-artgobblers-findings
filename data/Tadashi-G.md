### Computation of Goo balance can be optimized

**Summary**: Pre-computing the square root of 1e18 in `computeGOOBalance` saves gas. 

**Details**: Perform this change in [L38](https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L38) of LibGoo.sol:

```diff
-(emissionMultiple * lastBalanceWad * 1e18).sqrt()
+(emissionMultiple * lastBalanceWad).sqrt() * 1e9
```

⛽ **Profiling**: Computed using `forge snapshot --diff`:

```solidity
testCanMintPageFromVirtualBalance() (gas: -24 (-0.008%)) 
testGooAddition() (gas: -20 (-0.008%)) 
testGooRemoval() (gas: -24 (-0.009%)) 
testMintFromGooBalance() (gas: -24 (-0.009%)) 
testSnapshotDoesNotAffectBalance() (gas: -96 (-0.024%)) 
Overall gas change: -188 (-0.059%)
```

**Notes**:
    - The comments in the previous lines should be rewritten if this change is made
    - The resolution of the operation will decrease if this change is made

### Block of code can be made unchecked

**Summary**: part of `tokenURI` can be made `unchecked`

**Details**: Since there will be only 10 legendary Globbers, [L707-L709](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L707-L709) of ArtGobblers.sol can be changed to

```solidity
unchecked {
	if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold)
	  return string.concat(BASE_URI, gobblerId.toString());
}
```

⛽ **Profiling**: Computed using `forge snapshot --diff`:

```solidity
testMintedLegendaryURI() (gas: -161 (-0.000%)) 
testUnmintedLegendaryUri() (gas: -402 (-1.588%)) 
Overall gas change: -563 (-1.589%)
```

### **Post-increments that can be replaced by pre-increments**

**Summary**: Pre-incrementing a variable is cheaper than post-incrementing it. For more information, see [G012](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g012---use-prefix-increment-instead-of-postfix-increment-if-possible) of c4-common-issues. 

**Details**: 
Consider the following changes:

1. [L37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37) of ArtGobblers.sol 
    
    ```diff
    -for (uint256 i = 0; i < ids.length; i++) {
    +for (uint256 i = 0; i < ids.length; ++i) {
    ```
    
2. [L251](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251) of Pages.sol 
    
    ```diff
    -for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
    +for (uint256 i = 0; i < numPages; ++i) _mint(community, ++lastMintedPageId);
    ```
    

⛽ **Profiling**: Computed using `forge snapshot --diff`:

```solidity
testCanWithdraw() (gas: -6 (-0.001%)) 
testFeedingArt() (gas: -224 (-0.087%)) 
testPagePricingPricingBeforeSwitch() (gas: -820505 (-0.365%)) 
testPagePricingPricingAfterSwitch() (gas: -2685499 (-0.743%)) 
testCantMintTooFastCommunityOneByOne() (gas: -57520 (-0.970%)) 
testCantMintTooFastCommunity() (gas: -21566 (-1.835%)) 
testCanMintMultipleCommunity() (gas: -39536 (-3.196%)) 
testCanMintCommunity() (gas: -37288 (-5.721%)) 
testCanMintPageFromVirtualBalance() (gas: -22181 (-7.406%)) 
testCantFeed721As1155() (gas: -17745 (-7.541%)) 
testMintPage() (gas: -5081 (-8.639%)) 
testMintPageUsingVirtualBalance() (gas: -5081 (-8.782%)) 
testCantgobbleToUnownedGobbler() (gas: -17745 (-15.405%)) 
testRegularMint() (gas: -17745 (-16.310%)) 
testMintCommunityPages() (gas: -22181 (-37.596%)) 
Overall gas change: -3769903 (-114.598%)
```