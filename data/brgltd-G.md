# [G-01] Prefix increment costs less gas than postfix increment

There are 2 instances of this issue.

```
File: src/Pages.sol
251: for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

```
File: src/utils/GobblerReserve.sol
37: for (uint256 i = 0; i < ids.length; i++) {
```

# [G-02] Cache the length of the array before the loop

There are 2 instances of this issue.

```
File: src/utils/GobblerReserve.sol
37: for (uint256 i = 0; i < ids.length; i++) {
```

```
File: src/utils/token/GobblersERC1155B.sol
114: for (uint256 i = 0; i < owners.length; ++i) {
```

# [G-03] Initializing a variable with the default value wastes gas

There are 7 instances of this issue.

```
File: src/ArtGobblers.sol
432: for (uint256 i = 0; i < cost; ++i) {
592: for (uint256 i = 0; i < numGobblers; ++i) {
```

```
File: src/Pages.sol
251: for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

```
File: src/utils/GobblerReserve.sol
37: for (uint256 i = 0; i < ids.length; i++) {
```

```
File: src/utils/token/GobblersERC1155B.sol
114: for (uint256 i = 0; i < owners.length; ++i) {
173: for (uint256 i = 0; i < amount; ++i) {
```

```
File: src/utils/token/GobblersERC721.sol
186: for (uint256 i = 0; i < amount; ++i) {
```

# [G-04] Use custom errors rather than require/revert strings to save gas

There are 28 instances of this issue.

```
File: lib/VRGDAs/src/VRGDA.sol
32: require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
```

```
File: lib/solmate/src/auth/Owned.sol
20: require(msg.sender == owner, "UNAUTHORIZED");
```

```
File: lib/solmate/src/tokens/ERC721.sol
36: require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
40: require(owner != address(0), "ZERO_ADDRESS");
69: require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
87: require(from == _ownerOf[id], "WRONG_FROM");
89: require(to != address(0), "INVALID_RECIPIENT");
158: require(to != address(0), "INVALID_RECIPIENT");
160: require(_ownerOf[id] == address(0), "ALREADY_MINTED");
175: require(owner != address(0), "NOT_MINTED");
```

```
File: lib/solmate/src/utils/SignedWadMath.sol
90: if (x >= 135305999368893231589) revert("EXP_OVERFLOW");
142: require(x > 0, "UNDEFINED");
```

```
File: src/ArtGobblers.sol
437: require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
696: if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.
705: if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");
711: revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.
885: require(from == getGobblerData[id].owner, "WRONG_FROM");
887: require(to != address(0), "INVALID_RECIPIENT");
```

```
File: src/Pages.sol
266: if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```

```
File: src/utils/token/GobblersERC1155B.sol
107: require(owners.length == ids.length, "LENGTH_MISMATCH");
```

```
File: src/utils/token/GobblersERC721.sol
62: require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
66: require(owner != address(0), "ZERO_ADDRESS");
95: require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
```

```
File: src/utils/token/PagesERC721.sol
55: require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
59: require(owner != address(0), "ZERO_ADDRESS");
85: require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
103: require(from == _ownerOf[id], "WRONG_FROM");
105: require(to != address(0), "INVALID_RECIPIENT");
```

# [G-05] Use right/left shift instead of division/multiplication to save gas

There are 2 instances of this issue.

```
File: src/ArtGobblers.sol
449: getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
462: cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

# [G-06] Using private rather than public for constants, saves gas

The values can still be inspected on the source code if necessary

There are 11 instances of this issue.

```
File: script/deploy/DeployRinkeby.s.sol
13: string public constant gobblerBaseUri = "https://testnet.ag.xyz/api/nfts/gobblers/";
14: string public constant gobblerUnrevealedUri = "https://testnet.ag.xyz/api/nfts/unrevealed";
15: string public constant pagesBaseUri = "https://testnet.ag.xyz/api/nfts/pages/";
```

```
File: src/ArtGobblers.sol
112: uint256 public constant MAX_SUPPLY = 10000;
115: uint256 public constant MINTLIST_SUPPLY = 2000;
118: uint256 public constant LEGENDARY_SUPPLY = 10;
122: uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
126: uint256 public constant MAX_MINTABLE = MAX_SUPPLY
177: uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
180: uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
184: uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```

# [G-07] x += y costs more gas than x = x + y for state variables

There's 1 instance with this issue.

```
File: src/ArtGobblers.sol
464: legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
```

# [G-08] Repeated validation logic can be refactored into a single function modifier to save gas

The logic for `if (gobblerRevealsData.waitingForSeed) revert SeedPending()` is used across `upgradeRandProvider()` and `revealGobblers()`.

```
function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
    // Revert if waiting for seed, so we don't interrupt requests in flight.
    if (gobblerRevealsData.waitingForSeed) revert SeedPending();
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L560-L562

```
function revealGobblers(uint256 numGobblers) external {
    uint256 randomSeed = gobblerRevealsData.randomSeed;

    uint256 lastRevealedId = gobblerRevealsData.lastRevealedId;

    uint256 totalRemainingToBeRevealed = gobblerRevealsData.toBeRevealed;

    // Can't reveal if we're still waiting for a new seed.
    if (gobblerRevealsData.waitingForSeed) revert SeedPending();
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L576-L584

It can be converted into a single modifier to save gas and improve code reusability.

```
diff --git a/ArtGobblers.sol.orig b/ArtGobblers.sol
index 7a5925a..7700255 100644
--- a/ArtGobblers.sol.orig
+++ b/ArtGobblers.sol
@@ -271,6 +271,12 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv

     error UnauthorizedCaller(address caller);

+
+    modifier notPending(bool isWaitingForSeed) {
+        // Revert if waiting for seed, so we don't interrupt requests in flight.
+        if (isWaitingForSeed) revert SeedPending();
+    }
+
     /*//////////////////////////////////////////////////////////////
                                CONSTRUCTOR
     //////////////////////////////////////////////////////////////*/
@@ -557,10 +563,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiver

     /// @notice Upgrade the rand provider contract. Useful if current VRF is sunset.
     /// @param newRandProvider The new randomness provider contract address.
-    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
-        // Revert if waiting for seed, so we don't interrupt requests in flight.
-        if (gobblerRevealsData.waitingForSeed) revert SeedPending();
-
+    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner notPending(gobblerRevealsData.waitingForSeed) {
         randProvider = newRandProvider; // Update the randomness provider.

         emit RandProviderUpgraded(msg.sender, newRandProvider);
@@ -573,16 +576,13 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiver
     /// @notice Knuth shuffle to progressively reveal
     /// new gobblers using entropy from a random seed.
     /// @param numGobblers The number of gobblers to reveal.
-    function revealGobblers(uint256 numGobblers) external {
+    function revealGobblers(uint256 numGobblers) external notPending(gobblerRevealsData.waitingForSeed) {
         uint256 randomSeed = gobblerRevealsData.randomSeed;

         uint256 lastRevealedId = gobblerRevealsData.lastRevealedId;

         uint256 totalRemainingToBeRevealed = gobblerRevealsData.toBeRevealed;

-        // Can't reveal if we're still waiting for a new seed.
-        if (gobblerRevealsData.waitingForSeed) revert SeedPending();
-
         // Can't reveal more gobblers than are currently remaining to be revealed with the seed.
         if (numGobblers > totalRemainingToBeRevealed) revert NotEnoughRemainingToBeRevealed(totalRemainingToBeRevealed);
```
