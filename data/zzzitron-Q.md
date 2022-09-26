## Summary

Risk | title
---|---
L00 | lacking zero address check
N00 | outdated/mismatching description from the README

## Low

### L00: lacking zero address check

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L287
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L156

Constructors of `GobblerReserve`, `ArtGobblers`, `Pages` do not check the owner address, the team address or community address for the zero address. Given that some gobblers or pages will be minted to those addresses, one may add some safeguard for that.

## Non critical

### N00: outdated/mismatching description from the README

from the readme/whitepaper:

> Art Gobblers themselves are fully animated ERC1155 NFTs.

They are ERC721 NFTs.

>  new Legendary Gobbler appears each time an additional 10% of the total supply of Gobblers is issued via VRGDA, and each Legendary Auction is scheduled to end by the time the next Legendary Gobbler appears (or when all Gobblers are issued, in the case of the final Legendary)

There will be more gobblers minted after the final Legendary (for the price calculation, presumably).





<!-- zzzitron QA -->

