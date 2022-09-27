----

Findings: Immutable addresses lack zero-address check

Code:
	- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L314-L317
		○ Address: goo, pages, team, community

	- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83 (on address artGobblers)
	- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L84 (on address pages)

	- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L179 (on address goo)

	- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L181 (on address community)

	- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L23-L25 
		○ Address: artGobblers

Explanation: Constructors should check the address written in an immutable address variable is not the zero address

Mitigation:
	- Add a zero address check to above addresses

----

Findings: Public functions can be external

Code:
	- https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L21
	- https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L43

Explanation: It is good practice to mark functions as external instead of public if they are not called by the contract where they are defined.

Mitigation: Declare these functions as external instead of public

----

Findings: Floating Pragma

Code: 
	- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L2
(all contracts)

Explanation: Instead of >=0.8.0. It is recommended to lock the pragma version to the same version as used in the other contracts and also consider known bugs ( https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.

Mitigation: pragma solidity 0.8.17

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
