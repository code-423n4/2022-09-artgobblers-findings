# Gas Report

## G1: Indexed bitmap can be used to check if a mintlist is claimed instead of using a address to bool mapping

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L149

Currently whether someone has claimed their mintlist is stored in an address to bool mapping. This is inefficient as it requires zero to non-zero SSTORE any time someone claims their airdrop. It is instead possible to store 256 addresses' mintlist state in a single storage space by using bitmap. With that, most claims would do a non-zero to non-zero SSTORE, significantly reducing costs. The efficiency of this method depends on the ratio of `MINTLIST_SUPPLY / NUMBER_OF_MINTLISTED_ADDRESSES`. Closer this value to 1, more efficient it will be. If you are going to have few thousand addresses in the mintlist, I recommend this method. But if you are going to have hundreds of thousands of addresses, then it might not worth the trouble. You can refer to Uniswap's implementation for further info: https://github.com/Uniswap/merkle-distributor/blob/master/contracts/MerkleDistributor.sol

## G2: Storage variable instead of the cached version is used

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L493

In `legendaryGobblerPrice()` function `numMintedFromGoo` is cached to memory as `mintedFromGoo`. However, later in the function, `numMintedFromGoo` is read instead of the local equivalent. Consider replacing that instance with `mintedFromGoo` to avoid an expensive SLOAD.