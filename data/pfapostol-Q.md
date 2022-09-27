# Summary

## Low Risk Issues

|       | Issue                                                                                 | Instances |
| :---: | :------------------------------------------------------------------------------------ | :-------: |
| **1** | Parameter missed in NatSpec documentation                                             |     1     |
| **2** | Lack of zero checks for immutable variables                                           |    15     |
| **3** | Insufficient validation of user input                                                 |     1     |
| **4** | Critical changes should use two-step procedure                                        |     1     |
| **5** | There is no way to predict how many reserve gobblers/ community pages should you mint |     2     |

## Total: 20 instances over 5 issues

---

#### 1. **Parameter missed in NatSpec documentation (1 instance)**

##### - src/ArtGobblers.sol:[278-286]()

Pages \_pages not covered in NatSpec

```solidity
278	    /// @notice Sets VRGDA parameters, mint config, relevant addresses, and URIs.
279	    /// @param _merkleRoot Merkle root of mint mintlist.
280	    /// @param _mintStart Timestamp for the start of the VRGDA mint.
281	    /// @param _goo Address of the Goo contract.
282	    /// @param _team Address of the team reserve.
283	    /// @param _community Address of the community reserve.
284	    /// @param _randProvider Address of the randomness provider.
285	    /// @param _baseUri Base URI for revealed gobblers.
286	    /// @param _unrevealedUri URI for unrevealed gobblers.
287	    constructor(
288	        // Mint config:
289	        bytes32 _merkleRoot,
290	        uint256 _mintStart,
291	        // Addresses:
292	        Goo _goo,
293	        Pages _pages,
```

---

#### 2. **Lack of zero checks for immutable variables (15 instances)**

##### - src/ArtGobblers.sol:[314](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L314), [564](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L564)

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..7ffd93f 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -245,6 +245,8 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  245, 245:                                  ERRORS
  246, 246:     //////////////////////////////////////////////////////////////*/
  247, 247:
+      248:+    error ZeroAddress();
+      249:+
  248, 250:     error InvalidProof();
  249, 251:     error AlreadyClaimed();
  250, 252:     error MintStartPending();
