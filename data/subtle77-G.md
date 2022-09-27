1 :
++I COSTS LESS GAS COMPARED TO I++ OR I += 1 IN FOR LOOPS (~5 GAS PER ITERATION)
++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

- src/utils/GobblerReserve.sol
37     for (uint256 i = 0; i < ids.length; i++) {
 38                artGobblers.transferFrom(address(this), to, ids[i]);

- /src/Pages.sol (numPages; i++)
251 for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

2:
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

- src/utils/GobblerReserve.sol
37  for (uint256 i = 0; i < ids.length; i++) {

-src/utils/token/GobblersERC1155B.sol
114 for (uint256 i = 0; i < owners.length; ++i)

3:Cache storage values in memory to minimize SLOADs

The code can be optimized by minimising the number of SLOADs. SLOADs are expensive 100 gas compared to MLOADs/MSTOREs(3gas)
Storage value should get cached in memory

-/blob/main/src/ArtGobblers.sol (cache LEGENDARY_GOBBLER_INITIAL_START_PRICE)
462    cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2

-/blob/main/src/ArtGobblers.sol (cache FIRST_LEGENDARY_GOBBLER_ID)
705 if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");
708      if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold)

-blob/main/src/ArtGobblers.sol (cache LEGENDARY_AUCTION_INTERVAL)
487     uint256 numMintedAtStart = (numSold + 1) * LEGENDARY_AUCTION_INTERVAL;
488
489           // If not enough gobblers have been minted to start the auction yet, return how many need to be minted.
490            if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);
491
492            // Compute how many gobblers were minted since the auction began.
493           uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
494
495           // prettier-ignore
496          // If we've minted the full interval or beyond it, the price has decayed to 0.
497           if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;
498           // Otherwise decay the price linearly based on what fraction of the interval has been minted.
499           else return FixedPointMathLib.unsafeDivUp(startPrice * (LEGENDARY_AUCTION_INTERVAL - numMintedSinceStart), LEGENDARY_AUCTION_INTERVAL);

-





