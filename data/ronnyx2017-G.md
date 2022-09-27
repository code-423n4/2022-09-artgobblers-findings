# 1. Should use local variables on the stack

Lines of code:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L482-L499

Use the local variable `mintedFromGoo` on the stack instead of the global var `numMintedFromGoo` in the storage to save gas.

# 2. Mint cost for legendary Gobblers should be less than 1100

Lines of code:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L461-L464

The cost for minting a legendary gobbler should set a soft cap about 1100. At the starting of the fifth legendary gobbler auction, the max cost for minting will be about 1100 that will run out of the max gas limit. And if the cost continues to increase for the next auction, the auction will be delayed significantly, as no one can call the mint function successfully until the cost decays below 1100.

# 3. Using Keccak256 to provide pseudo-randomness is wasteful in the current situation

Lines of code:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L670-L675

Although it uses the Keccak256 to get multiple random numbers in the chainlink docs about VRF best practices. Its wasteful in the current situation that using a 256 bits seed to generate fewer than 2000 random numbers at one time.

Its a good idea to use LCG, which only consumes 13 gas at one time. It will save 23 gas every time you update the seed. And the number distribution is sufficiently uniform in this range.
