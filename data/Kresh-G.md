### GAS OPTIMIZED CYCLE PATTERN
```
unchecked {
    for (uint256 i; i != n; ++i) {}
}
```
Inside the code:
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..1b26c8b 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -429,7 +429,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
             uint256 id; // Storing outside the loop saves ~7 gas per iteration.
 
-            for (uint256 i = 0; i < cost; ++i) {
+            for (uint256 i; i != cost; ++i) {
                 id = gobblerIds[i];
 
                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
@@ -589,7 +589,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         // Implements a Knuth shuffle. If something in
         // here can overflow, we've got bigger problems.
         unchecked {
-            for (uint256 i = 0; i < numGobblers; ++i) {
+            for (uint256 i; i != numGobblers; ++i) {
                 /*//////////////////////////////////////////////////////////////
                                       DETERMINE RANDOM SWAP
                 //////////////////////////////////////////////////////////////*/
diff --git a/src/Pages.sol b/src/Pages.sol
index 0d4ac88..29aeedb 100644
--- a/src/Pages.sol
+++ b/src/Pages.sol
@@ -248,7 +248,7 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
             if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();
 
             // Mint the pages to the community reserve while updating lastMintedPageId.
-            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
+            for (uint256 i; i != numPages; ++i) _mint(community, ++lastMintedPageId);
 
             currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id.
 
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..e51942a 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -34,7 +34,7 @@ contract GobblerReserve is Owned {
     function withdraw(address to, uint256[] calldata ids) external onlyOwner {
         // This is quite inefficient, but it's okay because this is not a hot path.
         unchecked {
-            for (uint256 i = 0; i < ids.length; i++) {
+            for (uint256 i; i != ids.length; ++i) {
                 artGobblers.transferFrom(address(this), to, ids[i]);
             }
         }
diff --git a/src/utils/token/GobblersERC1155B.sol b/src/utils/token/GobblersERC1155B.sol
index ccf5cd0..af8f4e4 100644
--- a/src/utils/token/GobblersERC1155B.sol
+++ b/src/utils/token/GobblersERC1155B.sol
@@ -111,7 +111,7 @@ abstract contract GobblersERC1155B {
         // Unchecked because the only math done is incrementing
         // the array index counter which cannot possibly overflow.
         unchecked {
-            for (uint256 i = 0; i < owners.length; ++i) {
+            for (uint256 i; i != owners.length; ++i) {
                 balances[i] = balanceOf(owners[i], ids[i]);
             }
         }
@@ -170,7 +170,7 @@ abstract contract GobblersERC1155B {
 
         // Counter overflow is unrealistic on human timescales.
         unchecked {
-            for (uint256 i = 0; i < amount; ++i) {
+            for (uint256 i; i != amount; ++i) {
                 ids[i] = ++lastMintedId; // Increment id while setting.
 
                 amounts[i] = 1; // ERC1155B amounts are always 1.
diff --git a/src/utils/token/GobblersERC721.sol b/src/utils/token/GobblersERC721.sol
index f2d0647..8e90b8a 100644
--- a/src/utils/token/GobblersERC721.sol
+++ b/src/utils/token/GobblersERC721.sol
@@ -183,7 +183,7 @@ abstract contract GobblersERC721 {
         unchecked {
             getUserData[to].gobblersOwned += uint32(amount);
 
-            for (uint256 i = 0; i < amount; ++i) {
+            for (uint256 i; i != amount; ++i) {
                 getGobblerData[++lastMintedId].owner = to;
 
                 emit Transfer(address(0), to, lastMintedId);
```

### GAS OPTIMIZED CYCLE WITH CACHING
Any calculations/loadings mut be done out of the cycle initialization
Inside the code:
```diff
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
index ccf5cd0..fde3e0c 100644
--- a/src/utils/token/GobblersERC1155B.sol
+++ b/src/utils/token/GobblersERC1155B.sol
@@ -111,7 +111,8 @@ abstract contract GobblersERC1155B {
         // Unchecked because the only math done is incrementing
         // the array index counter which cannot possibly overflow.
         unchecked {
-            for (uint256 i = 0; i < owners.length; ++i) {
+            uint256 length = owners.length;
+            for (uint256 i = 0; i < length; ++i) {
                 balances[i] = balanceOf(owners[i], ids[i]);
             }
         }
```

### REPLACE != to == WHERE IT IS POSSIBLE
*Don't replace != 0 because the optimizer optimizes this structure very well
Inside the code:
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..6a357f1 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -545,14 +545,16 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
     /// @param randomness The 256 bits of verifiable randomness provided by the rand provider.
     function acceptRandomSeed(bytes32, uint256 randomness) external {
         // The caller must be the randomness provider, revert in the case it's not.
-        if (msg.sender != address(randProvider)) revert NotRandProvider();
+        if (msg.sender == address(randProvider)) {
+            // The unchecked cast to uint64 is equivalent to moduloing the randomness by 2**64.
+            gobblerRevealsData.randomSeed = uint64(randomness); // 64 bits of randomness is plenty.
 
-        // The unchecked cast to uint64 is equivalent to moduloing the randomness by 2**64.
-        gobblerRevealsData.randomSeed = uint64(randomness); // 64 bits of randomness is plenty.
+            gobblerRevealsData.waitingForSeed = false; // We have the seed now, open up reveals.
 
-        gobblerRevealsData.waitingForSeed = false; // We have the seed now, open up reveals.
-
-        emit RandomnessFulfilled(randomness);
+            emit RandomnessFulfilled(randomness);
+        } else {
+            revert NotRandProvider();
+        }
     }
 
     /// @notice Upgrade the rand provider contract. Useful if current VRF is sunset.
@@ -792,11 +794,13 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
     /// @param gooAmount The amount of goo to burn from the user's virtual balance.
     function burnGooForPages(address user, uint256 gooAmount) external {
         // The caller must be the Pages contract, revert otherwise.
-        if (msg.sender != address(pages)) revert UnauthorizedCaller(msg.sender);
-
-        // Burn the requested amount of goo from the user's virtual goo balance.
-        // Will revert if the user doesn't have enough goo in their virtual balance.
-        updateUserGooBalance(user, gooAmount, GooBalanceUpdateType.DECREASE);
+        if (msg.sender == address(pages)) {
+            // Burn the requested amount of goo from the user's virtual goo balance.
+            // Will revert if the user doesn't have enough goo in their virtual balance.
+            updateUserGooBalance(user, gooAmount, GooBalanceUpdateType.DECREASE);
+        } else {
+            revert UnauthorizedCaller(msg.sender);
+        }
     }
 
     /// @dev An enum for representing whether to
diff --git a/src/Goo.sol b/src/Goo.sol
index bcc459c..83d75b9 100644
--- a/src/Goo.sol
+++ b/src/Goo.sol
@@ -90,9 +90,11 @@ contract Goo is ERC20("Goo", "GOO", 18) {
 
     /// @notice Requires caller address to match user address.
     modifier only(address user) {
-        if (msg.sender != user) revert Unauthorized();
-
-        _;
+        if (msg.sender == user) {
+            _;
+        } else {
+            revert Unauthorized();
+        }
     }
 
     /// @notice Mint any amount of goo to a user. Can only be called by ArtGobblers.
```

### CACHE THE SAME CALCULATIONS/LOADINGS
Inside the code:
```diff
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

### MAKE POINTER TO ELEMENTS OF MAPPING IF THEY REPEAT
Inside the code:
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..fb52530 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -433,12 +433,12 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                 id = gobblerIds[i];

                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
+                GobblerData storage curGobblerData = getGobblerData[id];
+                require(curGobblerData.owner == msg.sender, "WRONG_FROM");

-                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
+                burnedMultipleTotal += curGobblerData.emissionMultiple;

-                burnedMultipleTotal += getGobblerData[id].emissionMultiple;
-
-                emit Transfer(msg.sender, getGobblerData[id].owner = address(0), id);
+                emit Transfer(msg.sender, curGobblerData.owner = address(0), id);
             }

             /*//////////////////////////////////////////////////////////////
@@ -451,11 +451,12 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             // Update the user's user data struct in one big batch. We add burnedMultipleTotal to their
             // emission multiple (not burnedMultipleTotal * 2) to account for the standard gobblers that
             // were burned and hence should have their multiples subtracted from the user's total multiple.
-            getUserData[msg.sender].lastBalance = uint128(gooBalance(msg.sender)); // Checkpoint balance.
-            getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
-            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
+            UserData storage userData = getUserData[msg.sender];
+            userData.lastBalance = uint128(gooBalance(msg.sender)); // Checkpoint balance.
+            userData.lastTimestamp = uint64(block.timestamp); // Store time alongside it.
+            userData.emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
             // We subtract the amount of gobblers burned, and then add 1 to factor in the new legendary.
-            getUserData[msg.sender].gobblersOwned = uint32(getUserData[msg.sender].gobblersOwned - cost + 1);
+            userData.gobblersOwned = uint32(userData.gobblersOwned - cost + 1);

             // New start price is the max of LEGENDARY_GOBBLER_INITIAL_START_PRICE and cost * 2.
             legendaryGobblerAuctionData.startPrice = uint120(
@@ -612,17 +613,19 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                 //////////////////////////////////////////////////////////////*/

                 // Get the index of the swap id.
-                uint64 swapIndex = getGobblerData[swapId].idx == 0
+                GobblerData storage swapGobblerData = getGobblerData[swapId];
+                uint64 swapIndex = swapGobblerData.idx == 0
                     ? uint64(swapId) // Hasn't been shuffled before.
-                    : getGobblerData[swapId].idx; // Shuffled before.
+                    : swapGobblerData.idx; // Shuffled before.

                 // Get the owner of the current id.
                 address currentIdOwner = getGobblerData[currentId].owner;

+                GobblerData storage currentGobblerData = getGobblerData[currentId];
                 // Get the index of the current id.
-                uint64 currentIndex = getGobblerData[currentId].idx == 0
+                uint64 currentIndex = currentGobblerData.idx == 0
                     ? uint64(currentId) // Hasn't been shuffled before.
-                    : getGobblerData[currentId].idx; // Shuffled before.
+                    : currentGobblerData.idx; // Shuffled before.

                 /*//////////////////////////////////////////////////////////////
                                   SWAP INDICES AND SET MULTIPLE
@@ -646,20 +649,21 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                 }

                 // Swap the index and multiple of the current id.
-                getGobblerData[currentId].idx = swapIndex;
-                getGobblerData[currentId].emissionMultiple = uint32(newCurrentIdMultiple);
+                currentGobblerData.idx = swapIndex;
+                currentGobblerData.emissionMultiple = uint32(newCurrentIdMultiple);

                 // Swap the index of the swap id.
-                getGobblerData[swapId].idx = currentIndex;
+                swapGobblerData.idx = currentIndex;

                 /*//////////////////////////////////////////////////////////////
                                    UPDATE CURRENT ID MULTIPLE
                 //////////////////////////////////////////////////////////////*/

                 // Update the user data for the owner of the current id.
-                getUserData[currentIdOwner].lastBalance = uint128(gooBalance(currentIdOwner));
-                getUserData[currentIdOwner].lastTimestamp = uint64(block.timestamp);
-                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
+                UserData storage userData = getUserData[currentIdOwner];
+                userData.lastBalance = uint128(gooBalance(currentIdOwner));
+                userData.lastTimestamp = uint64(block.timestamp);
+                userData.emissionMultiple += uint32(newCurrentIdMultiple);

                 // Update the random seed to choose a new distance for the next iteration.
                 // It is critical that we cast to uint64 here, as otherwise the random seed
@@ -882,7 +886,8 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         address to,
         uint256 id
     ) public override {
-        require(from == getGobblerData[id].owner, "WRONG_FROM");
+        GobblerData storage gobblerData = getGobblerData[id];
+        require(from == gobblerData.owner, "WRONG_FROM");

         require(to != address(0), "INVALID_RECIPIENT");

@@ -893,10 +898,10 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv

         delete getApproved[id];

-        getGobblerData[id].owner = to;
+        gobblerData.owner = to;

         unchecked {
-            uint32 emissionMultiple = getGobblerData[id].emissionMultiple; // Caching saves gas.
+            uint32 emissionMultiple = gobblerData.emissionMultiple; // Caching saves gas.

             // We update their last balance before updating their emission multiple to avoid
             // penalizing them by retroactively applying their new (lower) emission multiple.
```

### REPLACE TERNARY OPERATORS IN MOST CASES
Inside the code:
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..b538f38 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -376,9 +376,11 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
         // Decrement the user's goo balance by the current
         // price, either from virtual balance or ERC20 balance.
-        useVirtualBalance
-            ? updateUserGooBalance(msg.sender, currentPrice, GooBalanceUpdateType.DECREASE)
-            : goo.burnForGobblers(msg.sender, currentPrice);
+        if (useVirtualBalance) {
+            updateUserGooBalance(msg.sender, currentPrice, GooBalanceUpdateType.DECREASE);
+        } else {
+            goo.burnForGobblers(msg.sender, currentPrice);
+        }
 
         unchecked {
             ++numMintedFromGoo; // Overflow should be impossible due to the supply cap.
@@ -458,9 +460,11 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             getUserData[msg.sender].gobblersOwned = uint32(getUserData[msg.sender].gobblersOwned - cost + 1);
 
             // New start price is the max of LEGENDARY_GOBBLER_INITIAL_START_PRICE and cost * 2.
-            legendaryGobblerAuctionData.startPrice = uint120(
-                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
-            );
+            if (cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2) {
+                legendaryGobblerAuctionData.startPrice = uint120(LEGENDARY_GOBBLER_INITIAL_START_PRICE);
+            } else {
+                legendaryGobblerAuctionData.startPrice = uint120(cost * 2);
+            }
             legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
 
             // If gobblerIds has 1,000 elements this should cost around ~270,000 gas.
@@ -612,17 +616,23 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                 //////////////////////////////////////////////////////////////*/
 
                 // Get the index of the swap id.
-                uint64 swapIndex = getGobblerData[swapId].idx == 0
-                    ? uint64(swapId) // Hasn't been shuffled before.
-                    : getGobblerData[swapId].idx; // Shuffled before.
+                uint64 swapIndex;
+                if (getGobblerData[swapId].idx != 0) {
+                    swapIndex = getGobblerData[swapId].idx; // Shuffled before.
+                } else {
+                    swapIndex = uint64(swapId); // Hasn't been shuffled before.
+                }
 
                 // Get the owner of the current id.
                 address currentIdOwner = getGobblerData[currentId].owner;
 
                 // Get the index of the current id.
-                uint64 currentIndex = getGobblerData[currentId].idx == 0
-                    ? uint64(currentId) // Hasn't been shuffled before.
-                    : getGobblerData[currentId].idx; // Shuffled before.
+                uint64 currentIndex;
+                if (getGobblerData[currentId].idx != 0) {
+                    currentIndex = getGobblerData[currentId].idx; // Shuffled before.
+                } else {
+                    currentIndex = uint64(currentId); // Hasn't been shuffled before.
+                }
 
                 /*//////////////////////////////////////////////////////////////
                                   SWAP INDICES AND SET MULTIPLE
@@ -817,9 +827,14 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
     ) internal {
         // Will revert due to underflow if we're decreasing by more than the user's current balance.
         // Don't need to do checked addition in the increase case, but we do it anyway for convenience.
-        uint256 updatedBalance = updateType == GooBalanceUpdateType.INCREASE
-            ? gooBalance(user) + gooAmount
-            : gooBalance(user) - gooAmount;
+        uint256 updatedBalance;
+        if (updateType == GooBalanceUpdateType.INCREASE) {
+            unchecked {
+                updatedBalance = gooBalance(user) + gooAmount;
+            }
+        } else {
+            updatedBalance = gooBalance(user) - gooAmount;
+        }
 
         // Snapshot the user's new goo balance with the current timestamp.
         getUserData[user].lastBalance = uint128(updatedBalance);
diff --git a/src/Pages.sol b/src/Pages.sol
index 0d4ac88..9760bc9 100644
--- a/src/Pages.sol
+++ b/src/Pages.sol
@@ -201,9 +201,11 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
 
         // Decrement the user's goo balance by the current
         // price, either from virtual balance or ERC20 balance.
-        useVirtualBalance
-            ? artGobblers.burnGooForPages(msg.sender, currentPrice)
-            : goo.burnForPages(msg.sender, currentPrice);
+        if (useVirtualBalance) {
+            artGobblers.burnGooForPages(msg.sender, currentPrice);
+        } else {
+            goo.burnForPages(msg.sender, currentPrice);
+        }
 
         unchecked {
             emit PagePurchased(msg.sender, pageId = ++currentId, currentPrice);
```

### REPALCE += 1 AND -= 1 TO ++ AND -- RESPECTIVELY
Inside the code:
```diff
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

### ADD PAYABLE TYPE TO EVERY FUNCTION IF YOU FIND THIS NEEDED
It save about 24 gas per function call
Inside the code:
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..adb45d9 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -336,7 +336,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
     /// limit is enforced during the creation of the merkle proof, which will be shared publicly.
     /// @param proof Merkle proof to verify the sender is mintlisted.
     /// @return gobblerId The id of the gobbler that was claimed.
-    function claimGobbler(bytes32[] calldata proof) external returns (uint256 gobblerId) {
+    function claimGobbler(bytes32[] calldata proof) external payable returns (uint256 gobblerId) {
         // If minting has not yet begun, revert.
         if (mintStart > block.timestamp) revert MintStartPending();
 
```

### ADD ANONYMOUS TYPE TO EVERY EVENT IF YOU FIND THIS NEEDED
Anonymous type make the event non-filterable from blockchain but it saves about 400 gas per event emition
Inside the code:
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..a950f06 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -226,7 +226,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
                                  EVENTS
     //////////////////////////////////////////////////////////////*/
 
-    event GooBalanceUpdated(address indexed user, uint256 newGooBalance);
+    event GooBalanceUpdated(address indexed user, uint256 newGooBalance) anonymous;
 
     event GobblerClaimed(address indexed user, uint256 indexed gobblerId);
     event GobblerPurchased(address indexed user, uint256 indexed gobblerId, uint256 price);
```