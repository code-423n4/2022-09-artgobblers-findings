-> X = X + Y IS CHEAPER THAN X += Y (same for X = X - Y IS CHEAPER THAN X -= Y)

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=burnedMultipleTotal%20%2B%3D%20getGobblerData%5Bid%5D.emissionMultiple%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=getUserData%5Bmsg.sender%5D.emissionMultiple%20%2B%3D%20uint32(burnedMultipleTotal)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=legendaryGobblerAuctionData.numSold%20%2B%3D%201%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=getUserData%5BcurrentIdOwner%5D.emissionMultiple%20%2B%3D%20uint32(newCurrentIdMultiple)%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=getUserData%5Bfrom%5D.gobblersOwned%20%2D%3D%201%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=getUserData%5Bto%5D.emissionMultiple%20%2B%3D%20emissionMultiple%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=getUserData%5Bto%5D.gobblersOwned%20%2B%3D%201%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=getUserData%5Bfrom%5D.emissionMultiple%20%2D%3D%20emissionMultiple%3B


->STATE VARIABLES ONLY SET IN THE CONSTRUCTOR SHOULD BE DECLARED IMMUTABLE

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas)

https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#:~:text=address%20public%20immutable%20coldWallet%20%3D%200x126620598A797e6D9d2C280b5dB91b46F27A8330%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#:~:text=address%20public%20immutable%20root%20%3D%200x1D18077167c1177253555e45B4b5448B11E30b4b%3B
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#:~:text=uint256%20public%20immutable%20mintStart%20%3D%201656369768%3B

-> ++i costs less gas compared to i++ or i += 1 (Also --i costs less gas compared to i--- or i -= 1)

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#:~:text=unchecked%20%7B-,_balanceOf%5Bfrom%5D%2D%2D%3B,-_balanceOf%5Bto%5D
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#:~:text=from%5D%2D%2D%3B-,_balanceOf%5Bto%5D%2B%2B%3B,-%7D
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#:~:text=i%20%3C%20numPages%3B-,i%2B%2B,-)%20_mint(community
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#:~:text=ids.length%3B-,i%2B%2B,-)%20%7B

->USING BOOLS FOR STORAGE INCURS OVERHEAD

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#:~:text=mapping(address%20%3D%3E%20bool)
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#:~:text=mapping(address%20%3D%3E%20bool)
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#:~:text=mapping(address%20%3D%3E%20bool)
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#:~:text=mapping(address%20%3D%3E%20bool)


