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

## 2 step owner change

Contract:
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L40

Issue:
In setOwner function, an incorrectly passed argument address will be set as admin of contract which will prohibit all function calls which are admin only. This can impact GobblerReserve (withdraw function), ArtGobblers (upgradeRandProvider)

Recommendation:
Kindly change this address change to a 2 step process (Pending admin, confirm admin) and also add checks for 0 address