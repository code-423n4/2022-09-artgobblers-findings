## [Low-01] mintStart is not limited

In the constructors of ArtGobblers and Pages contracts, there is no limit on mintStart, and when mintStart is too small (like 0), the price of NFT will be very low.
So consider making mintStart >= block.timestamp

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L310-L311
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L177-L178
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L219-L229
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L396-L402


## [Low-02] Unable to call gobble on non-ERC721 NFTs

In the gobble function of the ArtGobblers contract, it is not possible to send non-ERC721 (like cryptopunks) NFTs to the contract, adding support for non-ERC721 NFTs would benefit the project ecosystem.

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L746-L748


## [Low-03]  _safeMint() should be used rather than _mint() wherever possible

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver.


https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L356-L357
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L389-L390
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L469-L470
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L211-L212


## [Low-04] GobblerReserve: to is unchecked in withdraw(), which can cause user's NFT to be frozen

The address to will receive the NFT when withdraw() is called. However, if to is a contract address that does not support ERC721, the NFT can be frozen in the contract.

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L38-L39

## [Low-05]  Users with the isApprovedForAll and getApproved permissions should also be able to call the gobble function

Currently only the owner of a Gobbler can call the gobble function on that Gobbler, but users with the isApprovedForAll and getApproved permissions should also be able to call the gobble function on that Gobbler

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L723-L733
