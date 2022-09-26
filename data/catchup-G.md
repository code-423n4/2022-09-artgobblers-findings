

## G01 Saving SLOAD (~100 gas)

```ArtGobblers.sol```
- replace ```numMintedFromGoo``` with cached ```mintedFromGoo```. Save 1 SLOAD.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L493


## G02 Change 2 bool state variables to uint8 (each ~20K gas, ~40K total)
```    
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27

Consider changing the bool's below to uint and use values ```1``` and ```2``` rather than ```true``` and ```false```.
This would save an extra SLOAD(100), and an additional Gsset(20K) when changing from ```false` to ```true```.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L149
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L212


## G03 postfix increments can be changed to prefix for the below 2 for loops

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
37:             for (uint256 i = 0; i < ids.length; i++) {						//@audit gas ++i

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
251:             for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);		//@audit gas ++i


