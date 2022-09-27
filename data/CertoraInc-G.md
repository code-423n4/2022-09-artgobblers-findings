# Gas Optimizations 
* Variables like `UNREVEALED_URI` and `BASE_URI`, which can't be changed, can be marked as immutable to save gas when accessing them.
* The `mapping(uint256 => mapping(address => mapping(uint256 => uint256))) public getCopiesOfArtGobbledByGobbler;` mapping can be changed to simplify it. We know the gobbler id will be maximum 10000, so it can fit in a `uint96`. That means that instead of mapping a gobbler id to an address of an nft, we can group them together and create the new `mapping(uint256 => mapping(uint256 => uint256))) public getCopiesOfArtGobbledByGobbler;`, where the first uint256 is the gobbler id and the nft contract's address grouped (`(gobblerId << 160) | nft`).
* Calculate `toWadUnsafe(MAX_MINTABLE)` in the `ArtGobblers` contract pre deployment so it will save gas on deployment.
* Cache storage variables to reduce the number of SLOADs
  * Cache `legendaryGobblerAuctionData.numSold`, `getGobblerData[id]` in the `ArtGobblers.mintLegendaryGobbler`
  * Use the cached value in `ArtGobblers.legendaryGobblerPrice` instead of accessing `numMintedFromGoo` again (`uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;` => `uint256 numMintedSinceStart = mintedFromGoo - numMintedAtStart;`)
  * Cache `getGobblerData[swapId].idx` and `getGobblerData[currentId].idx` in the `ArtGobblers.revealGobblers` function. The optimized code:
    ```sol
    uint idx = getGobblerData[swapId].idx;
    uint64 swapIndex = idx == 0
        ? uint64(swapId) // Hasn't been shuffled before.
        : idx; // Shuffled before.

    // Get the owner of the current id.
    address currentIdOwner = getGobblerData[currentId].owner;

    // Get the index of the current id.
    idx = getGobblerData[currentId].idx;
    uint64 currentIndex = idx == 0
        ? uint64(currentId) // Hasn't been shuffled before.
        : idx; // Shuffled before.
    ```
* Modify the expression `legendaryGobblerAuctionData.startPrice = uint120(cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2);` in the `ArtGobblers.mintLegendaryGobbler` function by:
    1. Changing `cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2` to `2 * cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE`
    2. Changing `2 * cost` to `cost << 1`
    3. Cache the value of `cost << 1` instead of calculating it twice
    The final expression:
    ```sol
    uint doubleCost = cost << 1;
    legendaryGobblerAuctionData.startPrice = uint120(doubleCost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : doubleCost);
    ```
* Use shifting instead of multiplying by powers of 2 in the `ArtGobblers.mintLegendaryGobbler` function (`getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2)` => `getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal << 1)`)
* Use prefix incrementation and decrementation when incrementing or decrementing variables by one:
  * [line 464](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L464) of the `ArtGobblers.mintLegendaryGobbler` function - `legendaryGobblerAuctionData.numSold += 1` => `++legendaryGobblerAuctionData.numSold`
  * [line 906](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L906) - `getUserData[from].gobblersOwned -= 1;` => `--getUserData[from].gobblersOwned;`
  * [line 913](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L913) - `getUserData[to].gobblersOwned += 1;` => `++getUserData[to].gobblersOwned;`
* Create a constant for the last non-legendary gobbler id instead of calculating `FIRST_LEGENDARY_GOBBLER_ID - 1` in every iteration of the loop in `ArtGobblers.revealGobblers`
* Use `& (2 ** 64 - 1)` instead of `% 2 ** 64`. (`randomSeed := mod(keccak256(0, 32), shl(64, 1))` => `randomSeed := and(keccak256(0, 32), 0xffffffffffffffff)`)
* Optimize [this condition](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L848) using the following steps:
    1. `newNumMintedForReserves > (numMintedFromGoo + newNumMintedForReserves) / 5`
    2. `5 * newNumMintedForReserves > numMintedFromGoo + newNumMintedForReserves`
    3. `4 * newNumMintedForReserves > numMintedFromGoo`
    4. `(newNumMintedForReserves << 2) > numMintedFromGoo`
