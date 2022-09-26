## Zero address check missing

Contract:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L55

Issue:
In ownerOf function, there is no check to see if id is valid which result in returning owner being address zero

Recommendation:
Change it to require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

## Revise the approve function

Contract:
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L92

Issue:
In approve function, the already approved user should also be allowed to call this function to change spender

Recommendation:
Add below condition getApproved[id]==msg.sender . Same check goes for PagesERC721.sol

```
require(msg.sender == owner || isApprovedForAll[owner][msg.sender] || getApproved[id]==msg.sender, "NOT_AUTHORIZED");
```