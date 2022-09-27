--- 

 ## `_safemint()` should be used rather than `_mint()` wherever possible 

 ### Files Found: 

 There are 6 instances of this issue. 

- File: src/ArtGobblers.sol - line 356 

- File: src/ArtGobblers.sol - line 389 

- File: src/ArtGobblers.sol - line 469 

- File: src/Goo.sol - line 102 

- File: src/Pages.sol - line 211 

- File: src/Pages.sol - line 251 

 ### Mitigation: 

 `_mint()` is discouraged in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. 

 --- 

 --- 

 ## `Withdrawerc721`: using the `transferFrom()` function of an erc721 contract may freeze the user's nft 

 ### Files Found: 

 There are 2 instances of this issue. 

- File: src/ArtGobblers.sol - line 749 

- File: src/ArtGobblers.sol - line 881 


 ### Mitigation: 

 When using the transferFrom function of an ERC721 contract to send an NFT, if the receiving address is a smart contract and does not support ERC721, the NFT can be frozen in the contract. Use the ERC721 contract's safeTransferFrom function to send NFTs. 

 --- 
