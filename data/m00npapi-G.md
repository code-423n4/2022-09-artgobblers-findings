# Gas Optimization - using mul instead of left shift removing unnecessary optimization

#### Location - https://github.com/artgobblers/art-gobblers/blob/c821b6d48f836ccf5d152c75584bb9792bba774f/src/ArtGobblers.sol#L844


#### uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1); 


#### Unnecessary optimization, since nothing is gained here should use mul for better readability 

#### Per comments (numGobblersEach << 1) is equal to numGobblersEach * 2)

###### s.o ttransmissions11 throwing the easy bone in discord