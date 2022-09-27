The codebase is an very well optimized and secured one that follows general good practices. Nevertheless some low / informational issues can still be improved.

Low :
1- Race condition when minting legendary Gobbler

Dutch auctions have a problem in the blockchain. Since frontrunning exists if 2 users want to buy at a determined price the winner will be the highest gas payer or the one with more validation power. In a regular auction, the address that wants to win it the most could pay more. In a dutch auction, there is a race condition: the first tx to be included will win the auction. So if multiple users want to buy the auction at the start price the winner will be decided solely on gas paid and validation power. 

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L411

2- Not using safeMint
If the recipient is a contract, safeMint() checks whether they can handle ERC721 tokens which prevents from unwanted NFT's being bricked inside a contract. https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L356

This is correctly implemented in the contract's safeTransferFrom function but not implemented when minting.


Informational :

1- The gooble function only allows the owner to feed the gobble. The gobble() function should support feeding by approved non-owner addresses. These addresses already have the power to do it but in the current implementation, they would have to transfer it to themselves first. 

Instead of  owner!=msg.sender it should be  msg.sender == owner|| isApprovedForAll[owner][msg.sender] || msg.sender == getApproved[id],

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L733 

2- Lack of input checks 
The constructor should check _mintStart isn't in the past and critical addresses are not addresses(0)

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L311

3- Using an unset pragma. 

Files use pragma solidity >=0.8.0. This should be changed to a fixed pragma to ensure the compiler will run in the same way if the contracts are deployed on a newer solidity version

4- It's a good practice to declare enums and structs at the top of the contract before functions 

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L804



