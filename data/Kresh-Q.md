### Use safeTransferFrom to be sure that the ERC721 was transfered to the user or ERC721Receiver contract
Inside the code:
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..b16aea5 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -60,7 +60,7 @@ pragma solidity >=0.8.0;
              %@&%%#/***********./////(((((((####%%&&@@@@@@@@@@@@@@&@@@@@@@@@@@@@@@@&&%%%%%%%%#((((((((%@&#(((((#%@%/*******************./*/
 
 import {Owned} from "solmate/auth/Owned.sol";
-import {ERC721} from "solmate/tokens/ERC721.sol";
+import {ERC721, ERC721TokenReceiver} from "solmate/tokens/ERC721.sol";
 import {LibString} from "solmate/utils/LibString.sol";
 import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
 import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
@@ -80,7 +80,7 @@ import {Pages} from "./Pages.sol";
 /// @author FrankieIsLost <frankie@paradigm.xyz>
 /// @author transmissions11 <t11s@paradigm.xyz>
 /// @notice An experimental decentralized art factory by Justin Roiland and Paradigm.
-contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiver {
+contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC721TokenReceiver, ERC1155TokenReceiver {
     using LibString for uint256;
     using FixedPointMathLib for uint256;
 
@@ -745,7 +745,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
 
         isERC1155
             ? ERC1155(nft).safeTransferFrom(msg.sender, address(this), id, 1, "")
-            : ERC721(nft).transferFrom(msg.sender, address(this), id);
+            : ERC721(nft).safeTransferFrom(msg.sender, address(this), id);
     }
 
     /*//////////////////////////////////////////////////////////////
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..7699a69 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -35,7 +35,7 @@ contract GobblerReserve is Owned {
         // This is quite inefficient, but it's okay because this is not a hot path.
         unchecked {
             for (uint256 i = 0; i < ids.length; i++) {
-                artGobblers.transferFrom(address(this), to, ids[i]);
+                artGobblers.safeTransferFrom(address(this), to, ids[i]);
             }
         }
     }
```

### Use specific solidity versions
Inside the code (e.g.):
```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..446f6ff 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -1,5 +1,5 @@
 // SPDX-License-Identifier: MIT
-pragma solidity >=0.8.0;
+pragma solidity =0.8.13;
 
 /*                                                                              **,/*,
                                                                      *%@&%#/*,,..........,/(%&@@#*
```