##### Gas Optimizations

Gas savings are estimated using the gas report of existing `FOUNDRY_PROFILE="intense" forge test --gas-report` tests (the sum of all deployment costs and the sum of the costs of calling methods) and may vary depending on the implementation of the fix.
**Note**: method call evaluations are volatile: `≈ ±35 000`

|       | Issue                                                                                        | Instances | Estimated gas(deployments) | Estimated gas(avg method call) | Estimated gas(min method call) | Estimated gas(max method call) |
| :---: | :------------------------------------------------------------------------------------------- | :-------: | :------------------------: | :----------------------------: | :----------------------------: | :----------------------------: |
| **1** | Use custom errors rather than revert()/require() strings to save gas                         |    36     |          464 688           |        237 097 ± 35 000        |        231 885 ± 35 000        |        236 755 ± 35 000        |
| **2** | Use functions instead of modifiers                                                           |     1     |           89 330           |        67 087 ± 35 000         |        62 165 ± 35 000         |        67 078 ± 35 000         |
| **3** | Using bools for storage incurs overhead                                                      |     5     |           30 047           |        15 042 ± 35 000         |        10 185 ± 35 000         |        15 160 ± 35 000         |
| **4** | State variables should be cached in stack variables rather than re-reading them from storage |     2     |           12 819           |        28 790 ± 35 000         |        19 914 ± 35 000         |        13 107 ± 35 000         |
| **5** | ++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)            |     1     |            400             |         8 568 ± 35 000         |         -317 ± 35 000          |         3 199 ± 35 000         |
|       | **Overall gas savings**                                                                      |  **45**   |        **558 440**         |      **296 900 ± 35 000**      |      **297 516 ± 35 000**      |      **300 453 ± 35 000**      |

**Total: 45 instances over 5 issues**

---

#### 1. **Use custom errors rather than revert()/require() strings to save gas (36 instances)**

- Deployment. Gas Saved: **464 688**

- Minumal Method Call. Gas Saved: **237 097 ± 35 000**

- Average Method Call. Gas Saved: **231 885 ± 35 000**

- Maximum Method Call. Gas Saved: **236 755 ± 35 000**

- `forge snapshot --diff`. **Overall gas change: -11 266 (-1.828%)**

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

##### - 2022-09-artgobblers/src/ArtGobblers.sol:[437](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L437), [885](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L885), [887](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L887), [889-892](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L889-L892)

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..9c0cdbd 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -84,6 +84,10 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
   84,  84:     using LibString for uint256;
   85,  85:     using FixedPointMathLib for uint256;
   86,  86:
+       87:+
+       88:+    error InvalidRecipient();
+       89:+    error WrongFrom();
+       90:+
   87,  91:     /*//////////////////////////////////////////////////////////////
   88,  92:                                 ADDRESSES
   89,  93:     //////////////////////////////////////////////////////////////*/
@@ -434,7 +438,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  434, 438:
  435, 439:                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
  436, 440:
- 437     :-                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
+      441:+                if(getGobblerData[id].owner != msg.sender) revert WrongFrom();
  438, 442:
  439, 443:                 burnedMultipleTotal += getGobblerData[id].emissionMultiple;
  440, 444:
