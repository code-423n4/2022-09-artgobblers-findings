## RandomBytesRequested Event Always Emits Zero – [ChainlinkV1RandProvider.sol #L66](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L66)
`requestId` is declared as the return value of this function however usage in emitting an event prior to assignment means that it is never non-zero at the time of emitting the event. Instead consider: ```emit RandomBytesRequested(requestId = requestRandomness(chainlinkKeyHash, chainlinkFee));``` combining with [#L69](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L69).

## Incorrect Legendary Gobbler Auction Start Price Type – [ArtGobblers.sol #L461](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L461)
The start price type should be `uint128` to be consistent with other usage.

## Explicit Uint Type – [ArtGobblers.sol #L763](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L763)
Use the explicit `uint256` type, as in some circumstances (e.g. abi-encoding with signature) they are not synonymous.

## Consider Extending Withdraw Functionality – [GobblerReserve.sol #L34](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L34)
It may be wise to make this function internal and then have a single generalised public `onlyOwner` function which is capable of executing arbitrary abi-encoded calls e.g. to call the proposed internal withdraw functinon or transfer other assets which may be sent to the contract GobblerReserve contract address.

## Re-Write Left Shift As Exp – [ArtGobblers.sol #L674](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L674)
Here `shl(64, 1)` is equivalent to `exp(2, 64)` so re-write for readability.

## Re-Write Left Shift As Exp – [ArtGobblers.sol #L844](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L844)
`numGobblersEach << 1` is equivalent to `numGobblersEach * 2` so re-write for readability.