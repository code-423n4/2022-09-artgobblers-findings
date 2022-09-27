# Summary

We list 2 low-critical findings:
* (Low) It issues a legendary gobbler whenever 1/11 of gobblers are minted
* (Low) Users can feed ERC20 tokens to gobbler through ERC721 `transferFrom`

# (Low) It issues a legendary gobbler whenever 1/11 of gobblers are minted

## Impact

It issues a legendary gobbler whenever 1/11 of gobblers are minted, while spec specifies 10%.

## Proof of Concept

In `legendaryGobblerPrice`, it check that the number of gobblers minted at the start of the auction which is computed by multiplying the # of intervals:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L487

```
            // The number of gobblers minted at the start of the auction is computed by multiplying the # of
            // intervals that must pass before the next auction begins by the number of gobblers in each interval.
            uint256 numMintedAtStart = (numSold + 1) * LEGENDARY_AUCTION_INTERVAL;
```

The spec specifies 10%, but it issues a legendary gobbler whenever 1/11 of gobblers are minted:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L184

```
    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```

## Tools Used

Manual Review

## Recommended Mitigation Steps

Change L184 to:

```
    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / LEGENDARY_SUPPLY;
```

# (Low) Users can feed ERC20 tokens to gobbler through ERC721 `transferFrom`

## Impact

Users can feed ERC20 tokens to gobbler by `gobble()`. This behaviour may confuse the value of gobblers.

## Proof of Concept

Because the `transferFrom` of ERC20 is the same as `transferFrom` of ERC721, users can call `gobble()` and set `nft` address to any ERC20 addresses.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L748

## Tools Used

Manual Review

## Recommended Mitigation Steps

Check `ERC721(nft).supportsInterface(bytes4 interfaceId)` should have `type(IERC721).interfaceId`.

