# Quality Assurance Report

## Executive Summary
Codebase looks mature and well thought-out.
Documentation is complete and testing coverage is high.
The codebase could benefit with a few small adjustments to further simplify the codebase.

## Issues 


|   |      Issue     |  Instances |
|----------|-------------|:------:|
| 1 |  Left shift could be replaced by mul | 1 |
| 2  |    Minor oversight - Internal Function is named as if it was public Function is `internal` not prefixed with `_`    |   1 |
| 3  |  Missing checks for address(0x0) when assigning values to address state variables  |   3 |
| 4  |  Inconsistent use of inline delete  |  1 |

### 1. Left shift could be replaced by mul
Quoting `transmissions11` here https://github.com/artgobblers/art-gobblers/issues/145
the left shift doesnt save any gas and just hurts readability

### POC 

```solidity
 uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1); 
```

https://github.com/artgobblers/art-gobblers/blob/c821b6d48f836ccf5d152c75584bb9792bba774f/src/ArtGobblers.sol#L844


### 2. Minor oversight - Internal Function is named as if it was public Function is `internal` not prefixed with `_`

Function getNextValidator() is `internal` but not prefixed with `_`
### POC 

```solidity
    function updateUserGooBalance(
        address user,
        uint256 gooAmount,
        GooBalanceUpdateType updateType
    ) internal {
```
### Instances
```
2022-09-artgobblers/src/ArtGobblers.sol:813:5
```

### 3. Missing checks for address(0x0) when assigning values to address state variables 

### Summary 
Zero address should be checked for state variables and some parameters in functions like mints, withdrawals... A zero address can lead into problems.

### POC
 ```solidity  
     constructor(
        // Mint config:
        bytes32 _merkleRoot,
        uint256 _mintStart,
        // Addresses:
        Goo _goo,
        Pages _pages,
        address _team,
        address _community,
        RandProvider _randProvider,
        // URIs:
        string memory _baseUri,
        string memory _unrevealedUri
    )
        GobblersERC721("Art Gobblers", "GOBBLER")
        Owned(msg.sender)
        LogisticVRGDA(
            69.42e18, // Target price.
            0.31e18, // Price decay percent.
            // Max gobblers mintable via VRGDA.
            toWadUnsafe(MAX_MINTABLE),
            0.0023e18 // Time scale.
        )
    {
        mintStart = _mintStart;
        merkleRoot = _merkleRoot;

        goo = _goo;
        pages = _pages;
        team = _team;
        community = _community;
 ```
### Mitigation
Check zero address for state variables before assigning to them a value

### Instances
```
2022-09-artgobblers/src/ArtGobblers.sol:316:9
2022-09-artgobblers/src/ArtGobblers.sol:317:9
2022-09-artgobblers/src/Goo.sol:83:9
```

### 4.  Inconsistent use of inline delete
Gas costs are the same. More readable Less complex Less likely for solidity to have issues with it since it’s less esoteric. Deletion of field inside and outside the event have no difference in gas costs. 

### Reference 
https://github.com/artgobblers/art-gobblers/issues/149

### POC 
```solidity  
emit Transfer(msg.sender, getGobblerData[id].owner = address(0), id);
```
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L441

