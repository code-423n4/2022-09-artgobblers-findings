## Fixing .length in cycle
Calling length in each iteration
Better using .length out of for cycle
```   
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
```
## Fixing of using != instead of <
!= is better than > in some cases
```
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
```
##Cycle in test
cost + 10 in each iteration take some gas
```
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
```
## Incerements/Decrements
++x is better than x += 1
some cases in code:
```
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
```
## Ternary operator
In some cases it's better to use if then else condition instead of ternary operators 
```
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..d545015 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
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
```
## New storage
creating a new pointer instead of x[i] construct
```
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
```
