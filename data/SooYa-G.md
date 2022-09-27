## Loops Can be Optimized In Several Ways
To optimize this loop and make it consume less gas, we can do the folowing things:

1. Cache the length of arrays in loops. if it is a storage array, this is an extra sload operation (100 additional extra gas (EIP-2929 2) for each iteration except for the first), if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first), if it is a calldata array, this is an extra calldataload operation (3 additional gas for each iteration except for the first)

2. There’s no need to initialize i to its default value, it will be done automatically and it will consume more gas if it will be done (I know, sounds stupid, but trust me - it works).

So after applying all these changes, the loop will look something like this:

```
for (uint256 i; i < Variable; ++i) {
```

I suggest to change the code here:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114

## There’s No Need To Initialize `i` To Its Default Value
There’s no need to initialize i to its default value, it will be done automatically and it will consume more gas if it will be done (I know, sounds stupid, but trust me - it works). 

``` for (i;i < variable; ++i){ ```

I suggest to change the code here:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592

## `X += Y` Costs More Gas Than `X = X + Y` For State Variables
Change each operation that using += to `X = X + Y` for state variables

There are 3 instances for this issues:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L184


https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L439


https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L456

