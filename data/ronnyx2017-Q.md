# Low Risk Issues

## 1. Request id is uninitialized when RandomBytesRequested event emitted

Lines of code:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L62-L69

It's always zero in the event log because the RequestId is uninitialized.
```
    ├─ [58991] ArtGobblers::requestRandomSeed()
    │   ├─ emit RandomnessRequested(user: RandProviderTest: [0x62d69f6867a0a084c6d313943dc22023bc263691], toBeRevealed: 1)
    │   ├─ [46413] ChainlinkV1RandProvider::requestRandomBytes()
    │   │   ├─ emit RandomBytesRequested(requestId: 0x0000000000000000000000000000000000000000000000000000000000000000)  <--- here
```

## 2. The result of price functions that calculate users spend should always be rounded up

Lines of code:

https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L46-L51

It returns the result rounded down. 

The result of price functions that calculate users spend should always be rounded up

# Non-critical Issues

It is a bad practice to name local variables and global variables the same.

address `owner` in the gobble function. 

Lines of code:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L730-L733