@@ -308,6 +310,13 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  308, 310:             0.0023e18 // Time scale.
  309, 311:         )
  310, 312:     {
+      313:+        if (
+      314:+            address(_goo) == address(0) ||
+      315:+            address(_pages) == address(0) ||
+      316:+            _team == address(0) ||
+      317:+            _community == address(0) ||
+      318:+            address(_randProvider) == address(0)
+      319:+        ) revert ZeroAddress();
  311, 320:         mintStart = _mintStart;
  312, 321:         merkleRoot = _merkleRoot;
  313, 322:
@@ -560,6 +569,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  560, 569:     function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
  561, 570:         // Revert if waiting for seed, so we don't interrupt requests in flight.
  562, 571:         if (gobblerRevealsData.waitingForSeed) revert SeedPending();
+      572:+        if (address(newRandProvider) == address(0)) revert ZeroAddress();
  563, 573:
  564, 574:         randProvider = newRandProvider; // Update the randomness provider.
```

##### - src/utils/rand/ChainlinkV1RandProvider.sol:[55](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L55)

```diff
diff --git a/src/utils/rand/ChainlinkV1RandProvider.sol b/src/utils/rand/ChainlinkV1RandProvider.sol
index d9d48fb..58dc228 100644
--- a/src/utils/rand/ChainlinkV1RandProvider.sol
+++ b/src/utils/rand/ChainlinkV1RandProvider.sol
@@ -34,6 +34,7 @@ contract ChainlinkV1RandProvider is RandProvider, VRFConsumerBase {
   34,  34:     //////////////////////////////////////////////////////////////*/
   35,  35:
   36,  36:     error NotGobblers();
+       37:+    error ZeroAddress();
   37,  38:
   38,  39:     /*//////////////////////////////////////////////////////////////
   39,  40:                                CONSTRUCTOR
@@ -52,6 +53,7 @@ contract ChainlinkV1RandProvider is RandProvider, VRFConsumerBase {
   52,  53:         bytes32 _chainlinkKeyHash,
   53,  54:         uint256 _chainlinkFee
   54,  55:     ) VRFConsumerBase(_vrfCoordinator, _linkToken) {
+       56:+        if (address(_artGobblers) == address(0)) revert ZeroAddress();
   55,  57:         artGobblers = _artGobblers;
   56,  58:
   57,  59:         chainlinkKeyHash = _chainlinkKeyHash;
```

##### - src/utils/GobblerReserve.sol:[24](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L24)

```diff
diff --git a/src/utils/GobblerReserve.sol b/src/utils/GobblerReserve.sol
index 1e4596c..13ce1a6 100644
--- a/src/utils/GobblerReserve.sol
+++ b/src/utils/GobblerReserve.sol
@@ -14,6 +14,8 @@ contract GobblerReserve is Owned {
   14,  14:                                 ADDRESSES
   15,  15:     //////////////////////////////////////////////////////////////*/
   16,  16:
+       17:+    error ZeroAddress();
+       18:+
   17,  19:     /// @notice Art Gobblers contract address.
   18,  20:     ArtGobblers public immutable artGobblers;
   19,  21:
@@ -21,6 +23,7 @@ contract GobblerReserve is Owned {
   21,  23:     /// @param _artGobblers The address of the ArtGobblers contract.
   22,  24:     /// @param _owner The address of the owner of Gobbler Reserve.
   23,  25:     constructor(ArtGobblers _artGobblers, address _owner) Owned(_owner) {
+       26:+        if (address(_artGobblers) == address(0)) revert ZeroAddress();
   24,  27:         artGobblers = _artGobblers;
   25,  28:     }
   26,  29:
```

##### - src/Goo.sol:[83](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Goo.sol#L83)

```diff
diff --git a/src/Goo.sol b/src/Goo.sol
index bcc459c..21ed4d4 100644
--- a/src/Goo.sol
+++ b/src/Goo.sol
@@ -80,6 +80,7 @@ contract Goo is ERC20("Goo", "GOO", 18) {
   80,  80:     /// @param _artGobblers Address of the ArtGobblers contract.
   81,  81:     /// @param _pages Address of the Pages contract.
   82,  82:     constructor(address _artGobblers, address _pages) {
+       83:+        if (_artGobblers == address(0) || _pages == address(0)) revert ZeroAddress();
   83,  84:         artGobblers = _artGobblers;
   84,  85:         pages = _pages;
   85,  86:     }
```

##### - lib/solmate/src/auth/Owned.sol:[30](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L30), [40](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L30)

```diff
diff --git a/lib/solmate/src/auth/Owned.sol b/lib/solmate/src/auth/Owned.sol
index 01cb6df..f208190 100644
--- a/lib/solmate/src/auth/Owned.sol
+++ b/lib/solmate/src/auth/Owned.sol
@@ -4,6 +4,7 @@ pragma solidity >=0.8.0;
    4,   4: /// @notice Simple single owner authorization mixin.
    5,   5: /// @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/Owned.sol)
    6,   6: abstract contract Owned {
+        7:+    error ZeroAddress();
    7,   8:     /*//////////////////////////////////////////////////////////////
    8,   9:                                  EVENTS
    9,  10:     //////////////////////////////////////////////////////////////*/
@@ -25,6 +26,7 @@ abstract contract Owned {
   25,  26:     //////////////////////////////////////////////////////////////*/
   26,  27:
   27,  28:     constructor(address _owner) {
+       29:+        if (_owner == address(0)) revert ZeroAddress();
   28,  30:         owner = _owner;
   29,  31:
   30,  32:         emit OwnerUpdated(address(0), _owner);
@@ -36,6 +38,7 @@ abstract contract Owned {
   36,  38:
   37,  39:     function setOwner(address newOwner) public virtual onlyOwner {
   38,  40:
+       41:+        if (newOwner == address(0)) revert ZeroAddress();
   39,  42:         owner = newOwner;
   40,  43:
   41,  44:         emit OwnerUpdated(msg.sender, newOwner);
```

##### - src/Pages.sol:[179](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L179)

```diff
diff --git a/src/Pages.sol b/src/Pages.sol
index 0d4ac88..703f91f 100644
--- a/src/Pages.sol
+++ b/src/Pages.sol
@@ -139,6 +139,8 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
  139, 139:                                  ERRORS
  140, 140:     //////////////////////////////////////////////////////////////*/
  141, 141:
+      142:+    error ZeroAddress();
+      143:+
  142, 144:     error ReserveImbalance();
  143, 145:
  144, 146:     error PriceExceededMax(uint256 currentPrice);
@@ -174,6 +176,7 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
  174, 176:             9e18 // Pages to target per day.
  175, 177:         )
  176, 178:     {
+      179:+        if (address(_goo) == address(0) || _community == address(0)) revert ZeroAddress();
  177, 180:         mintStart = _mintStart;
  178, 181:
  179, 182:         goo = _goo;
```

##### - src/utils/token/PagesERC721.sol:[43](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L43)

```diff
diff --git a/src/utils/token/PagesERC721.sol b/src/utils/token/PagesERC721.sol
index 4bbca84..668f9a2 100644
--- a/src/utils/token/PagesERC721.sol
+++ b/src/utils/token/PagesERC721.sol
@@ -7,6 +7,7 @@ import {ArtGobblers} from "../../ArtGobblers.sol";
    7,   7: /// @notice ERC721 implementation optimized for Pages by pre-approving them to the ArtGobblers contract.
    8,   8: /// @author Modified from Solmate (https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC721.sol)
    9,   9: abstract contract PagesERC721 {
+       10:+    error ZeroAddress();
   10,  11:     /*//////////////////////////////////////////////////////////////
   11,  12:                                  EVENTS
   12,  13:     //////////////////////////////////////////////////////////////*/
@@ -38,6 +39,7 @@ abstract contract PagesERC721 {
   38,  39:         string memory _name,
   39,  40:         string memory _symbol
   40,  41:     ) {
+       42:+        if (address(_artGobblers) == address(0)) revert ZeroAddress();
   41,  43:         name = _name;
   42,  44:         symbol = _symbol;
   43,  45:         artGobblers = _artGobblers;
```

---

#### 3. **Insufficient validation of user input (1 instance)**

mintReservedGobblers function does not check for overflow in `numGobblersEach << 1`

##### Impact:

In this case, the impact is very limited. If the contract is deployed on the mainnet, then the gas will be quickly spent. But if it is deployed in a network with cheap gas, or the attacker has an unlimited amount of gas, then he will be able to cause DOS or, in the worst case, mint a lot of gobblers

##### - src/ArtGobblers.sol:[839](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L839)

```diff
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -245,6 +245,8 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  245, 245:                                  ERRORS
  246, 246:     //////////////////////////////////////////////////////////////*/
  247, 247:
+      248:+    error OutOfReserved();
+      249:+
  248, 250:     error InvalidProof();
  249, 251:     error AlreadyClaimed();
  250, 252:     error MintStartPending();
@@ -837,6 +839,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  837, 839:     /// @dev Gobblers minted to reserves cannot comprise more than 20% of the sum of
  838, 840:     /// the supply of goo minted gobblers and the supply of gobblers minted to reserves.
  839, 841:     function mintReservedGobblers(uint256 numGobblersEach) external returns (uint256 lastMintedGobblerId) {
+      842:+        if (numGobblersEach > 799) revert OutOfReserved();
  840, 843:         unchecked {
  841, 844:             // Optimistically increment numMintedForReserves, may be reverted below. Overflow in this
  842, 845:             // calculation is possible but numGobblersEach would have to be so large that it would cause the
```

---

#### 4. **Critical changes should use two-step procedure (1 instance)**

##### - lib/solmate/src/auth/Owned.sol:[37](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L37)

```diff
Submodule lib/solmate contains modified content
Submodule lib/solmate bbd297d..2bebde9:
diff --git a/lib/solmate/src/auth/Owned.sol b/lib/solmate/src/auth/Owned.sol
index 01cb6df..ce0dede 100644
--- a/lib/solmate/src/auth/Owned.sol
+++ b/lib/solmate/src/auth/Owned.sol
@@ -4,17 +4,20 @@ pragma solidity >=0.8.0;
    4,   4: /// @notice Simple single owner authorization mixin.
    5,   5: /// @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/Owned.sol)
    6,   6: abstract contract Owned {
+        7:+    error ZeroAddress();
    7,   8:     /*//////////////////////////////////////////////////////////////
    8,   9:                                  EVENTS
    9,  10:     //////////////////////////////////////////////////////////////*/
   10,  11:
   11,  12:     event OwnerUpdated(address indexed user, address indexed newOwner);
+       13:+    event PendingOwnerUpdated(address indexed user, address indexed newOwner);
   12,  14:
   13,  15:     /*//////////////////////////////////////////////////////////////
   14,  16:                             OWNERSHIP STORAGE
   15,  17:     //////////////////////////////////////////////////////////////*/
   16,  18:
   17,  19:     address public owner;
+       20:+    address public pending_owner;
   18,  21:
   19,  22:     function onlyOwner() internal virtual {
   20,  23:         require(msg.sender == owner, "UNAUTHORIZED");
@@ -25,6 +28,7 @@ abstract contract Owned {
   25,  28:     //////////////////////////////////////////////////////////////*/
   26,  29:
   27,  30:     constructor(address _owner) {
+       31:+        if (_owner == address(0)) revert ZeroAddress();
   28,  32:         owner = _owner;
   29,  33:
   30,  34:         emit OwnerUpdated(address(0), _owner);
@@ -34,10 +38,18 @@ abstract contract Owned {
   34,  38:                              OWNERSHIP LOGIC
   35,  39:     //////////////////////////////////////////////////////////////*/
   36,  40:
-  37     :-    function setOwner(address newOwner) public virtual onlyOwner {
-  38     :-
-  39     :-        owner = newOwner;
+       41:+    function acceptOwner() public virtual {
+       42:+        address po_ = pending_owner;
+       43:+        emit OwnerUpdated(owner, po_);
+       44:+        if (msg.sender != po_) revert OnlyPendingOwner();
+       45:+        owner = po_;
+       46:+        delete pending_owner;
+       47:+    }
+       48:+
+       49:+    function setPendingOwner(address newOwner) public virtual onlyOwner {
+       50:+        if (newOwner == address(0)) revert ZeroAddress();
+       51:+        pending_owner = newOwner;
   40,  52:
-  41     :-        emit OwnerUpdated(msg.sender, newOwner);
+       53:+        emit PendingOwnerUpdated(pending_owner, newOwner);
   42,  54:     }
-  43     :-}
\ No newline at end of file
+       55:+}
```

---

#### 5. **There is no way to predict how many reserve gobblers/ community pages should you mint (2 instance)**

It would be useful for the user to know how many reserves he can mint

##### - src/ArtGobblers.sol:[link](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol)

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..0d317d4 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -857,6 +857,10 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
  857, 857:         emit ReservedGobblersMinted(msg.sender, lastMintedGobblerId, numGobblersEach);
  858, 858:     }
  859, 859:
+      860:+    function reservedGobblersToMint() external view returns (uint256) {
+      861:+        return (numMintedFromGoo  - 4 * numMintedForReserves) / 8;
+      862:+    }
+      863:+
  860, 864:     /*//////////////////////////////////////////////////////////////
  861, 865:                           CONVENIENCE FUNCTIONS
  862, 866:     //////////////////////////////////////////////////////////////*/
```

##### - src/Pages.sol:[link](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol)

```diff
diff --git a/src/Pages.sol b/src/Pages.sol
index 0d4ac88..de054ba 100644
--- a/src/Pages.sol
+++ b/src/Pages.sol
@@ -256,6 +256,10 @@ contract Pages is PagesERC721, LogisticToLinearVRGDA {
  256, 256:         }
  257, 257:     }
  258, 258:
+      259:+    function CommunityPagesToMint() external view returns (uint256) {
+      260:+        return (currentId - 10 * numMintedForCommunity) / 9;
+      261:+    }
+      262:+
  259, 263:     /*//////////////////////////////////////////////////////////////
  260, 264:                              TOKEN URI LOGIC
  261, 265:     //////////////////////////////////////////////////////////////*/
```
