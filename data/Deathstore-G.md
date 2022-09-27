
diff --git a/.gas-snapshot b/.gas-snapshot
index c494841..9a11231 100644
--- a/.gas-snapshot
+++ b/.gas-snapshot
@@ -75,7 +75,7 @@ BenchmarksTest:testPagePrice() (gas: 9108)
 BenchmarksTest:testRemoveGoo() (gas: 31872)
 BenchmarksTest:testRevealGobblers() (gas: 2913008)
 BenchmarksTest:testTransferGobbler() (gas: 49062)
-GobblerReserveTest:testCanWithdraw() (gas: 763845)
+GobblerReserveTest:testCanWithdraw() (gas: 763853)
 GooTest:testBurnAllowed() (gas: 56314)
 GooTest:testBurnNotAllowed() (gas: 56354)
 GooTest:testMintByAuthority() (gas: 53601)
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..0f1565a 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -34,7 +34,8 @@ contract GobblerReserve is Owned {
     function withdraw(address to, uint256[] calldata ids) external onlyOwner {
         // This is quite inefficient, but it's okay because this is not a hot path.
         unchecked {
-            for (uint256 i = 0; i < ids.length; i++) {
+            uint256 length = ids.length;
+            for (uint256 i = 0; i < length; i++) {
                 artGobblers.transferFrom(address(this), to, ids[i]);
             }
         }
diff --git a/src/utils/token/GobblersERC1155B.sol b/src/utils/token/GobblersERC1155B.sol
index ccf5cd0..18b8471 100644
--- a/src/utils/token/GobblersERC1155B.sol
+++ b/src/utils/token/GobblersERC1155B.sol
@@ -104,14 +104,15 @@ abstract contract GobblersERC1155B {
         virtual
         returns (uint256[] memory balances)
     {
-        require(owners.length == ids.length, "LENGTH_MISMATCH");
+        uint256 length = owners.length;
+        require(length == ids.length, "LENGTH_MISMATCH");
 
-        balances = new uint256[](owners.length);
+        balances = new uint256[](length);
 
         // Unchecked because the only math done is incrementing
         // the array index counter which cannot possibly overflow.
         unchecked {
-            for (uint256 i = 0; i < owners.length; ++i) {
+            for (uint256 i = 0; i < length; ++i) {
                 balances[i] = balanceOf(owners[i], ids[i]);
             }
         }
diff --git a/.gas-snapshot b/.gas-snapshot
index c494841..9a11231 100644
--- a/.gas-snapshot
+++ b/.gas-snapshot
@@ -75,7 +75,7 @@ BenchmarksTest:testPagePrice() (gas: 9108)
 BenchmarksTest:testRemoveGoo() (gas: 31872)
 BenchmarksTest:testRevealGobblers() (gas: 2913008)
 BenchmarksTest:testTransferGobbler() (gas: 49062)
-GobblerReserveTest:testCanWithdraw() (gas: 763845)
+GobblerReserveTest:testCanWithdraw() (gas: 763853)
 GooTest:testBurnAllowed() (gas: 56314)
 GooTest:testBurnNotAllowed() (gas: 56354)
 GooTest:testMintByAuthority() (gas: 53601)
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..0f1565a 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -34,7 +34,8 @@ contract GobblerReserve is Owned {
     function withdraw(address to, uint256[] calldata ids) external onlyOwner {
         // This is quite inefficient, but it's okay because this is not a hot path.
         unchecked {
-            for (uint256 i = 0; i < ids.length; i++) {
+            uint256 length = ids.length;
+            for (uint256 i = 0; i < length; i++) {
                 artGobblers.transferFrom(address(this), to, ids[i]);
             }
         }
diff --git a/.gas-snapshot b/.gas-snapshot
index c494841..e38bd43 100644
--- a/.gas-snapshot
+++ b/.gas-snapshot
@@ -1,62 +1,62 @@
-ArtGobblersTest:testCanMintMultipleReserved() (gas: 1363063)
-ArtGobblersTest:testCanMintPageFromVirtualBalance() (gas: 299517)
-ArtGobblersTest:testCanMintReserved() (gas: 670320)
-ArtGobblersTest:testCannotMintLegendaryWithLegendary() (gas: 78518641)
-ArtGobblersTest:testCannotMintPageWithInsufficientBalance() (gas: 217059)
-ArtGobblersTest:testCannotReuseSeedForReveal() (gas: 275715)
-ArtGobblersTest:testCannotRevealMoreGobblersThanRemainingToBeRevealed() (gas: 299793)
-ArtGobblersTest:testCantAddMoreGooThanOwned() (gas: 209640)
-ArtGobblersTest:testCantFeed1155As721() (gas: 1754349)
-ArtGobblersTest:testCantFeed721As1155() (gas: 235316)
-ArtGobblersTest:testCantFeedGobblers() (gas: 179190)
-ArtGobblersTest:testCantFeedUnownedArt() (gas: 140301)
-ArtGobblersTest:testCantMintTooFastReserved() (gas: 1239121)
-ArtGobblersTest:testCantMintTooFastReservedOneByOne() (gas: 6429888)
-ArtGobblersTest:testCantRemoveGoo() (gas: 210496)
-ArtGobblersTest:testCantSetRandomSeedWithoutRevealing() (gas: 286963)
+ArtGobblersTest:testCanMintMultipleReserved() (gas: 1363003)
+ArtGobblersTest:testCanMintPageFromVirtualBalance() (gas: 299511)
+ArtGobblersTest:testCanMintReserved() (gas: 670288)
+ArtGobblersTest:testCannotMintLegendaryWithLegendary() (gas: 78515677)
+ArtGobblersTest:testCannotMintPageWithInsufficientBalance() (gas: 217053)
+ArtGobblersTest:testCannotReuseSeedForReveal() (gas: 275700)
+ArtGobblersTest:testCannotRevealMoreGobblersThanRemainingToBeRevealed() (gas: 299781)
+ArtGobblersTest:testCantAddMoreGooThanOwned() (gas: 209628)
+ArtGobblersTest:testCantFeed1155As721() (gas: 1754343)
+ArtGobblersTest:testCantFeed721As1155() (gas: 235311)
+ArtGobblersTest:testCantFeedGobblers() (gas: 179183)
+ArtGobblersTest:testCantFeedUnownedArt() (gas: 140295)
+ArtGobblersTest:testCantMintTooFastReserved() (gas: 1239076)
+ArtGobblersTest:testCantMintTooFastReservedOneByOne() (gas: 6429564)
+ArtGobblersTest:testCantRemoveGoo() (gas: 210484)
+ArtGobblersTest:testCantSetRandomSeedWithoutRevealing() (gas: 286946)
 ArtGobblersTest:testCantgobbleToUnownedGobbler() (gas: 115187)
 ArtGobblersTest:testDoesNotAllowRevealingZero() (gas: 16278)
 ArtGobblersTest:testDoesNotRevertEarly() (gas: 7735)
 ArtGobblersTest:testDoesRevertWhenExpected() (gas: 10079)
-ArtGobblersTest:testEmissionMultipleUpdatesAfterTransfer() (gas: 253644)
-ArtGobblersTest:testFeeding1155() (gas: 1763713)
-ArtGobblersTest:testFeedingArt() (gas: 258468)
-ArtGobblersTest:testFeedingMultiple1155Copies() (gas: 1805578)
-ArtGobblersTest:testGobblerBalancesAfterTransfer() (gas: 254049)
-ArtGobblersTest:testGooAddition() (gas: 243272)
-ArtGobblersTest:testGooRemoval() (gas: 262382)
+ArtGobblersTest:testEmissionMultipleUpdatesAfterTransfer() (gas: 253632)
+ArtGobblersTest:testFeeding1155() (gas: 1763707)
+ArtGobblersTest:testFeedingArt() (gas: 258464)
+ArtGobblersTest:testFeedingMultiple1155Copies() (gas: 1805572)
+ArtGobblersTest:testGobblerBalancesAfterTransfer() (gas: 254037)
+ArtGobblersTest:testGooAddition() (gas: 243262)
+ArtGobblersTest:testGooRemoval() (gas: 262370)
 ArtGobblersTest:testInitialGobblerPrice() (gas: 14640)
-ArtGobblersTest:testLegendaryGobblerFinalPrice() (gas: 75838529)
-ArtGobblersTest:testLegendaryGobblerMidPrice() (gas: 56917944)
-ArtGobblersTest:testLegendaryGobblerMinStartPrice() (gas: 75885780)
+ArtGobblersTest:testLegendaryGobblerFinalPrice() (gas: 75835738)
+ArtGobblersTest:testLegendaryGobblerMidPrice() (gas: 56915849)
+ArtGobblersTest:testLegendaryGobblerMinStartPrice() (gas: 75882987)
 ArtGobblersTest:testLegendaryGobblerMintBeforeStart() (gas: 22126)
-ArtGobblersTest:testLegendaryGobblerPastFinalPrice() (gas: 71635852)
-ArtGobblersTest:testLegendaryGobblerTargetPrice() (gas: 37936155)
-ArtGobblersTest:testMintFreeLegendaryGobbler() (gas: 75872939)
-ArtGobblersTest:testMintFreeLegendaryGobblerPastInterval() (gas: 113792206)
+ArtGobblersTest:testLegendaryGobblerPastFinalPrice() (gas: 71630620)
+ArtGobblersTest:testLegendaryGobblerTargetPrice() (gas: 37934758)
+ArtGobblersTest:testMintFreeLegendaryGobbler() (gas: 75870145)
+ArtGobblersTest:testMintFreeLegendaryGobblerPastInterval() (gas: 113788018)
 ArtGobblersTest:testMintFromBalanceInsufficient() (gas: 23701)
 ArtGobblersTest:testMintFromGoo() (gas: 111224)
-ArtGobblersTest:testMintFromGooBalance() (gas: 252966)
+ArtGobblersTest:testMintFromGooBalance() (gas: 252960)
 ArtGobblersTest:testMintFromMintlist() (gas: 108203)
 ArtGobblersTest:testMintFromMintlistBeforeMintingStarts() (gas: 13877)
 ArtGobblersTest:testMintInsufficientBalance() (gas: 22876)
-ArtGobblersTest:testMintLegendaryGobbler() (gas: 41075776)
-ArtGobblersTest:testMintLegendaryGobblerWithInsufficientCost() (gas: 40766132)
-ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41267419)
-ArtGobblersTest:testMintLegendaryGobblerWithUnownedId() (gas: 40922878)
-ArtGobblersTest:testMintLegendaryGobblersExpectedIds() (gas: 264182200)
+ArtGobblersTest:testMintLegendaryGobbler() (gas: 41073875)
+ArtGobblersTest:testMintLegendaryGobblerWithInsufficientCost() (gas: 40764568)
+ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41265686)
+ArtGobblersTest:testMintLegendaryGobblerWithUnownedId() (gas: 40921148)
+ArtGobblersTest:testMintLegendaryGobblersExpectedIds() (gas: 264162931)
 ArtGobblersTest:testMintNotInMintlist() (gas: 11229)
 ArtGobblersTest:testMintPriceExceededMax() (gas: 72095)
 ArtGobblersTest:testMintReservedGobblersFailsWithNoMints() (gas: 32661)
-ArtGobblersTest:testMintedLegendaryURI() (gas: 75895832)
+ArtGobblersTest:testMintedLegendaryURI() (gas: 75893038)
 ArtGobblersTest:testMintingFromMintlistTwiceFails() (gas: 107465)
-ArtGobblersTest:testMultiReveal() (gas: 8475477)
-ArtGobblersTest:testPricingBasic() (gas: 53959904)
-ArtGobblersTest:testRevealDelayInitialMint() (gas: 114733)
-ArtGobblersTest:testRevealDelayRecurring() (gas: 259136)
-ArtGobblersTest:testRevealedUri() (gas: 217542)
-ArtGobblersTest:testSimpleRewards() (gas: 218517)
-ArtGobblersTest:testSnapshotDoesNotAffectBalance() (gas: 395277)
+ArtGobblersTest:testMultiReveal() (gas: 8475112)
+ArtGobblersTest:testPricingBasic() (gas: 53957800)
+ArtGobblersTest:testRevealDelayInitialMint() (gas: 114728)
+ArtGobblersTest:testRevealDelayRecurring() (gas: 259122)
+ArtGobblersTest:testRevealedUri() (gas: 217530)
+ArtGobblersTest:testSimpleRewards() (gas: 218505)
+ArtGobblersTest:testSnapshotDoesNotAffectBalance() (gas: 395256)
 ArtGobblersTest:testUnmintedLegendaryUri() (gas: 25309)
 ArtGobblersTest:testUnmintedUri() (gas: 13435)
 ArtGobblersTest:testUnrevealedUri() (gas: 117119)
@@ -64,44 +64,44 @@ BenchmarksTest:testAddGoo() (gas: 31862)
 BenchmarksTest:testGobblerPrice() (gas: 8986)
 BenchmarksTest:testGooBalance() (gas: 11831)
 BenchmarksTest:testLegendaryGobblersPrice() (gas: 10005)
-BenchmarksTest:testMintCommunityPages() (gas: 58998)
+BenchmarksTest:testMintCommunityPages() (gas: 58992)
 BenchmarksTest:testMintGobbler() (gas: 58811)
 BenchmarksTest:testMintGobblerUsingVirtualBalance() (gas: 49718)
-BenchmarksTest:testMintLegendaryGobbler() (gas: 476972)
+BenchmarksTest:testMintLegendaryGobbler() (gas: 476636)
 BenchmarksTest:testMintPage() (gas: 58812)
 BenchmarksTest:testMintPageUsingVirtualBalance() (gas: 57854)
-BenchmarksTest:testMintReservedGobblers() (gas: 105761)
+BenchmarksTest:testMintReservedGobblers() (gas: 105749)
 BenchmarksTest:testPagePrice() (gas: 9108)
 BenchmarksTest:testRemoveGoo() (gas: 31872)
-BenchmarksTest:testRevealGobblers() (gas: 2913008)
+BenchmarksTest:testRevealGobblers() (gas: 2912705)
 BenchmarksTest:testTransferGobbler() (gas: 49062)
-GobblerReserveTest:testCanWithdraw() (gas: 763845)
+GobblerReserveTest:testCanWithdraw() (gas: 763802)
 GooTest:testBurnAllowed() (gas: 56314)
 GooTest:testBurnNotAllowed() (gas: 56354)
 GooTest:testMintByAuthority() (gas: 53601)
 GooTest:testMintByNonAuthority() (gas: 13113)
 GooTest:testSetPages() (gas: 64071)
 OptimizationsTest:testFuzzCurrentIdMultipleBranchlessOptimization(uint256) (runs: 256, μ: 408, ~: 423)
-PagesTest:testCanMintCommunity() (gas: 651820)
-PagesTest:testCanMintMultipleCommunity() (gas: 1236901)
-PagesTest:testCantMintTooFastCommunity() (gas: 1175310)
-PagesTest:testCantMintTooFastCommunityOneByOne() (gas: 5929636)
+PagesTest:testCanMintCommunity() (gas: 651792)
+PagesTest:testCanMintMultipleCommunity() (gas: 1236848)
+PagesTest:testCantMintTooFastCommunity() (gas: 1175264)
+PagesTest:testCantMintTooFastCommunityOneByOne() (gas: 5929369)
 PagesTest:testInsufficientBalance() (gas: 20641)
 PagesTest:testMintBeforeSetMint() (gas: 20654)
 PagesTest:testMintBeforeStart() (gas: 11878)
 PagesTest:testMintCommunityPagesFailsWithNoMints() (gas: 30550)
 PagesTest:testMintPriceExceededMax() (gas: 69364)
-PagesTest:testPagePricingPricingAfterSwitch() (gas: 361305984)
-PagesTest:testPagePricingPricingBeforeSwitch() (gas: 224772592)
+PagesTest:testPagePricingPricingAfterSwitch() (gas: 361277544)
+PagesTest:testPagePricingPricingBeforeSwitch() (gas: 224764017)
 PagesTest:testRegularMint() (gas: 108798)
 PagesTest:testSwitchSmoothness() (gas: 13072)
 PagesTest:testTargetPrice() (gas: 14671)
 RandProviderTest:testOnlyGobblersCanRequestRandomness() (gas: 8223)
-RandProviderTest:testRandomnessIsCorrectlyRequested() (gas: 167211)
-RandProviderTest:testRandomnessIsFulfilled() (gas: 175668)
-RandProviderTest:testRandomnessIsNotUpgradableWithPendingSeed() (gas: 507049)
+RandProviderTest:testRandomnessIsCorrectlyRequested() (gas: 167205)
+RandProviderTest:testRandomnessIsFulfilled() (gas: 175662)
+RandProviderTest:testRandomnessIsNotUpgradableWithPendingSeed() (gas: 507043)
 RandProviderTest:testRandomnessIsOnlyUpgradableByOwner() (gas: 351791)
-RandProviderTest:testRandomnessIsUpgradable() (gas: 460201)
+RandProviderTest:testRandomnessIsUpgradable() (gas: 460195)
 VRGDAsTest:testFailOverflowForBeyondLimitGobblers(uint256,uint256) (runs: 256, μ: 10288, ~: 10288)
 VRGDAsTest:testGobblerPriceStrictlyIncreasesForMostGobblers() (gas: 4073280)
 VRGDAsTest:testNoOverflowForAllGobblers(uint256,uint256) (runs: 256, μ: 11196, ~: 11196)
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..6edb788 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -429,7 +429,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
             uint256 id; // Storing outside the loop saves ~7 gas per iteration.
 
-            for (uint256 i = 0; i < cost; ++i) {
+            for (uint256 i = 0; i != cost; ++i) {
                 id = gobblerIds[i];
 
                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
@@ -589,7 +589,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         // Implements a Knuth shuffle. If something in
         // here can overflow, we've got bigger problems.
         unchecked {
-            for (uint256 i = 0; i < numGobblers; ++i) {
+            for (uint256 i = 0; i != numGobblers; ++i) {
                 /*//////////////////////////////////////////////////////////////
                                       DETERMINE RANDOM SWAP
                 //////////////////////////////////////////////////////////////*/
diff --git a/src/Pages.sol b/src/Pages.sol
index 0d4ac88..8e2a008 100644
--- a/src/Pages.sol
+++ b/src/Pages.sol
@@ -248,7 +248,7 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
             if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();
 
             // Mint the pages to the community reserve while updating lastMintedPageId.
-            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
+            for (uint256 i = 0; i != numPages; i++) _mint(community, ++lastMintedPageId);
 
             currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id.
 
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..4ba2f87 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -34,7 +34,7 @@ contract GobblerReserve is Owned {
     function withdraw(address to, uint256[] calldata ids) external onlyOwner {
         // This is quite inefficient, but it's okay because this is not a hot path.
         unchecked {
-            for (uint256 i = 0; i < ids.length; i++) {
+            for (uint256 i = 0; i != ids.length; i++) {
                 artGobblers.transferFrom(address(this), to, ids[i]);
             }
         }
diff --git a/src/utils/token/GobblersERC1155B.sol b/src/utils/token/GobblersERC1155B.sol
index ccf5cd0..bf9060a 100644
--- a/src/utils/token/GobblersERC1155B.sol
+++ b/src/utils/token/GobblersERC1155B.sol
@@ -111,7 +111,7 @@ abstract contract GobblersERC1155B {
         // Unchecked because the only math done is incrementing
         // the array index counter which cannot possibly overflow.
         unchecked {
-            for (uint256 i = 0; i < owners.length; ++i) {
+            for (uint256 i = 0; i != owners.length; ++i) {
                 balances[i] = balanceOf(owners[i], ids[i]);
             }
         }
@@ -170,7 +170,7 @@ abstract contract GobblersERC1155B {
 
         // Counter overflow is unrealistic on human timescales.
         unchecked {
-            for (uint256 i = 0; i < amount; ++i) {
+            for (uint256 i = 0; i != amount; ++i) {
                 ids[i] = ++lastMintedId; // Increment id while setting.
 
                 amounts[i] = 1; // ERC1155B amounts are always 1.
diff --git a/src/utils/token/GobblersERC721.sol b/src/utils/token/GobblersERC721.sol
index f2d0647..df13884 100644
--- a/src/utils/token/GobblersERC721.sol
+++ b/src/utils/token/GobblersERC721.sol
@@ -183,7 +183,7 @@ abstract contract GobblersERC721 {
         unchecked {
             getUserData[to].gobblersOwned += uint32(amount);
 
-            for (uint256 i = 0; i < amount; ++i) {
+            for (uint256 i = 0; i != amount; ++i) {
                 getGobblerData[++lastMintedId].owner = to;
 
                 emit Transfer(address(0), to, lastMintedId);
diff --git a/test/ArtGobblers.t.sol b/test/ArtGobblers.t.sol
index 174d7b9..32a5f65 100644
--- a/test/ArtGobblers.t.sol
+++ b/test/ArtGobblers.t.sol
@@ -318,7 +318,7 @@ contract ArtGobblersTest is DSTestPlus {
 
         vm.warp(block.timestamp + timeDelta);
 
-        for (uint256 i = 0; i < numMint; i++) {
+        for (uint256 i = 0; i != numMint; i++) {
             vm.startPrank(address(gobblers));
             uint256 price = gobblers.gobblerPrice();
             goo.mintForGobblers(users[0], price);
@@ -444,7 +444,7 @@ contract ArtGobblersTest is DSTestPlus {
 
         assertEq(gobblers.getGobblerEmissionMultiple(mintedLegendaryId), emissionMultipleSum * 2);
 
-        for (uint256 i = 0; i < ids.length; i++) {
+        for (uint256 i = 0; i !=  ids.length; i++) {
             hevm.expectRevert("NOT_MINTED");
             gobblers.ownerOf(ids[i]);
         }
@@ -574,7 +574,7 @@ contract ArtGobblersTest is DSTestPlus {
         // We expect the first legendary to have this id.
         uint256 nextMintLegendaryId = 9991;
         mintGobblerToAddress(users[0], gobblers.LEGENDARY_AUCTION_INTERVAL());
-        for (int256 i = 0; i < 10; i++) {
+        for (int256 i = 0; i !=  10; i++) {
             vm.warp(block.timestamp + 400 days);
 
             mintGobblerToAddress(users[0], gobblers.LEGENDARY_AUCTION_INTERVAL());
@@ -1038,7 +1038,7 @@ contract ArtGobblersTest is DSTestPlus {
     function testLongRunningMintMaxFromGoo() public {
         uint256 maxMintableWithGoo = gobblers.MAX_MINTABLE();
 
-        for (uint256 i = 0; i < maxMintableWithGoo; i++) {
+        for (uint256 i = 0; i !=  maxMintableWithGoo; i++) {
             vm.warp(block.timestamp + 1 days);
             uint256 cost = gobblers.gobblerPrice();
             vm.prank(address(gobblers));
@@ -1052,7 +1052,7 @@ contract ArtGobblersTest is DSTestPlus {
     function testLongRunningMintMaxFromGooRevert() public {
         uint256 maxMintableWithGoo = gobblers.MAX_MINTABLE();
 
-        for (uint256 i = 0; i < maxMintableWithGoo + 1; i++) {
+        for (uint256 i = 0; i !=  maxMintableWithGoo + 1; i++) {
             vm.warp(block.timestamp + 1 days);
 
             if (i == maxMintableWithGoo) vm.expectRevert("UNDEFINED");
@@ -1071,7 +1071,7 @@ contract ArtGobblersTest is DSTestPlus {
     function testLongRunningMintMaxReserved() public {
         uint256 maxMintableWithGoo = gobblers.MAX_MINTABLE();
 
-        for (uint256 i = 0; i < maxMintableWithGoo; i++) {
+        for (uint256 i = 0; i !=  maxMintableWithGoo; i++) {
             vm.warp(block.timestamp + 1 days);
             uint256 cost = gobblers.gobblerPrice();
             vm.prank(address(gobblers));
@@ -1087,7 +1087,7 @@ contract ArtGobblersTest is DSTestPlus {
     function testLongRunningMintMaxTeamRevert() public {
         uint256 maxMintableWithGoo = gobblers.MAX_MINTABLE();
 
-        for (uint256 i = 0; i < maxMintableWithGoo; i++) {
+        for (uint256 i = 0; i !=  maxMintableWithGoo; i++) {
             vm.warp(block.timestamp + 1 days);
             uint256 cost = gobblers.gobblerPrice();
             vm.prank(address(gobblers));
@@ -1108,7 +1108,7 @@ contract ArtGobblersTest is DSTestPlus {
 
     /// @notice Mint a number of gobblers to the given address
     function mintGobblerToAddress(address addr, uint256 num) internal {
-        for (uint256 i = 0; i < num; i++) {
+        for (uint256 i = 0; i != num; i++) {
             vm.startPrank(address(gobblers));
             goo.mintForGobblers(addr, gobblers.gobblerPrice());
             vm.stopPrank();
diff --git a/test/Benchmarks.t.sol b/test/Benchmarks.t.sol
index d8e40a2..34cc8d5 100644
--- a/test/Benchmarks.t.sol
+++ b/test/Benchmarks.t.sol
@@ -136,7 +136,7 @@ contract BenchmarksTest is DSTest {
         uint256 legendaryGobblerCost = legendaryCost;
 
         uint256[] memory ids = new uint256[](legendaryGobblerCost);
-        for (uint256 i = 0; i < legendaryGobblerCost; i++) ids[i] = i + 1;
+        for (uint256 i = 0; i !=  legendaryGobblerCost; i++) ids[i] = i + 1;
 
         gobblers.mintLegendaryGobbler(ids);
     }
@@ -150,7 +150,7 @@ contract BenchmarksTest is DSTest {
     }
 
     function mintGobblerToAddress(address addr, uint256 num) internal {
-        for (uint256 i = 0; i < num; i++) {
+        for (uint256 i = 0; i !=  num; i++) {
             vm.startPrank(address(gobblers));
             goo.mintForGobblers(addr, gobblers.gobblerPrice());
             vm.stopPrank();
@@ -161,7 +161,7 @@ contract BenchmarksTest is DSTest {
     }
 
     function mintPageToAddress(address addr, uint256 num) internal {
-        for (uint256 i = 0; i < num; i++) {
+        for (uint256 i = 0; i !=  num; i++) {
             vm.startPrank(address(gobblers));
             goo.mintForGobblers(addr, pages.pagePrice());
             vm.stopPrank();
diff --git a/test/GobblerReserve.t.sol b/test/GobblerReserve.t.sol
index 56f90fe..b8afb43 100644
--- a/test/GobblerReserve.t.sol
+++ b/test/GobblerReserve.t.sol
@@ -117,7 +117,7 @@ contract GobblerReserveTest is DSTestPlus {
 
     /// @notice Mint a number of gobblers to the given address
     function mintGobblerToAddress(address addr, uint256 num) internal {
-        for (uint256 i = 0; i < num; i++) {
+        for (uint256 i = 0; i !=  num; i++) {
             vm.startPrank(address(gobblers));
             goo.mintForGobblers(addr, gobblers.gobblerPrice());
             vm.stopPrank();
diff --git a/test/Pages.t.sol b/test/Pages.t.sol
index 74a4e37..73a2dab 100644
--- a/test/Pages.t.sol
+++ b/test/Pages.t.sol
@@ -144,7 +144,7 @@ contract PagesTest is DSTestPlus {
 
         uint256 targetPrice = uint256(pages.targetPrice());
 
-        for (uint256 i = 0; i < numMint; i++) {
+        for (uint256 i = 0; i !=  numMint; i++) {
             uint256 price = pages.pagePrice();
             goo.mintForGobblers(user, price);
             vm.prank(user);
@@ -166,7 +166,7 @@ contract PagesTest is DSTestPlus {
 
         uint256 targetPrice = uint256(pages.targetPrice());
 
-        for (uint256 i = 0; i < numMint; i++) {
+        for (uint256 i = 0; i !=  numMint; i++) {
             uint256 price = pages.pagePrice();
             goo.mintForGobblers(user, price);
             vm.prank(user);
@@ -195,7 +195,7 @@ contract PagesTest is DSTestPlus {
 
     /// @notice Mint a number of pages to the given address
     function mintPageToAddress(address addr, uint256 num) internal {
-        for (uint256 i = 0; i < num; i++) {
+        for (uint256 i = 0; i !=  num; i++) {
             goo.mintForGobblers(addr, pages.pagePrice());
 
             vm.prank(addr);
diff --git a/test/RandProvider.t.sol b/test/RandProvider.t.sol
index cd21b1c..1220246 100644
--- a/test/RandProvider.t.sol
+++ b/test/RandProvider.t.sol
@@ -154,7 +154,7 @@ contract RandProviderTest is DSTestPlus {
 
     /// @notice Mint a number of gobblers to the given address
     function mintGobblerToAddress(address addr, uint256 num) internal {
-        for (uint256 i = 0; i < num; i++) {
+        for (uint256 i = 0; i !=  num; i++) {
             vm.startPrank(address(gobblers));
             goo.mintForGobblers(addr, gobblers.gobblerPrice());
             vm.stopPrank();
diff --git a/.gas-snapshot b/.gas-snapshot
index c494841..9bb50a4 100644
--- a/.gas-snapshot
+++ b/.gas-snapshot
@@ -42,7 +42,7 @@ ArtGobblersTest:testMintFromMintlistBeforeMintingStarts() (gas: 13877)
 ArtGobblersTest:testMintInsufficientBalance() (gas: 22876)
 ArtGobblersTest:testMintLegendaryGobbler() (gas: 41075776)
 ArtGobblersTest:testMintLegendaryGobblerWithInsufficientCost() (gas: 40766132)
-ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41267419)
+ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41249420)
 ArtGobblersTest:testMintLegendaryGobblerWithUnownedId() (gas: 40922878)
 ArtGobblersTest:testMintLegendaryGobblersExpectedIds() (gas: 264182200)
 ArtGobblersTest:testMintNotInMintlist() (gas: 11229)
diff --git a/test/ArtGobblers.t.sol b/test/ArtGobblers.t.sol
index 174d7b9..1c27324 100644
--- a/test/ArtGobblers.t.sol
+++ b/test/ArtGobblers.t.sol
@@ -523,12 +523,12 @@ contract ArtGobblersTest is DSTestPlus {
         setRandomnessAndReveal(cost, "seed");
         uint256 emissionMultipleSum;
         //add more ids than necessary
-        for (uint256 curId = 1; curId <= cost + 10; curId++) {
+        uint256 new_cost = cost + 10;
+        for (uint256 curId = 1; curId <= new_cost; curId++) {
             ids.push(curId);
             assertEq(gobblers.ownerOf(curId), users[0]);
             emissionMultipleSum += gobblers.getGobblerEmissionMultiple(curId);
         }
-
         vm.prank(users[0]);
         gobblers.mintLegendaryGobbler(ids);
 
@@ -538,7 +538,7 @@ contract ArtGobblersTest is DSTestPlus {
             gobblers.ownerOf(curId);
         }
         //check extra tokens were not burned
-        for (uint256 curId = cost + 1; curId <= cost + 10; curId++) {
+        for (uint256 curId = new_cost+1 ; curId <= new_cost; curId++) {
             assertEq(gobblers.ownerOf(curId), users[0]);
         }
     }
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..009ca7f 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -461,7 +461,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             legendaryGobblerAuctionData.startPrice = uint120(
                 cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
             );
-            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
+            ++legendaryGobblerAuctionData.numSold; // Increment the # of legendaries sold.
 
             // If gobblerIds has 1,000 elements this should cost around ~270,000 gas.
             emit LegendaryGobblerMinted(msg.sender, gobblerId, gobblerIds[:cost]);
@@ -903,14 +903,14 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             getUserData[from].lastBalance = uint128(gooBalance(from));
             getUserData[from].lastTimestamp = uint64(block.timestamp);
             getUserData[from].emissionMultiple -= emissionMultiple;
-            getUserData[from].gobblersOwned -= 1;
+            --getUserData[from].gobblersOwned;
 
             // We update their last balance before updating their emission multiple to avoid
             // overpaying them by retroactively applying their new (higher) emission multiple.
             getUserData[to].lastBalance = uint128(gooBalance(to));
             getUserData[to].lastTimestamp = uint64(block.timestamp);
             getUserData[to].emissionMultiple += emissionMultiple;
-            getUserData[to].gobblersOwned += 1;
+            ++getUserData[to].gobblersOwned;
         }
 
         emit Transfer(from, to, id);
diff --git a/.gas-snapshot b/.gas-snapshot
index c494841..5a794bb 100644
--- a/.gas-snapshot
+++ b/.gas-snapshot
@@ -1,79 +1,79 @@
 ArtGobblersTest:testCanMintMultipleReserved() (gas: 1363063)
-ArtGobblersTest:testCanMintPageFromVirtualBalance() (gas: 299517)
+ArtGobblersTest:testCanMintPageFromVirtualBalance() (gas: 299520)
 ArtGobblersTest:testCanMintReserved() (gas: 670320)
-ArtGobblersTest:testCannotMintLegendaryWithLegendary() (gas: 78518641)
-ArtGobblersTest:testCannotMintPageWithInsufficientBalance() (gas: 217059)
-ArtGobblersTest:testCannotReuseSeedForReveal() (gas: 275715)
+ArtGobblersTest:testCannotMintLegendaryWithLegendary() (gas: 78518528)
+ArtGobblersTest:testCannotMintPageWithInsufficientBalance() (gas: 217057)
+ArtGobblersTest:testCannotReuseSeedForReveal() (gas: 275713)
 ArtGobblersTest:testCannotRevealMoreGobblersThanRemainingToBeRevealed() (gas: 299793)
-ArtGobblersTest:testCantAddMoreGooThanOwned() (gas: 209640)
+ArtGobblersTest:testCantAddMoreGooThanOwned() (gas: 209638)
 ArtGobblersTest:testCantFeed1155As721() (gas: 1754349)
 ArtGobblersTest:testCantFeed721As1155() (gas: 235316)
 ArtGobblersTest:testCantFeedGobblers() (gas: 179190)
 ArtGobblersTest:testCantFeedUnownedArt() (gas: 140301)
 ArtGobblersTest:testCantMintTooFastReserved() (gas: 1239121)
 ArtGobblersTest:testCantMintTooFastReservedOneByOne() (gas: 6429888)
-ArtGobblersTest:testCantRemoveGoo() (gas: 210496)
-ArtGobblersTest:testCantSetRandomSeedWithoutRevealing() (gas: 286963)
+ArtGobblersTest:testCantRemoveGoo() (gas: 210494)
+ArtGobblersTest:testCantSetRandomSeedWithoutRevealing() (gas: 286960)
 ArtGobblersTest:testCantgobbleToUnownedGobbler() (gas: 115187)
 ArtGobblersTest:testDoesNotAllowRevealingZero() (gas: 16278)
 ArtGobblersTest:testDoesNotRevertEarly() (gas: 7735)
 ArtGobblersTest:testDoesRevertWhenExpected() (gas: 10079)
-ArtGobblersTest:testEmissionMultipleUpdatesAfterTransfer() (gas: 253644)
+ArtGobblersTest:testEmissionMultipleUpdatesAfterTransfer() (gas: 253642)
 ArtGobblersTest:testFeeding1155() (gas: 1763713)
 ArtGobblersTest:testFeedingArt() (gas: 258468)
 ArtGobblersTest:testFeedingMultiple1155Copies() (gas: 1805578)
-ArtGobblersTest:testGobblerBalancesAfterTransfer() (gas: 254049)
-ArtGobblersTest:testGooAddition() (gas: 243272)
-ArtGobblersTest:testGooRemoval() (gas: 262382)
+ArtGobblersTest:testGobblerBalancesAfterTransfer() (gas: 254047)
+ArtGobblersTest:testGooAddition() (gas: 243274)
+ArtGobblersTest:testGooRemoval() (gas: 262385)
 ArtGobblersTest:testInitialGobblerPrice() (gas: 14640)
 ArtGobblersTest:testLegendaryGobblerFinalPrice() (gas: 75838529)
 ArtGobblersTest:testLegendaryGobblerMidPrice() (gas: 56917944)
-ArtGobblersTest:testLegendaryGobblerMinStartPrice() (gas: 75885780)
+ArtGobblersTest:testLegendaryGobblerMinStartPrice() (gas: 75885778)
 ArtGobblersTest:testLegendaryGobblerMintBeforeStart() (gas: 22126)
 ArtGobblersTest:testLegendaryGobblerPastFinalPrice() (gas: 71635852)
 ArtGobblersTest:testLegendaryGobblerTargetPrice() (gas: 37936155)
-ArtGobblersTest:testMintFreeLegendaryGobbler() (gas: 75872939)
-ArtGobblersTest:testMintFreeLegendaryGobblerPastInterval() (gas: 113792206)
+ArtGobblersTest:testMintFreeLegendaryGobbler() (gas: 75872936)
+ArtGobblersTest:testMintFreeLegendaryGobblerPastInterval() (gas: 113792204)
 ArtGobblersTest:testMintFromBalanceInsufficient() (gas: 23701)
 ArtGobblersTest:testMintFromGoo() (gas: 111224)
-ArtGobblersTest:testMintFromGooBalance() (gas: 252966)
+ArtGobblersTest:testMintFromGooBalance() (gas: 252969)
 ArtGobblersTest:testMintFromMintlist() (gas: 108203)
 ArtGobblersTest:testMintFromMintlistBeforeMintingStarts() (gas: 13877)
 ArtGobblersTest:testMintInsufficientBalance() (gas: 22876)
-ArtGobblersTest:testMintLegendaryGobbler() (gas: 41075776)
-ArtGobblersTest:testMintLegendaryGobblerWithInsufficientCost() (gas: 40766132)
-ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41267419)
-ArtGobblersTest:testMintLegendaryGobblerWithUnownedId() (gas: 40922878)
-ArtGobblersTest:testMintLegendaryGobblersExpectedIds() (gas: 264182200)
+ArtGobblersTest:testMintLegendaryGobbler() (gas: 41071964)
+ArtGobblersTest:testMintLegendaryGobblerWithInsufficientCost() (gas: 40766022)
+ArtGobblersTest:testMintLegendaryGobblerWithSlippage() (gas: 41263608)
+ArtGobblersTest:testMintLegendaryGobblerWithUnownedId() (gas: 40919125)
+ArtGobblersTest:testMintLegendaryGobblersExpectedIds() (gas: 264182170)
 ArtGobblersTest:testMintNotInMintlist() (gas: 11229)
 ArtGobblersTest:testMintPriceExceededMax() (gas: 72095)
 ArtGobblersTest:testMintReservedGobblersFailsWithNoMints() (gas: 32661)
-ArtGobblersTest:testMintedLegendaryURI() (gas: 75895832)
+ArtGobblersTest:testMintedLegendaryURI() (gas: 75895829)
 ArtGobblersTest:testMintingFromMintlistTwiceFails() (gas: 107465)
-ArtGobblersTest:testMultiReveal() (gas: 8475477)
+ArtGobblersTest:testMultiReveal() (gas: 8475397)
 ArtGobblersTest:testPricingBasic() (gas: 53959904)
 ArtGobblersTest:testRevealDelayInitialMint() (gas: 114733)
-ArtGobblersTest:testRevealDelayRecurring() (gas: 259136)
-ArtGobblersTest:testRevealedUri() (gas: 217542)
-ArtGobblersTest:testSimpleRewards() (gas: 218517)
-ArtGobblersTest:testSnapshotDoesNotAffectBalance() (gas: 395277)
+ArtGobblersTest:testRevealDelayRecurring() (gas: 259135)
+ArtGobblersTest:testRevealedUri() (gas: 217540)
+ArtGobblersTest:testSimpleRewards() (gas: 218515)
+ArtGobblersTest:testSnapshotDoesNotAffectBalance() (gas: 395293)
 ArtGobblersTest:testUnmintedLegendaryUri() (gas: 25309)
 ArtGobblersTest:testUnmintedUri() (gas: 13435)
 ArtGobblersTest:testUnrevealedUri() (gas: 117119)
-BenchmarksTest:testAddGoo() (gas: 31862)
+BenchmarksTest:testAddGoo() (gas: 31867)
 BenchmarksTest:testGobblerPrice() (gas: 8986)
 BenchmarksTest:testGooBalance() (gas: 11831)
 BenchmarksTest:testLegendaryGobblersPrice() (gas: 10005)
 BenchmarksTest:testMintCommunityPages() (gas: 58998)
 BenchmarksTest:testMintGobbler() (gas: 58811)
-BenchmarksTest:testMintGobblerUsingVirtualBalance() (gas: 49718)
-BenchmarksTest:testMintLegendaryGobbler() (gas: 476972)
+BenchmarksTest:testMintGobblerUsingVirtualBalance() (gas: 49723)
+BenchmarksTest:testMintLegendaryGobbler() (gas: 473272)
 BenchmarksTest:testMintPage() (gas: 58812)
-BenchmarksTest:testMintPageUsingVirtualBalance() (gas: 57854)
+BenchmarksTest:testMintPageUsingVirtualBalance() (gas: 57859)
 BenchmarksTest:testMintReservedGobblers() (gas: 105761)
 BenchmarksTest:testPagePrice() (gas: 9108)
-BenchmarksTest:testRemoveGoo() (gas: 31872)
-BenchmarksTest:testRevealGobblers() (gas: 2913008)
+BenchmarksTest:testRemoveGoo() (gas: 31877)
+BenchmarksTest:testRevealGobblers() (gas: 2912803)
 BenchmarksTest:testTransferGobbler() (gas: 49062)
 GobblerReserveTest:testCanWithdraw() (gas: 763845)
 GooTest:testBurnAllowed() (gas: 56314)
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..d545015 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -434,11 +434,13 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
 
-                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
+                GobblerData storage pointer = getGobblerData[id];
 
-                burnedMultipleTotal += getGobblerData[id].emissionMultiple;
+                require(pointer.owner == msg.sender, "WRONG_FROM");
 
-                emit Transfer(msg.sender, getGobblerData[id].owner = address(0), id);
+                burnedMultipleTotal += pointer.emissionMultiple;
+
+                emit Transfer(msg.sender, pointer.owner = address(0), id);
             }
 
             /*//////////////////////////////////////////////////////////////
@@ -447,15 +449,15 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
             // The legendary's emissionMultiple is 2x the sum of the multiples of the gobblers burned.
             getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
-
+            UserData storage new_pointer_ = getUserData[msg.sender];
             // Update the user's user data struct in one big batch. We add burnedMultipleTotal to their
             // emission multiple (not burnedMultipleTotal * 2) to account for the standard gobblers that
             // were burned and hence should have their multiples subtracted from the user's total multiple.
-            getUserData[msg.sender].lastBalance = uint128(gooBalance(msg.sender)); // Checkpoint balance.
-            getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
-            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
+            new_pointer_.lastBalance = uint128(gooBalance(msg.sender)); // Checkpoint balance.
+            new_pointer_.lastTimestamp = uint64(block.timestamp); // Store time alongside it.
+            new_pointer_.emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
             // We subtract the amount of gobblers burned, and then add 1 to factor in the new legendary.
-            getUserData[msg.sender].gobblersOwned = uint32(getUserData[msg.sender].gobblersOwned - cost + 1);
+            new_pointer_.gobblersOwned = uint32(new_pointer_.gobblersOwned - cost + 1);
 
             // New start price is the max of LEGENDARY_GOBBLER_INITIAL_START_PRICE and cost * 2.
             legendaryGobblerAuctionData.startPrice = uint120(
@@ -612,10 +614,9 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                 //////////////////////////////////////////////////////////////*/
 
                 // Get the index of the swap id.
-                uint64 swapIndex = getGobblerData[swapId].idx == 0
-                    ? uint64(swapId) // Hasn't been shuffled before.
-                    : getGobblerData[swapId].idx; // Shuffled before.
-
+                uint64 swapIndex = uint64(swapId);
+                if (getGobblerData[swapId].idx == 0) {}
+                else {swapIndex =  getGobblerData[swapId].idx;}
                 // Get the owner of the current id.
                 address currentIdOwner = getGobblerData[currentId].owner;
 
@@ -646,8 +647,9 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                 }
 
                 // Swap the index and multiple of the current id.
-                getGobblerData[currentId].idx = swapIndex;
-                getGobblerData[currentId].emissionMultiple = uint32(newCurrentIdMultiple);
+                GobblerData storage new_pointer = getGobblerData[currentId];
+                new_pointer.idx = swapIndex;
+                new_pointer.emissionMultiple = uint32(newCurrentIdMultiple);
 
                 // Swap the index of the swap id.
                 getGobblerData[swapId].idx = currentIndex;
@@ -655,11 +657,11 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                 /*//////////////////////////////////////////////////////////////
                                    UPDATE CURRENT ID MULTIPLE
                 //////////////////////////////////////////////////////////////*/
-
+                UserData storage new_pointer_user = getUserData[currentIdOwner];
                 // Update the user data for the owner of the current id.
-                getUserData[currentIdOwner].lastBalance = uint128(gooBalance(currentIdOwner));
-                getUserData[currentIdOwner].lastTimestamp = uint64(block.timestamp);
-                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
+                new_pointer_user.lastBalance = uint128(gooBalance(currentIdOwner));
+                new_pointer_user.lastTimestamp = uint64(block.timestamp);
+                new_pointer_user.emissionMultiple += uint32(newCurrentIdMultiple);
 
                 // Update the random seed to choose a new distance for the next iteration.
                 // It is critical that we cast to uint64 here, as otherwise the random seed
@@ -820,10 +822,10 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         uint256 updatedBalance = updateType == GooBalanceUpdateType.INCREASE
             ? gooBalance(user) + gooAmount
             : gooBalance(user) - gooAmount;
-
+        UserData storage new_pointer = getUserData[user];
         // Snapshot the user's new goo balance with the current timestamp.
-        getUserData[user].lastBalance = uint128(updatedBalance);
-        getUserData[user].lastTimestamp = uint64(block.timestamp);
+        new_pointer.lastBalance = uint128(updatedBalance);
+        new_pointer.lastTimestamp = uint64(block.timestamp);
 
         emit GooBalanceUpdated(user, updatedBalance);
     }