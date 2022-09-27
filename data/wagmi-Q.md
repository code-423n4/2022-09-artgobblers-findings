# Summary

| Id | Title |
| -- | ----- |
| 1 | TokenURI should revert for burned gobblers |
| 2 | Art in Gobbler is lost after using to mint legendary gobbler |

## 1. TokenURI should revert for burned gobblers

In `ArtGobblers.tokenURI()` function, it checks for various cases but still return values for burned gobblers.

Consider revert or return empty string in `tokenURI()` in case gobbler is burned .


### Affected Codes
- https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L693

## 2. Art in Gobbler is lost after using to mint legendary gobbler

Arts that is fed to a standard gobbler through `gobble()` is lost when this gobbler is used to mint legendary gobbler.

In case legendary gobbler is minted, these arts should also be transferred to this new gobler. If art is a valuable NFT (CryptoPunk), this is a huge loss.

Consider adding a mechanism to allow users move arts from standard gobblers to new legendary gobbler gradually.

### Affected Codes
- https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L411
