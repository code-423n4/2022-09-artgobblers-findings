# Gas Optimization Report
## Note On Methodology
The gas snapshot file didn't seem to be up to date with the original tests so `forge snapshot` was run before any optimization was tested to ensure the snapshot was up-to-date.

Each of the following optimizations have been implemented and tested individually. The gas savings represent the total "overall gas change" displayed after
running `forge snapshot --diff .gas-snapshot`. 

## Optimizations
### [G-1] Bit Shifting Rather Than Base-2 Division / Multiplication
**Total Gas Improvement:** 128
**Files:** [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#462)
**Optimization:** Replace `/ 2` with `>> 1` and `* 2` with `<< 1` respectively.

### [G-2] Use Local Value Rather Than Storage Re-Read
**Total Gas Improvement:** 3224
**Files:** [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L493)
**Optimization:** Replace `numMintedFromGoo` with `mintedFromGoo` on L493. This will ensure that the existing local copy of the value is used instead of re-reading from storage.
### [G-3] Improve Swap Indices Retrieval
**Total Gas Improvement:** 87023
**Files:** `src/ArtGobblers.sol` [L615](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L615),[L623](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L623),
**Optimization:** Replace lines L615-625 with the following code avoiding the repeated storage load and make the ternary branchless:
```solidity
// Get the index of the swap id.
uint64 storedSwapIndex = getGobblerData[swapId].idx;
uint64 swapIndex;
assembly {
	swapIndex := or(storedSwapIndex, mul(iszero(storedSwapIndex), swapId))
}

// Get the owner of the current id.
address currentIdOwner = getGobblerData[currentId].owner;

// Get the index of the current id.
uint64 storedCurrentIndex = getGobblerData[currentId].idx;
uint64 currentIndex;
assembly {
	currentIndex := or(storedCurrentIndex, mul(iszero(storedCurrentIndex), currentId))
}
```

### [G-4] Remove Unused Method Parameter
**Total Gas Improvement:** 780967 (Note that some tests seem to fair slightly worse with this optimization)
**Files:** [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L546), [`src/utils/rand/ChainlinkV1RandProvider.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L76),
**Optimization:** Since the `acceptRandomSeed` method does nothing with `bytes32 requestId` it does not have to receive it. It can therefore be removed as the random provider is a custom adapter anyway.