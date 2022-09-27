## [G-01] `mintedFromGoo` CAN BE USED INSTEAD OF `numMintedFromGoo`
Calling the following `legendaryGobblerPrice` function executes `uint256 mintedFromGoo = numMintedFromGoo` before `uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart`, where `numMintedFromGoo` is a state variable. Because `mintedFromGoo` is cached in memory, using it instead of `numMintedFromGoo` for calculating `numMintedSinceStart` would replace an `sload` operation with an `mload` operation. Hence, please consider changing the code to `uint256 numMintedSinceStart = mintedFromGoo - numMintedAtStart` to save gas.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L478-L501
```solidity
    function legendaryGobblerPrice() public view returns (uint256) {
        // Retrieve and cache various auction parameters and variables.
        uint256 startPrice = legendaryGobblerAuctionData.startPrice;
        uint256 numSold = legendaryGobblerAuctionData.numSold;
        uint256 mintedFromGoo = numMintedFromGoo;

        unchecked {
            // The number of gobblers minted at the start of the auction is computed by multiplying the # of
            // intervals that must pass before the next auction begins by the number of gobblers in each interval.
            uint256 numMintedAtStart = (numSold + 1) * LEGENDARY_AUCTION_INTERVAL;

            // If not enough gobblers have been minted to start the auction yet, return how many need to be minted.
            if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);

            // Compute how many gobblers were minted since the auction began.
            uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;

            // prettier-ignore
            // If we've minted the full interval or beyond it, the price has decayed to 0.
            if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;
            // Otherwise decay the price linearly based on what fraction of the interval has been minted.
            else return FixedPointMathLib.unsafeDivUp(startPrice * (LEGENDARY_AUCTION_INTERVAL - numMintedSinceStart), LEGENDARY_AUCTION_INTERVAL);
        }
    }
```

## [G-02] `getGobblerData[swapId].idx` AND `getGobblerData[currentId].idx` CAN BE CACHED FOR USING THEIR CACHED VALUES
In the following code, `getGobblerData[swapId].idx` can be cached in memory, and then its cached value can be used. This saves gas by replacing two `sload` operations with one `sload`, one `mstore`, and two `mload` operations.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L615-L617
```solidity
	uint64 swapIndex = getGobblerData[swapId].idx == 0
		? uint64(swapId) // Hasn't been shuffled before.
		: getGobblerData[swapId].idx; // Shuffled before.
```

Similarly, `getGobblerData[currentId].idx` can be cached in memory, and its cached value can then be used in the following code. This also saves gas by replacing two `sload` operations with one `sload`, one `mstore`, and two `mload` operations.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L623-L625
```solidity
	uint64 currentIndex = getGobblerData[currentId].idx == 0
		? uint64(currentId) // Hasn't been shuffled before.
		: getGobblerData[currentId].idx; // Shuffled before.
```

## [G-03] ARRAY LENGTH CAN BE CACHED OUTSIDE OF LOOP
Caching the array length outside of the loop and using the cached length in the loop costs less gas than reading the array length for each iteration. For example, `ids.length` in the following code can be cached outside of the loop like `uint256 idsLength = ids.length`, and `i < idsLength` can be used for each iteration.

```solidity
src\utils\GobblerReserve.sol
  37: for (uint256 i = 0; i < ids.length; i++) {
src\utils\token\GobblersERC1155B.sol
  114: for (uint256 i = 0; i < owners.length; ++i) {
```

## [G-04] VARIABLE DOES NOT NEED TO BE INITIALIZED TO ITS DEFAULT VALUE
Explicitly initializing a variable with its default value costs more gas than uninitializing it. For example, `uint256 i` can be used instead of `uint256 i = 0` in the following code.
```solidity
src\ArtGobblers.sol
  432: for (uint256 i = 0; i < cost; ++i) {
  592: for (uint256 i = 0; i < numGobblers; ++i) {

src\Pages.sol
  251: for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src\utils\GobblerReserve.sol
  37: for (uint256 i = 0; i < ids.length; i++) {

src\utils\token\GobblersERC721.sol
  186: for (uint256 i = 0; i < amount; ++i) {

src\utils\token\GobblersERC1155B.sol
  114: for (uint256 i = 0; i < owners.length; ++i) {
  173: for (uint256 i = 0; i < amount; ++i) {
```

## [G-05] ++VARIABLE CAN BE USED INSTEAD OF VARIABLE++
++variable costs less gas than variable++. For example, `i++` can be changed to `++i` in the following code.
```solidity
src\Pages.sol
  251: for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src\utils\GobblerReserve.sol
  37: for (uint256 i = 0; i < ids.length; i++) {
```

## [G-06] X = X + Y OR X = X - Y CAN BE USED INSTEAD OF X += Y OR X -= Y
x = x + y or x = x - y costs less gas than x += y or x -= y. For example, `getUserData[to].gobblersOwned += 1` can be changed to `getUserData[to].gobblersOwned = getUserData[to].gobblersOwned + 1` in the following code.

```solidity
lib\solmate\src\utils\SignedWadMath.sol
  203: r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
  205: r += 600920179829731861736702779321621459595472258049074101567377883020018308;

src\ArtGobblers.sol
  439: burnedMultipleTotal += getGobblerData[id].emissionMultiple;
  456: getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
  464: legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
  662: getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
  844: uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
  905: getUserData[from].emissionMultiple -= emissionMultiple;
  906: getUserData[from].gobblersOwned -= 1;
  912: getUserData[to].emissionMultiple += emissionMultiple;
  913: getUserData[to].gobblersOwned += 1;

src\Pages.sol
  244: uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);

src\utils\token\GobblersERC721.sol
  184: getUserData[to].gobblersOwned += uint32(amount);
```

## [G-07] REVERT WITH CUSTOM ERROR CAN BE USED INSTEAD OF REQUIRE() WITH REASON STRING
`revert` with custom error can cost less gas than `require()` with reason string. Please consider using `revert` with custom error to replace the following `require()`.

```solidity
lib\solmate\src\auth\Owned.sol
  20: require(msg.sender == owner, "UNAUTHORIZED");

lib\solmate\src\tokens\ERC721.sol
  36: require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
  40: require(owner != address(0), "ZERO_ADDRESS");
  69: require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
  87: require(from == _ownerOf[id], "WRONG_FROM");
  89: require(to != address(0), "INVALID_RECIPIENT");
  91: require(
  118: require(
  134: require(
  158: require(to != address(0), "INVALID_RECIPIENT");
  160: require(_ownerOf[id] == address(0), "ALREADY_MINTED");
  175: require(owner != address(0), "NOT_MINTED");
  196: require(
  211: require(

lib\solmate\src\utils\SignedWadMath.sol
  142: require(x > 0, "UNDEFINED");

lib\VRGDAs\src\VRGDA.sol
  32: require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");

src\ArtGobblers.sol
  437: require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
  885: require(from == getGobblerData[id].owner, "WRONG_FROM");
  887: require(to != address(0), "INVALID_RECIPIENT");
  889: require(

src\utils\token\GobblersERC721.sol
  62: require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
  66: require(owner != address(0), "ZERO_ADDRESS");
  95: require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
  121: require(
  137: require(

src\utils\token\GobblersERC1155B.sol
  107: require(owners.length == ids.length, "LENGTH_MISMATCH");
  149: require(
  185: require(

src\utils\token\PagesERC721.sol
  55: require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
  59: require(owner != address(0), "ZERO_ADDRESS");
  85: require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
  103: require(from == _ownerOf[id], "WRONG_FROM");
  105: require(to != address(0), "INVALID_RECIPIENT");
  107: require(
  135: require(
  151: require(
```