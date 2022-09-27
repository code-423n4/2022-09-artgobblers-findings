# Gas

## Instead of a modifier use a inline if to check if `msg.sender` can access to the method.

```
> forge snapshot --diff
Overall gas change: -305862 (-0.708%)
```

Recommended changes

```diff
diff --git a/src/Goo.sol b/src/Goo.sol
index bcc459c..046d783 100644
--- a/src/Goo.sol
+++ b/src/Goo.sol
@@ -88,31 +88,27 @@ contract Goo is ERC20("Goo", "GOO", 18) {
                              MINT/BURN LOGIC
     //////////////////////////////////////////////////////////////*/
 
-    /// @notice Requires caller address to match user address.
-    modifier only(address user) {
-        if (msg.sender != user) revert Unauthorized();
-
-        _;
-    }
-
     /// @notice Mint any amount of goo to a user. Can only be called by ArtGobblers.
     /// @param to The address of the user to mint goo to.
     /// @param amount The amount of goo to mint.
-    function mintForGobblers(address to, uint256 amount) external only(artGobblers) {
+    function mintForGobblers(address to, uint256 amount) external {
+        if (msg.sender != artGobblers) revert Unauthorized();
         _mint(to, amount);
     }
 
     /// @notice Burn any amount of goo from a user. Can only be called by ArtGobblers.
     /// @param from The address of the user to burn goo from.
     /// @param amount The amount of goo to burn.
-    function burnForGobblers(address from, uint256 amount) external only(artGobblers) {
+    function burnForGobblers(address from, uint256 amount) external {
+        if (msg.sender != artGobblers) revert Unauthorized();
         _burn(from, amount);
     }
 
     /// @notice Burn any amount of goo from a user. Can only be called by Pages.
     /// @param from The address of the user to burn goo from.
     /// @param amount The amount of goo to burn.
-    function burnForPages(address from, uint256 amount) external only(pages) {
+    function burnForPages(address from, uint256 amount) external {
+        if (msg.sender != pages) revert Unauthorized();
         _burn(from, amount);
     }
 }
```

---
