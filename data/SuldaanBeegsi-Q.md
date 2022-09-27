First issue:

Use OpenZeppelin - ArtGobblers.sol

The lines 62-68 are import lines were the developers have imported Owned.sol, ERC721.sol etc. These contracts can be also found in the OpenZeppelin library, which is the industry standard. When thinking of smart contract developing, we have to have longevity in mind. OpenZeppelin is a large organization with multiple years of experience in developing a open source library. I am fan of the developer of soulmate, but longevity in mind it is best for the projects lifespan to use OpenZeppelin library.

Second issue:

The use of Solmate header folding brakes when using vscode as the editor. 

Third issue: line 844 - ArtGobblers.sol

uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);

The left shift could be replaced by mul.

P.S thanks for the free C4



