# LOW

## Missing zero address checks

### [src/utils/GobblerReserve.sol#L24](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L24)

Missing address(0) check for `_artGobblers`

### [src/utils/token/PagesERC721.sol#L41-L43](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L43)

- Missing address(0) check for `_artGobblers`

### [src/Pages.sol#L179-L182](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L179-L182)

- Missing address(0) check for `_goo`
- Missing address(0) check for `_community`

### [src/ArtGobblers.sol#L314-L318](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L314-L318)

- Missing address(0) check for `_goo`
- Missing address(0) check for `_pages`
- Missing address(0) check for `_team`
- Missing address(0) check for `_community`
- Missing address(0) check for `_randProvider`



# QA


## Embrace the `++i` and `--i`
In some cases you are using `i++` use always the same pattern unless its necesary tou use a post increment or post decrement.


```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..c8b3689 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -429,7 +429,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
             uint256 id; // Storing outside the loop saves ~7 gas per iteration.
 
-            for (uint256 i = 0; i < cost; ++i) {
+            for (uint256 i; i < cost; ++i) {
                 id = gobblerIds[i];
 
                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
@@ -589,7 +589,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         // Implements a Knuth shuffle. If something in
         // here can overflow, we've got bigger problems.
         unchecked {
-            for (uint256 i = 0; i < numGobblers; ++i) {
+            for (uint256 i; i < numGobblers; ++i) {
                 /*//////////////////////////////////////////////////////////////
                                       DETERMINE RANDOM SWAP
                 //////////////////////////////////////////////////////////////*/
diff --git a/src/Pages.sol b/src/Pages.sol
index 0d4ac88..1a43aa2 100644
--- a/src/Pages.sol
+++ b/src/Pages.sol
@@ -248,7 +248,7 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
             if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();
 
             // Mint the pages to the community reserve while updating lastMintedPageId.
-            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
+            for (uint256 i = 0; i < numPages; ++i) _mint(community, ++lastMintedPageId);
 
             currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id.
 
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..5e2bb85 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -34,7 +34,7 @@ contract GobblerReserve is Owned {
     function withdraw(address to, uint256[] calldata ids) external onlyOwner {
         // This is quite inefficient, but it's okay because this is not a hot path.
         unchecked {
-            for (uint256 i = 0; i < ids.length; i++) {
+            for (uint256 i = 0; i < ids.length; ++i) {
                 artGobblers.transferFrom(address(this), to, ids[i]);
             }
         }
```


## Initial value of `uint256` is `0` there is no need to set it

```diff
diff --git a/src/Pages.sol b/src/Pages.sol
index 0d4ac88..8586ade 100644
--- a/src/Pages.sol
+++ b/src/Pages.sol
@@ -248,7 +248,7 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
             if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();
 
             // Mint the pages to the community reserve while updating lastMintedPageId.
-            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
+            for (uint256 i; i < numPages; i++) _mint(community, ++lastMintedPageId);
 
             currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id.
 
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..00c106b 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -34,7 +34,7 @@ contract GobblerReserve is Owned {
     function withdraw(address to, uint256[] calldata ids) external onlyOwner {
         // This is quite inefficient, but it's okay because this is not a hot path.
         unchecked {
-            for (uint256 i = 0; i < ids.length; i++) {
+            for (uint256 i; i < ids.length; i++) {
                 artGobblers.transferFrom(address(this), to, ids[i]);
             }
         }
diff --git a/src/utils/token/GobblersERC1155B.sol b/src/utils/token/GobblersERC1155B.sol
index ccf5cd0..1a5e2d3 100644
--- a/src/utils/token/GobblersERC1155B.sol
+++ b/src/utils/token/GobblersERC1155B.sol
@@ -111,7 +111,7 @@ abstract contract GobblersERC1155B {
         // Unchecked because the only math done is incrementing
         // the array index counter which cannot possibly overflow.
         unchecked {
-            for (uint256 i = 0; i < owners.length; ++i) {
+            for (uint256 i; i < owners.length; ++i) {
                 balances[i] = balanceOf(owners[i], ids[i]);
             }
         }
@@ -170,7 +170,7 @@ abstract contract GobblersERC1155B {
 
         // Counter overflow is unrealistic on human timescales.
         unchecked {
-            for (uint256 i = 0; i < amount; ++i) {
+            for (uint256 i; i < amount; ++i) {
                 ids[i] = ++lastMintedId; // Increment id while setting.
 
                 amounts[i] = 1; // ERC1155B amounts are always 1.
diff --git a/src/utils/token/GobblersERC721.sol b/src/utils/token/GobblersERC721.sol
index f2d0647..1531933 100644
--- a/src/utils/token/GobblersERC721.sol
+++ b/src/utils/token/GobblersERC721.sol
@@ -183,7 +183,7 @@ abstract contract GobblersERC721 {
         unchecked {
             getUserData[to].gobblersOwned += uint32(amount);
 
-            for (uint256 i = 0; i < amount; ++i) {
+            for (uint256 i; i < amount; ++i) {
                 getGobblerData[++lastMintedId].owner = to;
 
                 emit Transfer(address(0), to, lastMintedId);

```