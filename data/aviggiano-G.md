# 1. Replace `require(condition, "ERROR")` by `if(!condition) revert Error();` pattern for gas savings.

List of affected assets in scope:

```
$ grep -rn '^\s*[^//]*require\s*(' src/ArtGobblers.sol lib/solmate/src/utils/FixedPointMathLib.sol lib/solmate/src/tokens/ERC721.sol src/utils/token/GobblersERC1155B.sol lib/solmate/src/utils/SignedWadMath.sol src/utils/token/GobblersERC721.sol src/utils/token/PagesERC721.sol script/deploy/DeployBase.s.sol src/Pages.sol src/utils/rand/ChainlinkV1RandProvider.sol lib/VRGDAs/src/LogisticToLinearVRGDA.sol lib/solmate/src/utils/MerkleProofLib.sol script/deploy/DeployRinkeby.s.sol src/Goo.sol lib/VRGDAs/src/LogisticVRGDA.sol lib/solmate/src/utils/LibString.sol lib/VRGDAs/src/VRGDA.sol lib/goo-issuance/src/LibGOO.sol lib/solmate/src/auth/Owned.sol lib/VRGDAs/src/LinearVRGDA.sol src/utils/GobblerReserve.sol src/utils/rand/RandProvider.sol

src/ArtGobblers.sol:437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
src/ArtGobblers.sol:885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
src/ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
src/ArtGobblers.sol:889:        require(
lib/solmate/src/tokens/ERC721.sol:36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:40:        require(owner != address(0), "ZERO_ADDRESS");
lib/solmate/src/tokens/ERC721.sol:69:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
lib/solmate/src/tokens/ERC721.sol:87:        require(from == _ownerOf[id], "WRONG_FROM");
lib/solmate/src/tokens/ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:91:        require(
lib/solmate/src/tokens/ERC721.sol:118:        require(
lib/solmate/src/tokens/ERC721.sol:134:        require(
lib/solmate/src/tokens/ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:160:        require(_ownerOf[id] == address(0), "ALREADY_MINTED");
lib/solmate/src/tokens/ERC721.sol:175:        require(owner != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:196:        require(
lib/solmate/src/tokens/ERC721.sol:211:        require(
src/utils/token/GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
src/utils/token/GobblersERC1155B.sol:149:            require(
src/utils/token/GobblersERC1155B.sol:185:            require(
lib/solmate/src/utils/SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
src/utils/token/GobblersERC721.sol:121:        require(
src/utils/token/GobblersERC721.sol:137:        require(
src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
src/utils/token/PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
src/utils/token/PagesERC721.sol:107:        require(
src/utils/token/PagesERC721.sol:135:            require(
src/utils/token/PagesERC721.sol:151:            require(
lib/VRGDAs/src/VRGDA.sol:32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
lib/solmate/src/auth/Owned.sol:20:        require(msg.sender == owner, "UNAUTHORIZED");

```

E.g. for the first occurrence

**Before**
```
src/ArtGobblers.sol:439:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
```
```
forge test --match-test testMintLegendaryGobblerWithUnownedId --gas-report
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                            ┆ min             ┆ avg     ┆ median  ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ mintLegendaryGobbler                     ┆ 182111          ┆ 182111  ┆ 182111  ┆ 182111  ┆ 1       │
```

**After**
```
src/ArtGobblers.sol:439:                if(getGobblerData[id].owner != msg.sender) revert WrongFrom();
```
```
forge test --match-test testMintLegendaryGobblerWithUnownedId --gas-report
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                            ┆ min             ┆ avg     ┆ median  ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ mintLegendaryGobbler                     ┆ 182039          ┆ 182039  ┆ 182039  ┆ 182039  ┆ 1       │
```

The other findings are analogous.

# 2. Only clear NFT approval storage for approved NFTs

List of affected assets in scope:

```
$ git grep -n 'delete getApproved'

src/ArtGobblers.sol:894:        delete getApproved[id];
src/utils/token/PagesERC721.sol:122:        delete getApproved[id];
```

**After**
```
$ git diff src/

diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..0ca71d8 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -887,12 +887,12 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         require(to != address(0), "INVALID_RECIPIENT");
 
         require(
-            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
+            msg.sender == from ||
+                isApprovedForAll[from][msg.sender] ||
+                (msg.sender == getApproved[id] && ((getApproved[id] = address(0)) == address(0))),
             "NOT_AUTHORIZED"
         );
 
-        delete getApproved[id];
-
         getGobblerData[id].owner = to;
 
         unchecked {
diff --git a/src/utils/token/PagesERC721.sol b/src/utils/token/PagesERC721.sol
index 4bbca84..331a9fb 100644
--- a/src/utils/token/PagesERC721.sol
+++ b/src/utils/token/PagesERC721.sol
@@ -105,7 +105,9 @@ abstract contract PagesERC721 {
         require(to != address(0), "INVALID_RECIPIENT");
 
         require(
-            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
+            msg.sender == from ||
+                isApprovedForAll(from, msg.sender) ||
+                (msg.sender == getApproved[id] && ((getApproved[id] = address(0)) == address(0))),
             "NOT_AUTHORIZED"
         );
 
@@ -119,8 +121,6 @@ abstract contract PagesERC721 {
 
         _ownerOf[id] = to;
 
-        delete getApproved[id];
-
         emit Transfer(from, to, id);
     }

```

Gas changes:

```
# Run before changes
$ forge snapshot --ffi

# Execute code changes

# Run after changes
$ forge snapshot --ffi --diff .gas-snapshot

# Output
Test result: ok. 66 passed; 0 failed; finished in 2.76s
...
testCanWithdraw() (gas: -3669 (-0.005%))
testFeedingArt() (gas: -1827 (-0.007%))
testGobblerBalancesAfterTransfer() (gas: -2293 (-0.009%))
testEmissionMultipleUpdatesAfterTransfer() (gas: -2293 (-0.009%))
testTransferGobbler() (gas: -2293 (-0.046%))
Overall gas change: -12375 (-0.076%)
```