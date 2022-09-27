## Optimize Transfer Event (_mint) – [GobblersERC721.sol #L169-L171](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L169-L171)
Combine event emission with assignment to save some gas: ```emit Transfer(address(0), getGobblerData[id].owner = to, id);```

## Optimize Transfer Event (_batchMint) – [GobblersERC721.sol #L187-189](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L187-L189)
Combine event emission with assignment to save some gas: ```emit Transfer(address(0), getGobblerData[++lastMintedId].owner = to, lastMintedId);```

## Optimize Struct Write – [ArtGobblers.sol #411-L471](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L411-L471)
Optimize the happy path by loading the `legendaryGobblerAuctionData` struct into memory for more efficient reads/writes, then write back storage after #L464.

## Optimize Transfer Event (_batchMint) – [GobblersERC721.sol #L187-189](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L187-L189)
Combine event emission with assignment to save some gas: ```emit Transfer(address(0), getGobblerData[++lastMintedId].owner = to, lastMintedId);```

## Prefer ++i over i++ – [Pages.sol #L251 & GobblerReserve.sol #L37]
Pre-increment loop counters to save some gas.