@@ -882,14 +886,11 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  882, 886:         address to,
  883, 887:         uint256 id
  884, 888:     ) public override {
- 885     :-        require(from == getGobblerData[id].owner, "WRONG_FROM");
+      889:+        if(from != getGobblerData[id].owner) revert WrongFrom();
  886, 890:
- 887     :-        require(to != address(0), "INVALID_RECIPIENT");
+      891:+        if(to == address(0)) revert InvalidRecipient();
  888, 892:
- 889     :-        require(
- 890     :-            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
- 891     :-            "NOT_AUTHORIZED"
- 892     :-        );
+      893:+        if(msg.sender != from && !isApprovedForAll[from][msg.sender] && msg.sender != getApproved[id]) revert NotAuthorized();
  893, 894:
  894, 895:         delete getApproved[id];
  895, 896:
diff --git a/test/ArtGobblers.t.sol b/test/ArtGobblers.t.sol
index 174d7b9..143d656 100644
--- a/test/ArtGobblers.t.sol
+++ b/test/ArtGobblers.t.sol
@@ -565,7 +565,7 @@ contract ArtGobblersTest is DSTestPlus {
  565, 565:         ids.push(999);
  566, 566:
  567, 567:         vm.prank(users[0]);
- 568     :-        vm.expectRevert("WRONG_FROM");
+      568:+        vm.expectRevert(ArtGobblers.WrongFrom.selector);
  569, 569:         gobblers.mintLegendaryGobbler(ids);
  570, 570:     }
  571, 571:
```

##### - 2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol:[36](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L36), [40](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L40), [69](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L69), [87](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L87), [89](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L89), [91-94](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L91-L94), [118-123](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L118-L123), [134-139](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L134-L139), [158](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L158), [160](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L160), [175](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L175), [196-201](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L196-L201), [211-216](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L211-L216)

```diff
Submodule lib/solmate contains modified content
diff --git a/lib/solmate/src/tokens/ERC721.sol b/lib/solmate/src/tokens/ERC721.sol
index b47f271..3a7a1bd 100644
--- a/lib/solmate/src/tokens/ERC721.sol
+++ b/lib/solmate/src/tokens/ERC721.sol
@@ -4,6 +4,15 @@ pragma solidity >=0.8.0;
    4,   4: /// @notice Modern, minimalist, and gas efficient ERC-721 implementation.
    5,   5: /// @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC721.sol)
    6,   6: abstract contract ERC721 {
+        7:+    error UnsafeRecipient();
+        8:+    error NotMinted();
+        9:+    error AlreadyMinted();
+       10:+    error InvalidRecipient();
+       11:+    error NotAuthorized();
+       12:+    error WrongFrom();
+       13:+    error ZeroAddress();
+       14:+
+       15:+
    7,  16:     /*//////////////////////////////////////////////////////////////
    8,  17:                                  EVENTS
    9,  18:     //////////////////////////////////////////////////////////////*/
@@ -33,11 +42,11 @@ abstract contract ERC721 {
   33,  42:     mapping(address => uint256) internal _balanceOf;
   34,  43:
   35,  44:     function ownerOf(uint256 id) public view virtual returns (address owner) {
-  36     :-        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
+       45:+        if((owner = _ownerOf[id]) == address(0)) revert NotMinted();
   37,  46:     }
   38,  47:
   39,  48:     function balanceOf(address owner) public view virtual returns (uint256) {
-  40     :-        require(owner != address(0), "ZERO_ADDRESS");
+       49:+        if(owner == address(0)) revert ZeroAddress();
   41,  50:
   42,  51:         return _balanceOf[owner];
   43,  52:     }
@@ -66,7 +75,7 @@ abstract contract ERC721 {
   66,  75:     function approve(address spender, uint256 id) public virtual {
   67,  76:         address owner = _ownerOf[id];
   68,  77:
-  69     :-        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
+       78:+        if(msg.sender != owner && !isApprovedForAll[owner][msg.sender]) revert NotAuthorized();
   70,  79:
   71,  80:         getApproved[id] = spender;
   72,  81:
@@ -84,14 +93,11 @@ abstract contract ERC721 {
   84,  93:         address to,
   85,  94:         uint256 id
   86,  95:     ) public virtual {
-  87     :-        require(from == _ownerOf[id], "WRONG_FROM");
+       96:+        if(from != _ownerOf[id]) revert WrongFrom();
   88,  97:
-  89     :-        require(to != address(0), "INVALID_RECIPIENT");
+       98:+        if(to == address(0)) revert InvalidRecipient();
   90,  99:
-  91     :-        require(
-  92     :-            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
-  93     :-            "NOT_AUTHORIZED"
-  94     :-        );
+      100:+        if(msg.sender != from && !isApprovedForAll[from][msg.sender] && msg.sender != getApproved[id]) revert NotAuthorized();
   95, 101:
   96, 102:         // Underflow of the sender's balance is impossible because we check for
   97, 103:         // ownership above and the recipient's balance can't realistically overflow.
@@ -115,12 +121,8 @@ abstract contract ERC721 {
  115, 121:     ) public virtual {
  116, 122:         transferFrom(from, to, id);
  117, 123:
- 118     :-        require(
- 119     :-            to.code.length == 0 ||
- 120     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
- 121     :-                ERC721TokenReceiver.onERC721Received.selector,
- 122     :-            "UNSAFE_RECIPIENT"
- 123     :-        );
+      124:+        if(to.code.length != 0 && ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") !=
+      125:+                ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  124, 126:     }
  125, 127:
  126, 128:     function safeTransferFrom(
@@ -131,12 +133,9 @@ abstract contract ERC721 {
  131, 133:     ) public virtual {
  132, 134:         transferFrom(from, to, id);
  133, 135:
- 134     :-        require(
- 135     :-            to.code.length == 0 ||
- 136     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
- 137     :-                ERC721TokenReceiver.onERC721Received.selector,
- 138     :-            "UNSAFE_RECIPIENT"
- 139     :-        );
+      136:+        if(to.code.length != 0 &&
+      137:+                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) !=
+      138:+                ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  140, 139:     }
  141, 140:
  142, 141:     /*//////////////////////////////////////////////////////////////
@@ -155,9 +154,9 @@ abstract contract ERC721 {
  155, 154:     //////////////////////////////////////////////////////////////*/
  156, 155:
  157, 156:     function _mint(address to, uint256 id) internal virtual {
- 158     :-        require(to != address(0), "INVALID_RECIPIENT");
+      157:+        if(to == address(0)) revert InvalidRecipient();
  159, 158:
- 160     :-        require(_ownerOf[id] == address(0), "ALREADY_MINTED");
+      159:+        if(_ownerOf[id] != address(0)) revert AlreadyMinted();
  161, 160:
  162, 161:         // Counter overflow is incredibly unrealistic.
  163, 162:         unchecked {
@@ -172,7 +171,7 @@ abstract contract ERC721 {
  172, 171:     function _burn(uint256 id) internal virtual {
  173, 172:         address owner = _ownerOf[id];
  174, 173:
- 175     :-        require(owner != address(0), "NOT_MINTED");
+      174:+        if(owner == address(0)) revert NotMinted();
  176, 175:
  177, 176:         // Ownership check above ensures no underflow.
  178, 177:         unchecked {
@@ -193,12 +192,10 @@ abstract contract ERC721 {
  193, 192:     function _safeMint(address to, uint256 id) internal virtual {
  194, 193:         _mint(to, id);
  195, 194:
- 196     :-        require(
- 197     :-            to.code.length == 0 ||
- 198     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, "") ==
- 199     :-                ERC721TokenReceiver.onERC721Received.selector,
- 200     :-            "UNSAFE_RECIPIENT"
- 201     :-        );
+      195:+        if(
+      196:+            to.code.length != 0 &&
+      197:+                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, "") !=
+      198:+                ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  202, 199:     }
  203, 200:
  204, 201:     function _safeMint(
@@ -208,12 +205,10 @@ abstract contract ERC721 {
  208, 205:     ) internal virtual {
  209, 206:         _mint(to, id);
  210, 207:
- 211     :-        require(
- 212     :-            to.code.length == 0 ||
- 213     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, data) ==
- 214     :-                ERC721TokenReceiver.onERC721Received.selector,
- 215     :-            "UNSAFE_RECIPIENT"
- 216     :-        );
+      208:+        if(
+      209:+            to.code.length != 0 &&
+      210:+                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, data) !=
+      211:+                ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  217, 212:     }
  218, 213: }
  219, 214:
```

##### - 2022-09-artgobblers/src/utils/token/GobblersERC721.sol:[62](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L62), [66](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L66), [95](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L95), [121-126](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L121-L126), [137-142](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L137-L142)

```diff
diff --git a/src/utils/token/GobblersERC721.sol b/src/utils/token/GobblersERC721.sol
index f2d0647..866fc4b 100644
--- a/src/utils/token/GobblersERC721.sol
+++ b/src/utils/token/GobblersERC721.sol
@@ -6,6 +6,11 @@ import {ERC721TokenReceiver} from "solmate/tokens/ERC721.sol";
    6,   6: /// @notice ERC721 implementation optimized for ArtGobblers by packing balanceOf/ownerOf with user/attribute data.
    7,   7: /// @author Modified from Solmate (https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC721.sol)
    8,   8: abstract contract GobblersERC721 {
+        9:+    error UnsafeRecipient();
+       10:+    error NotAuthorized();
+       11:+    error ZeroAddress();
+       12:+    error NotMinted();
+       13:+
    9,  14:     /*//////////////////////////////////////////////////////////////
   10,  15:                                  EVENTS
   11,  16:     //////////////////////////////////////////////////////////////*/
@@ -59,11 +64,11 @@ abstract contract GobblersERC721 {
   59,  64:     mapping(address => UserData) public getUserData;
   60,  65:
   61,  66:     function ownerOf(uint256 id) external view returns (address owner) {
-  62     :-        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
+       67:+        if((owner = getGobblerData[id].owner) == address(0)) revert NotMinted();
   63,  68:     }
   64,  69:
   65,  70:     function balanceOf(address owner) external view returns (uint256) {
-  66     :-        require(owner != address(0), "ZERO_ADDRESS");
+       71:+        if(owner == address(0)) revert ZeroAddress();
   67,  72:
   68,  73:         return getUserData[owner].gobblersOwned;
   69,  74:     }
@@ -92,7 +97,7 @@ abstract contract GobblersERC721 {
   92,  97:     function approve(address spender, uint256 id) external {
   93,  98:         address owner = getGobblerData[id].owner;
   94,  99:
-  95     :-        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
+      100:+        if(msg.sender != owner && !isApprovedForAll[owner][msg.sender]) revert NotAuthorized();
   96, 101:
   97, 102:         getApproved[id] = spender;
   98, 103:
@@ -118,12 +123,10 @@ abstract contract GobblersERC721 {
  118, 123:     ) external {
  119, 124:         transferFrom(from, to, id);
  120, 125:
- 121     :-        require(
- 122     :-            to.code.length == 0 ||
- 123     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
- 124     :-                ERC721TokenReceiver.onERC721Received.selector,
- 125     :-            "UNSAFE_RECIPIENT"
- 126     :-        );
+      126:+        if(
+      127:+            to.code.length != 0 &&
+      128:+                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") !=
+      129:+                ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  127, 130:     }
  128, 131:
  129, 132:     function safeTransferFrom(
@@ -134,12 +137,10 @@ abstract contract GobblersERC721 {
  134, 137:     ) external {
  135, 138:         transferFrom(from, to, id);
  136, 139:
- 137     :-        require(
- 138     :-            to.code.length == 0 ||
- 139     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
- 140     :-                ERC721TokenReceiver.onERC721Received.selector,
- 141     :-            "UNSAFE_RECIPIENT"
- 142     :-        );
+      140:+        if(
+      141:+            to.code.length != 0 &&
+      142:+                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) !=
+      143:+                ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  143, 144:     }
  144, 145:
  145, 146:     /*//////////////////////////////////////////////////////////////
diff --git a/test/ArtGobblers.t.sol b/test/ArtGobblers.t.sol
index 174d7b9..87e14bc 100644
--- a/test/ArtGobblers.t.sol
+++ b/test/ArtGobblers.t.sol
@@ -1,6 +1,7 @@
    1,   1: // SPDX-License-Identifier: MIT
    2,   2: pragma solidity >=0.8.0;
    3,   3:
+        4:+import {GobblersERC721} from "../src/utils/token/GobblersERC721.sol";
    4,   5: import {DSTestPlus} from "solmate/test/utils/DSTestPlus.sol";
    5,   6: import {Utilities} from "./utils/Utilities.sol";
    6,   7: import {console} from "./utils/Console.sol";
@@ -445,7 +446,7 @@ contract ArtGobblersTest is DSTestPlus {
  445, 446:         assertEq(gobblers.getGobblerEmissionMultiple(mintedLegendaryId), emissionMultipleSum * 2);
  446, 447:
  447, 448:         for (uint256 i = 0; i < ids.length; i++) {
- 448     :-            hevm.expectRevert("NOT_MINTED");
+      449:+            hevm.expectRevert(GobblersERC721.NotMinted.selector);
  449, 450:             gobblers.ownerOf(ids[i]);
  450, 451:         }
  451, 452:     }
@@ -534,7 +535,7 @@ contract ArtGobblersTest is DSTestPlus {
  534, 535:
  535, 536:         //check full cost was burned
  536, 537:         for (uint256 curId = 1; curId <= cost; curId++) {
- 537     :-            hevm.expectRevert("NOT_MINTED");
+      538:+            hevm.expectRevert(GobblersERC721.NotMinted.selector);
  538, 539:             gobblers.ownerOf(curId);
  539, 540:         }
  540, 541:         //check extra tokens were not burned
@@ -565,7 +566,7 @@ contract ArtGobblersTest is DSTestPlus {
  565, 566:         ids.push(999);
  566, 567:
  567, 568:         vm.prank(users[0]);
- 568     :-        vm.expectRevert("WRONG_FROM");
+      569:+        vm.expectRevert(ArtGobblers.WrongFrom.selector);
  569, 570:         gobblers.mintLegendaryGobbler(ids);
  570, 571:     }
  571, 572:
```

##### - 2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:[107](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L107), [149-153](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L149-L153), [185-189](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L185-L189)

```diff
diff --git a/src/utils/token/GobblersERC1155B.sol b/src/utils/token/GobblersERC1155B.sol
index ccf5cd0..35838a7 100644
--- a/src/utils/token/GobblersERC1155B.sol
+++ b/src/utils/token/GobblersERC1155B.sol
@@ -6,6 +6,9 @@ import {ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
    6,   6: /// @notice ERC1155B implementation optimized for ArtGobblers by using the ownerOf storage slot to store attribute data.
    7,   7: /// @author Modified from Solmate (https://github.com/Rari-Capital/solmate/blob/v7/src/tokens/ERC1155B.sol)
    8,   8: abstract contract GobblersERC1155B {
+        9:+    error UnsafeRecipient();
+       10:+    error LengthMismatch();
+       11:+
    9,  12:     /*//////////////////////////////////////////////////////////////
   10,  13:                                  EVENTS
   11,  14:     //////////////////////////////////////////////////////////////*/
@@ -104,7 +107,7 @@ abstract contract GobblersERC1155B {
  104, 107:         virtual
  105, 108:         returns (uint256[] memory balances)
  106, 109:     {
- 107     :-        require(owners.length == ids.length, "LENGTH_MISMATCH");
+      110:+        if(owners.length != ids.length) revert LengthMismatch();
  108, 111:
  109, 112:         balances = new uint256[](owners.length);
  110, 113:
@@ -146,11 +149,9 @@ abstract contract GobblersERC1155B {
  146, 149:         emit TransferSingle(msg.sender, address(0), to, id, 1);
  147, 150:
  148, 151:         if (to.code.length != 0) {
- 149     :-            require(
- 150     :-                ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
- 151     :-                    ERC1155TokenReceiver.onERC1155Received.selector,
- 152     :-                "UNSAFE_RECIPIENT"
- 153     :-            );
+      152:+            if(
+      153:+                ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) !=
+      154:+                    ERC1155TokenReceiver.onERC1155Received.selector) revert UnsafeRecipient();
  154, 155:         }
  155, 156:     }
  156, 157:
@@ -182,11 +183,9 @@ abstract contract GobblersERC1155B {
  182, 183:         emit TransferBatch(msg.sender, address(0), to, ids, amounts);
  183, 184:
  184, 185:         if (to.code.length != 0) {
- 185     :-            require(
- 186     :-                ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
- 187     :-                    ERC1155TokenReceiver.onERC1155BatchReceived.selector,
- 188     :-                "UNSAFE_RECIPIENT"
- 189     :-            );
+      186:+            if(
+      187:+                ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) !=
+      188:+                    ERC1155TokenReceiver.onERC1155BatchReceived.selector) revert UnsafeRecipient();
  190, 189:         }
  191, 190:
  192, 191:         return lastMintedId; // Return the new last minted id.
```

##### - 2022-09-artgobblers/lib/solmate/src/auth/Owned.sol:[20](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L20)

```diff
Submodule lib/solmate contains modified content
diff --git a/lib/solmate/src/auth/Owned.sol b/lib/solmate/src/auth/Owned.sol
index 1e52695..e6995a5 100644
--- a/lib/solmate/src/auth/Owned.sol
+++ b/lib/solmate/src/auth/Owned.sol
@@ -4,6 +4,8 @@ pragma solidity >=0.8.0;
    4,   4: /// @notice Simple single owner authorization mixin.
    5,   5: /// @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/Owned.sol)
    6,   6: abstract contract Owned {
+        7:+    error Unauthorized();
+        8:+
    7,   9:     /*//////////////////////////////////////////////////////////////
    8,  10:                                  EVENTS
    9,  11:     //////////////////////////////////////////////////////////////*/
@@ -17,7 +19,7 @@ abstract contract Owned {
   17,  19:     address public owner;
   18,  20:
   19,  21:     modifier onlyOwner() virtual {
-  20     :-        require(msg.sender == owner, "UNAUTHORIZED");
+       22:+        if(msg.sender != owner) revert Unauthorized();
   21,  23:
   22,  24:         _;
   23,  25:     }
diff --git a/test/RandProvider.t.sol b/test/RandProvider.t.sol
index cd21b1c..83de626 100644
--- a/test/RandProvider.t.sol
+++ b/test/RandProvider.t.sol
@@ -1,6 +1,7 @@
    1,   1: // SPDX-License-Identifier: MIT
    2,   2: pragma solidity >=0.8.0;
    3,   3:
+        4:+import {Owned} from "solmate/auth/Owned.sol";
    4,   5: import {DSTestPlus} from "solmate/test/utils/DSTestPlus.sol";
    5,   6: import {Utilities} from "./utils/Utilities.sol";
    6,   7: import {console} from "./utils/Console.sol";
@@ -122,7 +123,7 @@ contract RandProviderTest is DSTestPlus {
  122, 123:
  123, 124:     function testRandomnessIsOnlyUpgradableByOwner() public {
  124, 125:         RandProvider newProvider = new ChainlinkV1RandProvider(ArtGobblers(address(0)), address(0), address(0), 0, 0);
- 125     :-        vm.expectRevert("UNAUTHORIZED");
+      126:+        vm.expectRevert(Owned.Unauthorized.selector);
  126, 127:         vm.prank(address(0xBEEFBABE));
  127, 128:         gobblers.upgradeRandProvider(newProvider);
  128, 129:     }
```

##### - 2022-09-artgobblers/src/utils/token/PagesERC721.sol:[55](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L55), [59](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L59), [85](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L85), [103](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L103), [105](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L105), [107-110](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L107-L110), [135-139](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L135-L139), [151-155](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L151-L155)

```diff
diff --git a/src/utils/token/PagesERC721.sol b/src/utils/token/PagesERC721.sol
index 4bbca84..5829185 100644
--- a/src/utils/token/PagesERC721.sol
+++ b/src/utils/token/PagesERC721.sol
@@ -7,6 +7,13 @@ import {ArtGobblers} from "../../ArtGobblers.sol";
    7,   7: /// @notice ERC721 implementation optimized for Pages by pre-approving them to the ArtGobblers contract.
    8,   8: /// @author Modified from Solmate (https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC721.sol)
    9,   9: abstract contract PagesERC721 {
+       10:+    error UnsafeRecipient();
+       11:+    error NotAuthorized();
+       12:+    error InvalidRecipient();
+       13:+    error WrongFrom();
+       14:+    error ZeroAddress();
+       15:+    error NotMinted();
+       16:+
   10,  17:     /*//////////////////////////////////////////////////////////////
   11,  18:                                  EVENTS
   12,  19:     //////////////////////////////////////////////////////////////*/
@@ -52,11 +59,11 @@ abstract contract PagesERC721 {
   52,  59:     mapping(address => uint256) internal _balanceOf;
   53,  60:
   54,  61:     function ownerOf(uint256 id) external view returns (address owner) {
-  55     :-        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
+       62:+        if((owner = _ownerOf[id]) == address(0)) revert NotMinted();
   56,  63:     }
   57,  64:
   58,  65:     function balanceOf(address owner) external view returns (uint256) {
-  59     :-        require(owner != address(0), "ZERO_ADDRESS");
+       66:+        if(owner == address(0)) revert ZeroAddress();
   60,  67:
   61,  68:         return _balanceOf[owner];
   62,  69:     }
@@ -82,7 +89,7 @@ abstract contract PagesERC721 {
   82,  89:     function approve(address spender, uint256 id) external {
   83,  90:         address owner = _ownerOf[id];
   84,  91:
-  85     :-        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
+       92:+        if(msg.sender == owner && !isApprovedForAll(owner, msg.sender)) revert NotAuthorized();
   86,  93:
   87,  94:         getApproved[id] = spender;
   88,  95:
@@ -100,14 +107,11 @@ abstract contract PagesERC721 {
  100, 107:         address to,
  101, 108:         uint256 id
  102, 109:     ) public {
- 103     :-        require(from == _ownerOf[id], "WRONG_FROM");
+      110:+        if(from != _ownerOf[id]) revert WrongFrom();
  104, 111:
- 105     :-        require(to != address(0), "INVALID_RECIPIENT");
+      112:+        if(to == address(0)) revert InvalidRecipient();
  106, 113:
- 107     :-        require(
- 108     :-            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
- 109     :-            "NOT_AUTHORIZED"
- 110     :-        );
+      114:+        if(msg.sender != from && !isApprovedForAll(from, msg.sender) && msg.sender != getApproved[id]) revert NotAuthorized();
  111, 115:
  112, 116:         // Underflow of the sender's balance is impossible because we check for
  113, 117:         // ownership above and the recipient's balance can't realistically overflow.
@@ -132,11 +136,8 @@ abstract contract PagesERC721 {
  132, 136:         transferFrom(from, to, id);
  133, 137:
  134, 138:         if (to.code.length != 0)
- 135     :-            require(
- 136     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
- 137     :-                    ERC721TokenReceiver.onERC721Received.selector,
- 138     :-                "UNSAFE_RECIPIENT"
- 139     :-            );
+      139:+            if(ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") !=
+      140:+                    ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  140, 141:     }
  141, 142:
  142, 143:     function safeTransferFrom(
@@ -148,11 +149,9 @@ abstract contract PagesERC721 {
  148, 149:         transferFrom(from, to, id);
  149, 150:
  150, 151:         if (to.code.length != 0)
- 151     :-            require(
- 152     :-                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
- 153     :-                    ERC721TokenReceiver.onERC721Received.selector,
- 154     :-                "UNSAFE_RECIPIENT"
- 155     :-            );
+      152:+            if(
+      153:+                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) !=
+      154:+                    ERC721TokenReceiver.onERC721Received.selector) revert UnsafeRecipient();
  156, 155:     }
  157, 156:
  158, 157:     /*//////////////////////////////////////////////////////////////
diff --git a/test/ArtGobblers.t.sol b/test/ArtGobblers.t.sol
index 87e14bc..a01ae8b 100644
--- a/test/ArtGobblers.t.sol
+++ b/test/ArtGobblers.t.sol
@@ -1,6 +1,7 @@
    1,   1: // SPDX-License-Identifier: MIT
    2,   2: pragma solidity >=0.8.0;
    3,   3:
+        4:+import {PagesERC721} from "../src/utils/token/PagesERC721.sol";
    4,   5: import {GobblersERC721} from "../src/utils/token/GobblersERC721.sol";
    5,   6: import {DSTestPlus} from "solmate/test/utils/DSTestPlus.sol";
    6,   7: import {Utilities} from "./utils/Utilities.sol";
@@ -965,7 +966,7 @@ contract ArtGobblersTest is DSTestPlus {
  965, 966:         address user = users[0];
  966, 967:         mintGobblerToAddress(user, 1);
  967, 968:         vm.startPrank(user);
- 968     :-        vm.expectRevert("WRONG_FROM");
+      969:+        vm.expectRevert(PagesERC721.WrongFrom.selector);
  969, 970:         gobblers.gobble(1, address(pages), 1, false);
  970, 971:         vm.stopPrank();
  971, 972:     }
```

##### - 2022-09-artgobblers/lib/solmate/src/utils/SignedWadMath.sol:[142](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L142)

```diff
Submodule lib/solmate contains modified content
diff --git a/lib/solmate/src/utils/SignedWadMath.sol b/lib/solmate/src/utils/SignedWadMath.sol
index 6078106..2127965 100644
--- a/lib/solmate/src/utils/SignedWadMath.sol
+++ b/lib/solmate/src/utils/SignedWadMath.sol
@@ -1,6 +1,8 @@
    1,   1: // SPDX-License-Identifier: MIT
    2,   2: pragma solidity >=0.8.0;
    3,   3:
+        4:+error Undefined();
+        5:+
    4,   6: /// @notice Signed 18 decimal fixed point (wad) arithmetic library.
    5,   7: /// @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/utils/SignedWadMath.sol)
    6,   8:
@@ -139,7 +141,7 @@ function wadExp(int256 x) pure returns (int256 r) {
  139, 141:
  140, 142: function wadLn(int256 x) pure returns (int256 r) {
  141, 143:     unchecked {
- 142     :-        require(x > 0, "UNDEFINED");
+      144:+        if(x <= 0) revert Undefined();
  143, 145:
  144, 146:         // We want to convert x from 10**18 fixed point to 2**96 fixed point.
  145, 147:         // We do this by multiplying by 2**96 / 10**18. But since
diff --git a/test/ArtGobblers.t.sol b/test/ArtGobblers.t.sol
index a01ae8b..60ef4aa 100644
--- a/test/ArtGobblers.t.sol
+++ b/test/ArtGobblers.t.sol
@@ -19,7 +19,7 @@ import {VRFCoordinatorMock} from "chainlink/v0.8/mocks/VRFCoordinatorMock.sol";
   19,  19: import {ERC721} from "solmate/tokens/ERC721.sol";
   20,  20: import {MockERC1155} from "solmate/test/utils/mocks/MockERC1155.sol";
   21,  21: import {LibString} from "solmate/utils/LibString.sol";
-  22     :-import {fromDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
+       22:+import {fromDaysWadUnsafe, Undefined} from "solmate/utils/SignedWadMath.sol";
   23,  23:
   24,  24: /// @notice Unit test for Art Gobbler Contract.
   25,  25: contract ArtGobblersTest is DSTestPlus {
@@ -349,7 +349,7 @@ contract ArtGobblersTest is DSTestPlus {
  349, 349:         // One plus the max number of mintable gobblers.
  350, 350:         int256 maxMintablePlusOne = int256(gobblers.MAX_MINTABLE() + 1) * 1e18;
  351, 351:         // This call should revert, since there should be no target date beyond max mintable gobblers.
- 352     :-        vm.expectRevert("UNDEFINED");
+      352:+        vm.expectRevert(Undefined.selector);
  353, 353:         gobblers.getTargetSaleTime(maxMintablePlusOne);
  354, 354:     }
  355, 355:
@@ -1057,14 +1057,14 @@ contract ArtGobblersTest is DSTestPlus {
 1057,1057:         for (uint256 i = 0; i < maxMintableWithGoo + 1; i++) {
 1058,1058:             vm.warp(block.timestamp + 1 days);
 1059,1059:
-1060     :-            if (i == maxMintableWithGoo) vm.expectRevert("UNDEFINED");
+     1060:+            if (i == maxMintableWithGoo) vm.expectRevert(Undefined.selector);
 1061,1061:             uint256 cost = gobblers.gobblerPrice();
 1062,1062:
 1063,1063:             vm.prank(address(gobblers));
 1064,1064:             goo.mintForGobblers(users[0], cost);
 1065,1065:             vm.prank(users[0]);
 1066,1066:
-1067     :-            if (i == maxMintableWithGoo) vm.expectRevert("UNDEFINED");
+     1067:+            if (i == maxMintableWithGoo) vm.expectRevert(Undefined.selector);
 1068,1068:             gobblers.mintFromGoo(type(uint256).max, false);
 1069,1069:         }
 1070,1070:     }
```

##### - 2022-09-artgobblers/lib/VRGDAs/src/VRGDA.sol:[32](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L32)

```diff
Submodule lib/VRGDAs contains modified content
diff --git a/lib/VRGDAs/src/VRGDA.sol b/lib/VRGDAs/src/VRGDA.sol
index c613ff4..e9bbc59 100644
--- a/lib/VRGDAs/src/VRGDA.sol
+++ b/lib/VRGDAs/src/VRGDA.sol
@@ -8,6 +8,8 @@ import {wadExp, wadLn, wadMul, unsafeWadMul, toWadUnsafe} from "solmate/utils/Si
    8,   8: /// @author FrankieIsLost <frankie@paradigm.xyz>
    9,   9: /// @notice Sell tokens roughly according to an issuance schedule.
   10,  10: abstract contract VRGDA {
+       11:+    error NonNegativeDecayConstant();
+       12:+
   11,  13:     /*//////////////////////////////////////////////////////////////
   12,  14:                             VRGDA PARAMETERS
   13,  15:     //////////////////////////////////////////////////////////////*/
@@ -29,7 +31,7 @@ abstract contract VRGDA {
   29,  31:         decayConstant = wadLn(1e18 - _priceDecayPercent);
   30,  32:
   31,  33:         // The decay constant must be negative for VRGDAs to work.
-  32     :-        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
+       34:+        if(decayConstant >= 0) revert NonNegativeDecayConstant();
   33,  35:     }
   34,  36:
   35,  37:     /*//////////////////////////////////////////////////////////////
```

---

#### 2. **Use functions instead of modifiers (1 instance)**

- Deployment. Gas Saved: **89 330**

- Minumal Method Call. Gas Saved: **67 087 ± 35 000**

- Average Method Call. Gas Saved: **62 165 ± 35 000**

- Maximum Method Call. Gas Saved: **67 078 ± 35 000**

- `forge snapshot --diff`. **Overall gas change: 102 (0.019%)**

##### - 2022-09-artgobblers/lib/solmate/src/auth/Owned.sol:[19](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L19)

```diff
Submodule lib/solmate contains modified content
diff --git a/lib/solmate/src/auth/Owned.sol b/lib/solmate/src/auth/Owned.sol
index 1e52695..c66560b 100644
--- a/lib/solmate/src/auth/Owned.sol
+++ b/lib/solmate/src/auth/Owned.sol
@@ -16,10 +16,8 @@ abstract contract Owned {
   16,  16:
   17,  17:     address public owner;
   18,  18:
-  19     :-    modifier onlyOwner() virtual {
+       19:+    function onlyOwner() internal virtual {
   20,  20:         require(msg.sender == owner, "UNAUTHORIZED");
-  21     :-
-  22     :-        _;
   23,  21:     }
   24,  22:
   25,  23:     /*//////////////////////////////////////////////////////////////
@@ -36,7 +34,8 @@ abstract contract Owned {
   36,  34:                              OWNERSHIP LOGIC
   37,  35:     //////////////////////////////////////////////////////////////*/
   38,  36:
-  39     :-    function setOwner(address newOwner) public virtual onlyOwner {
+       37:+    function setOwner(address newOwner) public virtual {
+       38:+        onlyOwner();
   40,  39:         owner = newOwner;
   41,  40:
   42,  41:         emit OwnerUpdated(msg.sender, newOwner);
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..23fc1e7 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -557,7 +557,8 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  557, 557:
  558, 558:     /// @notice Upgrade the rand provider contract. Useful if current VRF is sunset.
  559, 559:     /// @param newRandProvider The new randomness provider contract address.
- 560     :-    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
+      560:+    function upgradeRandProvider(RandProvider newRandProvider) external {
+      561:+        onlyOwner();
  561, 562:         // Revert if waiting for seed, so we don't interrupt requests in flight.
  562, 563:         if (gobblerRevealsData.waitingForSeed) revert SeedPending();
  563, 564:
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..1e4596c 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -31,7 +31,8 @@ contract GobblerReserve is Owned {
   31,  31:     /// @notice Withdraw gobblers from the reserve.
   32,  32:     /// @param to The address to transfer the gobblers to.
   33,  33:     /// @param ids The ids of the gobblers to transfer.
-  34     :-    function withdraw(address to, uint256[] calldata ids) external onlyOwner {
+       34:+    function withdraw(address to, uint256[] calldata ids) external {
+       35:+        onlyOwner();
   35,  36:         // This is quite inefficient, but it's okay because this is not a hot path.
   36,  37:         unchecked {
   37,  38:             for (uint256 i = 0; i < ids.length; i++) {
```

---

#### 3. **Using bools for storage incurs overhead (5 instances)**

- Deployment. Gas Saved: **30 047**

- Minumal Method Call. Gas Saved: **15 042 ± 35 000**

- Average Method Call. Gas Saved: **10 185 ± 35 000**

- Maximum Method Call. Gas Saved: **15 160 ± 35 000**

- `forge snapshot --diff`. **Overall gas change: -360 (-0.356%)**

```
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
```

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past

##### - 2022-09-artgobblers/src/ArtGobblers.sol:[149](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L149)

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..34a4a5d 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -146,7 +146,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  146, 146:     bytes32 public immutable merkleRoot;
  147, 147:
  148, 148:     /// @notice Mapping to keep track of which addresses have claimed from mintlist.
- 149     :-    mapping(address => bool) public hasClaimedMintlistGobbler;
+      149:+    mapping(address => uint256) public hasClaimedMintlistGobbler;
  150, 150:
  151, 151:     /*//////////////////////////////////////////////////////////////
  152, 152:                             VRGDA INPUT STATE
@@ -341,12 +341,12 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  341, 341:         if (mintStart > block.timestamp) revert MintStartPending();
  342, 342:
  343, 343:         // If the user has already claimed, revert.
- 344     :-        if (hasClaimedMintlistGobbler[msg.sender]) revert AlreadyClaimed();
+      344:+        if (hasClaimedMintlistGobbler[msg.sender]==1) revert AlreadyClaimed();
  345, 345:
  346, 346:         // If the user's proof is invalid, revert.
  347, 347:         if (!MerkleProofLib.verify(proof, merkleRoot, keccak256(abi.encodePacked(msg.sender)))) revert InvalidProof();
  348, 348:
- 349     :-        hasClaimedMintlistGobbler[msg.sender] = true;
+      349:+        hasClaimedMintlistGobbler[msg.sender] = 1;
  350, 350:
  351, 351:         unchecked {
  352, 352:             // Overflow should be impossible due to supply cap of 10,000.
```

##### - 2022-09-artgobblers/lib/solmate/src/tokens/ERC721.sol:[51](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L51)

```diff
Submodule lib/solmate contains modified content
diff --git a/lib/solmate/src/tokens/ERC721.sol b/lib/solmate/src/tokens/ERC721.sol
index b47f271..6d54151 100644
--- a/lib/solmate/src/tokens/ERC721.sol
+++ b/lib/solmate/src/tokens/ERC721.sol
@@ -48,7 +48,7 @@ abstract contract ERC721 {
   48,  48:
   49,  49:     mapping(uint256 => address) public getApproved;
   50,  50:
-  51     :-    mapping(address => mapping(address => bool)) public isApprovedForAll;
+       51:+    mapping(address => mapping(address => uint256)) public isApprovedForAll;
   52,  52:
   53,  53:     /*//////////////////////////////////////////////////////////////
   54,  54:                                CONSTRUCTOR
@@ -66,7 +66,7 @@ abstract contract ERC721 {
   66,  66:     function approve(address spender, uint256 id) public virtual {
   67,  67:         address owner = _ownerOf[id];
   68,  68:
-  69     :-        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
+       69:+        require(msg.sender == owner || isApprovedForAll[owner][msg.sender]==1, "NOT_AUTHORIZED");
   70,  70:
   71,  71:         getApproved[id] = spender;
   72,  72:
@@ -74,7 +74,7 @@ abstract contract ERC721 {
   74,  74:     }
   75,  75:
   76,  76:     function setApprovalForAll(address operator, bool approved) public virtual {
-  77     :-        isApprovedForAll[msg.sender][operator] = approved;
+       77:+        isApprovedForAll[msg.sender][operator] = approved?1:0;
   78,  78:
   79,  79:         emit ApprovalForAll(msg.sender, operator, approved);
   80,  80:     }
@@ -89,7 +89,7 @@ abstract contract ERC721 {
   89,  89:         require(to != address(0), "INVALID_RECIPIENT");
   90,  90:
   91,  91:         require(
-  92     :-            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
+       92:+            msg.sender == from || isApprovedForAll[from][msg.sender]==1 || msg.sender == getApproved[id],
   93,  93:             "NOT_AUTHORIZED"
   94,  94:         );
   95,  95:
```

##### - 2022-09-artgobblers/src/utils/token/GobblersERC721.sol:[77](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L77)

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 34a4a5d..7421c15 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -887,7 +887,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  887, 887:         require(to != address(0), "INVALID_RECIPIENT");
  888, 888:
  889, 889:         require(
- 890     :-            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
+      890:+            msg.sender == from || isApprovedForAll[from][msg.sender]==1 || msg.sender == getApproved[id],
  891, 891:             "NOT_AUTHORIZED"
  892, 892:         );
  893, 893:
diff --git a/src/utils/token/GobblersERC721.sol b/src/utils/token/GobblersERC721.sol
index f2d0647..13047e2 100644
--- a/src/utils/token/GobblersERC721.sol
+++ b/src/utils/token/GobblersERC721.sol
@@ -74,7 +74,7 @@ abstract contract GobblersERC721 {
   74,  74:
   75,  75:     mapping(uint256 => address) public getApproved;
   76,  76:
-  77     :-    mapping(address => mapping(address => bool)) public isApprovedForAll;
+       77:+    mapping(address => mapping(address => uint256)) public isApprovedForAll;
   78,  78:
   79,  79:     /*//////////////////////////////////////////////////////////////
   80,  80:                                CONSTRUCTOR
@@ -92,7 +92,7 @@ abstract contract GobblersERC721 {
   92,  92:     function approve(address spender, uint256 id) external {
   93,  93:         address owner = getGobblerData[id].owner;
   94,  94:
-  95     :-        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
+       95:+        require(msg.sender == owner || isApprovedForAll[owner][msg.sender]==1, "NOT_AUTHORIZED");
   96,  96:
   97,  97:         getApproved[id] = spender;
   98,  98:
@@ -100,7 +100,7 @@ abstract contract GobblersERC721 {
  100, 100:     }
  101, 101:
  102, 102:     function setApprovalForAll(address operator, bool approved) external {
- 103     :-        isApprovedForAll[msg.sender][operator] = approved;
+      103:+        isApprovedForAll[msg.sender][operator] = approved?1:0;
  104, 104:
  105, 105:         emit ApprovalForAll(msg.sender, operator, approved);
  106, 106:     }
```

##### - 2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol:[37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L37)

```diff
diff --git a/src/utils/token/GobblersERC1155B.sol b/src/utils/token/GobblersERC1155B.sol
index ccf5cd0..131d8e5 100644
--- a/src/utils/token/GobblersERC1155B.sol
+++ b/src/utils/token/GobblersERC1155B.sol
@@ -34,7 +34,7 @@ abstract contract GobblersERC1155B {
   34,  34:                              ERC1155 STORAGE
   35,  35:     //////////////////////////////////////////////////////////////*/
   36,  36:
-  37     :-    mapping(address => mapping(address => bool)) public isApprovedForAll;
+       37:+    mapping(address => mapping(address => uint256)) public isApprovedForAll;
   38,  38:
   39,  39:     /*//////////////////////////////////////////////////////////////
   40,  40:                         GOBBLERS/ERC1155B STORAGE
@@ -77,7 +77,7 @@ abstract contract GobblersERC1155B {
   77,  77:     //////////////////////////////////////////////////////////////*/
   78,  78:
   79,  79:     function setApprovalForAll(address operator, bool approved) public virtual {
-  80     :-        isApprovedForAll[msg.sender][operator] = approved;
+       80:+        isApprovedForAll[msg.sender][operator] = approved?1:0;
   81,  81:
   82,  82:         emit ApprovalForAll(msg.sender, operator, approved);
   83,  83:     }
```

##### - 2022-09-artgobblers/src/utils/token/PagesERC721.sol:[70](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L70)

```diff
diff --git a/src/utils/token/PagesERC721.sol b/src/utils/token/PagesERC721.sol
index 4bbca84..6698151 100644
--- a/src/utils/token/PagesERC721.sol
+++ b/src/utils/token/PagesERC721.sol
@@ -67,12 +67,12 @@ abstract contract PagesERC721 {
   67,  67:
   68,  68:     mapping(uint256 => address) public getApproved;
   69,  69:
-  70     :-    mapping(address => mapping(address => bool)) internal _isApprovedForAll;
+       70:+    mapping(address => mapping(address => uint256)) internal _isApprovedForAll;
   71,  71:
   72,  72:     function isApprovedForAll(address owner, address operator) public view returns (bool isApproved) {
   73,  73:         if (operator == address(artGobblers)) return true; // Skip approvals for the ArtGobblers contract.
   74,  74:
-  75     :-        return _isApprovedForAll[owner][operator];
+       75:+        return _isApprovedForAll[owner][operator]==1;
   76,  76:     }
   77,  77:
   78,  78:     /*//////////////////////////////////////////////////////////////
@@ -90,7 +90,7 @@ abstract contract PagesERC721 {
   90,  90:     }
   91,  91:
   92,  92:     function setApprovalForAll(address operator, bool approved) external {
-  93     :-        _isApprovedForAll[msg.sender][operator] = approved;
+       93:+        _isApprovedForAll[msg.sender][operator] = approved?1:0;
   94,  94:
   95,  95:         emit ApprovalForAll(msg.sender, operator, approved);
   96,  96:     }
```

---

#### 4. **State variables should be cached in stack variables rather than re-reading them from storage (2 instances)**

The code can be optimized by minimising the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

- Deployment. Gas Saved: **12 819**

- Minumal Method Call. Gas Saved: **28 790 ± 35 000**

- Average Method Call. Gas Saved: **19 914 ± 35 000**

- Maximum Method Call. Gas Saved: **13 107 ± 35 000**

- `forge snapshot --diff`. **Overall gas change: -17 963 (-1.914%)**

##### - 2022-09-artgobblers/src/ArtGobblers.sol:[437](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L437), [494](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L494)

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..7db8229 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -433,12 +433,13 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  433, 433:                 id = gobblerIds[i];
  434, 434:
  435, 435:                 if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);
+      436:+                GobblerData storage gobblerData = getGobblerData[id];
  436, 437:
- 437     :-                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
+      438:+                require(gobblerData.owner == msg.sender, "WRONG_FROM");
  438, 439:
- 439     :-                burnedMultipleTotal += getGobblerData[id].emissionMultiple;
+      440:+                burnedMultipleTotal += gobblerData.emissionMultiple;
  440, 441:
- 441     :-                emit Transfer(msg.sender, getGobblerData[id].owner = address(0), id);
+      442:+                emit Transfer(msg.sender, gobblerData.owner = address(0), id);
  442, 443:             }
  443, 444:
  444, 445:             /*//////////////////////////////////////////////////////////////
```

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 7db8229..942cfea 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -491,7 +491,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  491, 491:             if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);
  492, 492:
  493, 493:             // Compute how many gobblers were minted since the auction began.
- 494     :-            uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
+      494:+            uint256 numMintedSinceStart = mintedFromGoo - numMintedAtStart;
  495, 495:
  496, 496:             // prettier-ignore
  497, 497:             // If we've minted the full interval or beyond it, the price has decayed to 0.
```

---

#### 5. **++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too) (1 instance)**

- Deployment. Gas Saved: **400**

- Minumal Method Call. Gas Saved: **8 568 ± 35 000**

- Average Method Call. Gas Saved: **-317 ± 35 000**

- Maximum Method Call. Gas Saved: **3 199 ± 35 000**

- `forge snapshot --diff`. **Overall gas change: -6 (-0.001%)**

Saves 6 gas per loop

Pre-increments and pre-decrements are cheaper.

For a uint256 i variable, the following is true with the Optimizer enabled at 10k:

Increment:

i += 1 is the most expensive form
i++ costs 6 gas less than i += 1
++i costs 5 gas less than i++ (11 gas less than i += 1)
Decrement:

i -= 1 is the most expensive form
i-- costs 11 gas less than i -= 1
--i costs 5 gas less than i-- (16 gas less than i -= 1)

**Note:** Sometimes not applicable due to optimizer and other factors. This is the only case that I was able to confirm with the gas report

##### - 2022-09-artgobblers/src/utils/GobblerReserve.sol:[37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37)

```diff
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index da32f9e..5e2bb85 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -34,7 +34,7 @@ contract GobblerReserve is Owned {
   34,  34:     function withdraw(address to, uint256[] calldata ids) external onlyOwner {
   35,  35:         // This is quite inefficient, but it's okay because this is not a hot path.
   36,  36:         unchecked {
-  37     :-            for (uint256 i = 0; i < ids.length; i++) {
+       37:+            for (uint256 i = 0; i < ids.length; ++i) {
   38,  38:                 artGobblers.transferFrom(address(this), to, ids[i]);
   39,  39:             }
   40,  40:         }
```

---

#### 99. **Gas Overall Savings**
- Deployment. Gas Saved: **558 440**

- Minumal Method Call. Gas Saved: **296 900 ± 35 000**

- Average Method Call. Gas Saved: **297 516 ± 35 000**

- Maximum Method Call. Gas Saved: **300 453 ± 35 000**

- `forge snapshot --diff`. **Overall gas change: -29 495 (-4.079%)**

```diff
diff --git a/original.txt b/foundry.txt
index 7769080..5b78a6d 100644
--- a/original.txt
+++ b/foundry.txt
 ╭───────────────────────────────────────────────────────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
 │ lib/chainlink/contracts/src/v0.8/mocks/VRFCoordinatorMock.sol:VRFCoordinatorMock contract ┆                 ┆      ┆        ┆      ┆         │
 ╞═══════════════════════════════════════════════════════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
@@ -191,7 +225,7 @@ Test result: [32mok[0m. 66 passed; 0 failed; finished in 7.25s
 ╞══════════════════════════════════════════════════════════╪═════════════════╪═════════╪═════════╪═════════╪═════════╡
 │ Deployment Cost                                          ┆ Deployment Size ┆         ┆         ┆         ┆         │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ 9374132                                                  ┆ 46638           ┆         ┆         ┆         ┆         │
+│ 9078804                                                  ┆ 45164           ┆         ┆         ┆         ┆         │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ Function Name                                            ┆ min             ┆ avg     ┆ median  ┆ max     ┆ # calls │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
@@ -215,7 +249,7 @@ Test result: [32mok[0m. 66 passed; 0 failed; finished in 7.25s
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ root                                                     ┆ 237             ┆ 237     ┆ 237     ┆ 237     ┆ 1       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ run                                                      ┆ 7960137         ┆ 7960137 ┆ 7960137 ┆ 7960137 ┆ 5       │
+│ run                                                      ┆ 7663698         ┆ 7663698 ┆ 7663698 ┆ 7663698 ┆ 5       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ teamReserve                                              ┆ 2369            ┆ 2369    ┆ 2369    ┆ 2369    ┆ 1       │
 ╰──────────────────────────────────────────────────────────┴─────────────────┴─────────┴─────────┴─────────┴─────────╯
@@ -224,7 +258,7 @@ Test result: [32mok[0m. 66 passed; 0 failed; finished in 7.25s
 ╞═════════════════════════════════════════════╪═════════════════╪═════════╪════════╪══════════╪═════════╡
 │ Deployment Cost                             ┆ Deployment Size ┆         ┆        ┆          ┆         │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ 4033627                                     ┆ 22342           ┆         ┆        ┆          ┆         │
+│ 3890844                                     ┆ 21549           ┆         ┆        ┆          ┆         │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ Function Name                               ┆ min             ┆ avg     ┆ median ┆ max      ┆ # calls │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
@@ -248,7 +282,7 @@ Test result: [32mok[0m. 66 passed; 0 failed; finished in 7.25s
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ burnGooForPages                             ┆ 2392            ┆ 6040    ┆ 4215   ┆ 11515    ┆ 3       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ claimGobbler                                ┆ 574             ┆ 47364   ┆ 48139  ┆ 93282    ┆ 6       │
+│ claimGobbler                                ┆ 574             ┆ 47304   ┆ 48078  ┆ 93164    ┆ 6       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ getCopiesOfArtGobbledByGobbler              ┆ 771             ┆ 771     ┆ 771    ┆ 771      ┆ 3       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
@@ -256,31 +290,31 @@ Test result: [32mok[0m. 66 passed; 0 failed; finished in 7.25s
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ getGobblerEmissionMultiple                  ┆ 543             ┆ 543     ┆ 543    ┆ 543      ┆ 292     │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ getTargetSaleTime                           ┆ 483             ┆ 866     ┆ 1058   ┆ 1058     ┆ 3       │
+│ getTargetSaleTime                           ┆ 417             ┆ 844     ┆ 1058   ┆ 1058     ┆ 3       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ getUserEmissionMultiple                     ┆ 659             ┆ 801     ┆ 659    ┆ 2659     ┆ 14      │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ getVRGDAPrice                               ┆ 554             ┆ 1677    ┆ 1679   ┆ 1679     ┆ 1734    │
+│ getVRGDAPrice                               ┆ 488             ┆ 1678    ┆ 1679   ┆ 1679     ┆ 1734    │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ gobble                                      ┆ 813             ┆ 22003   ┆ 17930  ┆ 53775    ┆ 12      │
+│ gobble                                      ┆ 813             ┆ 21997   ┆ 17930  ┆ 53775    ┆ 12      │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ gobblerPrice                                ┆ 698             ┆ 1640    ┆ 1499   ┆ 3842     ┆ 54909   │
+│ gobblerPrice                                ┆ 632             ┆ 1640    ┆ 1499   ┆ 3842     ┆ 54909   │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ gobblerRevealsData                          ┆ 651             ┆ 1151    ┆ 651    ┆ 2651     ┆ 4       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ gooBalance                                  ┆ 2142            ┆ 2704    ┆ 2142   ┆ 6642     ┆ 21      │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ legendaryGobblerPrice                       ┆ 796             ┆ 1646    ┆ 796    ┆ 4796     ┆ 28      │
+│ legendaryGobblerPrice                       ┆ 687             ┆ 1537    ┆ 687    ┆ 4687     ┆ 28      │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ mintFromGoo                                 ┆ 890             ┆ 30201   ┆ 31598  ┆ 73398    ┆ 54910   │
+│ mintFromGoo                                 ┆ 824             ┆ 30201   ┆ 31598  ┆ 73398    ┆ 54910   │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ mintLegendaryGobbler                        ┆ 911             ┆ 72774   ┆ 31201  ┆ 456707   ┆ 23      │
+│ mintLegendaryGobbler                        ┆ 911             ┆ 71912   ┆ 31092  ┆ 452921   ┆ 23      │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ mintReservedGobblers                        ┆ 711             ┆ 3742247 ┆ 52000  ┆ 38699404 ┆ 21      │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ onERC1155Received                           ┆ 860             ┆ 860     ┆ 860    ┆ 860      ┆ 6       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ ownerOf                                     ┆ 621             ┆ 629     ┆ 621    ┆ 648      ┆ 451     │
+│ ownerOf                                     ┆ 582             ┆ 609     ┆ 621    ┆ 621      ┆ 451     │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ randProvider                                ┆ 426             ┆ 1426    ┆ 1426   ┆ 2426     ┆ 2       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
@@ -294,11 +328,11 @@ Test result: [32mok[0m. 66 passed; 0 failed; finished in 7.25s
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ tokenURI                                    ┆ 1395            ┆ 1683    ┆ 1395   ┆ 7187     ┆ 206     │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ transferFrom(address,address,uint256)       ┆ 43824           ┆ 43824   ┆ 43824  ┆ 43824    ┆ 1       │
+│ transferFrom(address,address,uint256)       ┆ 9836            ┆ 22036   ┆ 22036  ┆ 34236    ┆ 2       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ transferFrom(address,address,uint256)(bool) ┆ 9824            ┆ 25874   ┆ 29724  ┆ 34224    ┆ 4       │
+│ transferFrom(address,address,uint256)(bool) ┆ 29736           ┆ 34436   ┆ 29736  ┆ 43836    ┆ 3       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ upgradeRandProvider                         ┆ 2582            ┆ 4804    ┆ 2653   ┆ 9179     ┆ 3       │
+│ upgradeRandProvider                         ┆ 2531            ┆ 4803    ┆ 2677   ┆ 9203     ┆ 3       │
 ╰─────────────────────────────────────────────┴─────────────────┴─────────┴────────┴──────────┴─────────╯
 ╭──────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
 │ src/Goo.sol:Goo contract ┆                 ┆       ┆        ┆       ┆         │
@@ -323,49 +357,51 @@ Test result: [32mok[0m. 66 passed; 0 failed; finished in 7.25s
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ totalSupply              ┆ 363             ┆ 863   ┆ 363    ┆ 2363  ┆ 4       │
 ╰──────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
-╭──────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
-│ src/Pages.sol:Pages contract ┆                 ┆       ┆        ┆       ┆         │
-╞══════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
-│ Deployment Cost              ┆ Deployment Size ┆       ┆        ┆       ┆         │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ 1744161                      ┆ 10801           ┆       ┆        ┆       ┆         │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ Function Name                ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ BASE_URI                     ┆ 7568            ┆ 7568  ┆ 7568   ┆ 7568  ┆ 1       │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ artGobblers                  ┆ 272             ┆ 272   ┆ 272    ┆ 272   ┆ 1       │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ getTargetSaleTime            ┆ 413             ┆ 805   ┆ 1099   ┆ 1099  ┆ 7       │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ getVRGDAPrice                ┆ 1079            ┆ 1754  ┆ 1765   ┆ 1765  ┆ 8467    │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ goo                          ┆ 250             ┆ 250   ┆ 250    ┆ 250   ┆ 1       │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ mintCommunityPages           ┆ 635             ┆ 30822 ┆ 27113  ┆ 73492 ┆ 16      │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ mintFromGoo                  ┆ 517             ┆ 29476 ┆ 31589  ┆ 75096 ┆ 13333   │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ ownerOf                      ┆ 532             ┆ 532   ┆ 532    ┆ 532   ┆ 5       │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ pagePrice                    ┆ 914             ┆ 1670  ┆ 1600   ┆ 3943  ┆ 13331   │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ targetPrice                  ┆ 239             ┆ 239   ┆ 239    ┆ 239   ┆ 3       │
-├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ transferFrom                 ┆ 2795            ┆ 12529 ┆ 12529  ┆ 22264 ┆ 2       │
-╰──────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
+╭─────────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
+│ src/Pages.sol:Pages contract                ┆                 ┆       ┆        ┆       ┆         │
+╞═════════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
+│ Deployment Cost                             ┆ Deployment Size ┆       ┆        ┆       ┆         │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ 1656863                                     ┆ 10285           ┆       ┆        ┆       ┆         │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ Function Name                               ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ BASE_URI                                    ┆ 7568            ┆ 7568  ┆ 7568   ┆ 7568  ┆ 1       │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ artGobblers                                 ┆ 272             ┆ 272   ┆ 272    ┆ 272   ┆ 1       │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ getTargetSaleTime                           ┆ 413             ┆ 805   ┆ 1099   ┆ 1099  ┆ 7       │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ getVRGDAPrice                               ┆ 1079            ┆ 1754  ┆ 1765   ┆ 1765  ┆ 8467    │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ goo                                         ┆ 250             ┆ 250   ┆ 250    ┆ 250   ┆ 1       │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ mintCommunityPages                          ┆ 635             ┆ 30822 ┆ 27113  ┆ 73492 ┆ 16      │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ mintFromGoo                                 ┆ 517             ┆ 29476 ┆ 31589  ┆ 75096 ┆ 13333   │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ ownerOf                                     ┆ 532             ┆ 532   ┆ 532    ┆ 532   ┆ 5       │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ pagePrice                                   ┆ 914             ┆ 1670  ┆ 1600   ┆ 3943  ┆ 13331   │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ targetPrice                                 ┆ 239             ┆ 239   ┆ 239    ┆ 239   ┆ 3       │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ transferFrom(address,address,uint256)       ┆ 2729            ┆ 2729  ┆ 2729   ┆ 2729  ┆ 1       │
+├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
+│ transferFrom(address,address,uint256)(bool) ┆ 22276           ┆ 22276 ┆ 22276  ┆ 22276 ┆ 1       │
+╰─────────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
 ╭──────────────────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
 │ src/utils/GobblerReserve.sol:GobblerReserve contract ┆                 ┆       ┆        ┆       ┆         │
 ╞══════════════════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
 │ Deployment Cost                                      ┆ Deployment Size ┆       ┆        ┆       ┆         │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ 251133                                               ┆ 1451            ┆       ┆        ┆       ┆         │
+│ 218102                                               ┆ 1286            ┆       ┆        ┆       ┆         │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ Function Name                                        ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
 │ owner                                                ┆ 2345            ┆ 2345  ┆ 2345   ┆ 2345  ┆ 2       │
 ├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
-│ withdraw                                             ┆ 13027           ┆ 25227 ┆ 25227  ┆ 37427 ┆ 2       │
+│ withdraw                                             ┆ 13059           ┆ 25259 ┆ 25259  ┆ 37459 ┆ 2       │
 ╰──────────────────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
 ╭─────────────────────────────────────────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
 │ src/utils/rand/ChainlinkV1RandProvider.sol:ChainlinkV1RandProvider contract ┆                 ┆       ┆        ┆       ┆         │
```