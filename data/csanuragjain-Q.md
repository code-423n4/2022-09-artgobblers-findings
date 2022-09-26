## Zero address check missing

Contract:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55

Issue:
In ownerOf function, invalid id will result in returning zero address owner

Recommendation:
Change it to require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
___

## Approval check missing

Contract:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L92

Issue:
In approve function, approved user wont be allowed to call the approve function which is incorrect

Recommendation:
Add getApproved[id]==msg.sender to the list of allowed user. Same goes for PagesERC721.sol