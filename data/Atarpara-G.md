
### Gas Optimazation

#### G-01 Branchless & Caching state variable in revealGobblers() 

The code can be optimized by minimising the number of SLOADs and Remove Branch.
Storage value should get cached in memory

File: ArtGobblers.sol [line615-625](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L615-625)


```
    uint64 swapIndex = getGobblerData[swapId].idx == 0 //@audit 1 SLOAD
                      ? uint64(swapId) // Hasn't been shuffled before.
                      : getGobblerData[swapId].idx; // @audit 2 SLOAD

    address currentIdOwner =getGobblerData[currentId].owner;
    
    uint64 currentIndex = getGobblerData[currentId].idx == 0 //@audit 1 SLOAD
    ?uint64(currentId) 
    : getGobblerData[currentId].idx; //@audit 2 SLOAD


```

The above should be modified to

```
uint64 swapIndex = getGobblerData[swapId].idx;
assembly{
    swapIndex := xor(swapIndex,mul(xor(swapIndex,and(0xffffffffffffffff,swapId)),iszero(swapIndex)))
        }
// Get the owner of the current id.
address currentIdOwner =getGobblerData[currentId].owner;

// Get the index of the current id.            
uint64 currentIndex =getGobblerData[currentId].idx;
assembly{
    currentIndex := xor(currentIndex,mul(xor(currentIndex,and(0xffffffffffffffff,currentId)),iszero(currentIndex)))
    }
```
Here Second Sload coverted into Mload and remove the unnecessary branches by using xor logic. 


#### G-02 Custom Error 

The Bytecode size and run time optimization by using custom errors

###### File: ArtGobblers.sol 
1. [Line-437](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L437)
```
/// @audit can be use UnauthorizedCaller(address caller) error which is already defined 
require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
```
2. [Line-696](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L696)
3. [Line-705](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L705)
4. [Line-711](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L711)
```
/// @audit can be new custom error NotMinted() and use in line 696,705 and 711

if (gobblerId == 0) revert("NOT_MINTED"); // line 696

if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED"); // line 705

revert("NOT_MINTED"); // line 711

```

5. [Line-885](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885)
6. [Line-887](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887)
7. [Line-889](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L889)
```
/// @audit here can be use OwnerMismatch(address owner) error which is already defind
require(from == getGobblerData[id].owner, "WRONG_FROM"); // line 885

/// @audit here can be defined new custom error InvalidRecipient()
require(to != address(0), "INVALID_RECIPIENT"); // line 887 

/// @audit can be use UnauthorizedCaller(address caller) error which is already defined 
require(msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
        "NOT_AUTHORIZED"); // line 889
```

File : Pages.sol [Line-266](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L266)
```
/// @audit Can be define new custom error NotMinted() 
if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```

#### G-03 \++i costs less gas compared to i++ or i += 1 in for loops 

File : Pages.sol [Line-251](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)

```
/// @audit convert i++ into ++i for tiny gas saving
for (uint256 i = 0; i < numPages; i++)   _mint(community, ++lastMintedPageId);
```

Flie : GobblerReserve.sol [Line-37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
```
 /// @audit convert i++ into ++i for tiny gas saving
 for (uint256 i = 0; i < ids.length; i++) 
```

#### G-04 Caching Array length into Memory  

File : GobblerReserve.sol [Line-37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)

```
/// @audit here ids.length can be cache into memory
for (uint256 i = 0; i < ids.length; i++) 
```


