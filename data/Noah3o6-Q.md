-> EVENT IS MISSING INDEXED FIELDS
Each event should use three indexed fields if there are three or more fields.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=event%20GobblerPurchased(address%20indexed%20user%2C%20uint256%20indexed%20gobblerId%2C%20uint256%20price)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=event%20LegendaryGobblerMinted(address%20indexed%20user%2C%20uint256%20indexed%20gobblerId%2C%20uint256%5B%5D%20burnedGobblerIds)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=event%20ReservedGobblersMinted(address%20indexed%20user%2C%20uint256%20lastMintedGobblerId%2C%20uint256%20numGobblersEach)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=event%20GobblersRevealed(address%20indexed%20user%2C%20uint256%20numGobblers%2C%20uint256%20lastRevealedId)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=event%20ArtGobbled(address%20indexed%20user%2C%20uint256%20indexed%20gobblerId%2C%20address%20indexed%20nft%2C%20uint256%20id)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#:~:text=event%20TransferSingle(,)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#:~:text=event%20TransferBatch(,)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#:~:text=event%20ApprovalForAll(address%20indexed%20owner%2C%20address%20indexed%20operator%2C%20bool%20approved)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#:~:text=event%20ApprovalForAll(address%20indexed%20owner%2C%20address%20indexed%20operator%2C%20bool%20approved)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#:~:text=event%20ApprovalForAll(address%20indexed%20owner%2C%20address%20indexed%20operator%2C%20bool%20approved)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#:~:text=event%20PagePurchased(address%20indexed%20user%2C%20uint256%20indexed%20pageId%2C%20uint256%20price)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#:~:text=event%20CommunityPagesMinted(address%20indexed%20user%2C%20uint256%20lastMintedPageId%2C%20uint256%20numPages)%3B


-> _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=%3D%20%2B%2BcurrentNonLegendaryId)%3B-,%7D,%7D,-/*//////////////////////////////////////////////////////////////
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=gobblerId%2C%20gobblerIds%5B%3Acost%5D)%3B-,_mint,-(msg.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#:~:text=%2B%2BcurrentId%2C%20currentPrice)%3B-,_mint,-(msg.
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#:~:text=only(artGobblers)%20%7B-,_mint,-(to%2C%20amount)%3B


->USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=pragma%20solidity%20%3E%3D0.8.0%3B
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#:~:text=pragma%20solidity%20%3E%3D0.8.0%3B
