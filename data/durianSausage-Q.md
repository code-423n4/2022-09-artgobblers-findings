### L01:_SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE
#### problem
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### prof
src/ArtGobblers.sol, 356, b'        _mint(msg.sender, gobblerId);'
src/ArtGobblers.sol, 389, b'        _mint(msg.sender, gobblerId);'
src/ArtGobblers.sol, 469, b'            _mint(msg.sender, gobblerId);'
lib/solmate/src/tokens/ERC721.sol, 194, b'        _mint(to, id);'
lib/solmate/src/tokens/ERC721.sol, 209, b'        _mint(to, id);'
src/Goo.sol, 102, b'        _mint(to, amount);'
src/Pages.sol, 211, b'            _mint(msg.sender, pageId);'
src/Pages.sol, 251, b'            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);'

### L02: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
src/ArtGobblers.sol, 314, b'        goo = _goo;'
src/ArtGobblers.sol, 315, b'        pages = _pages;'
src/ArtGobblers.sol, 316, b'        team = _team;'
src/ArtGobblers.sol, 317, b'        community = _community;'
src/ArtGobblers.sol, 318, b'        randProvider = _randProvider;'
src/ArtGobblers.sol, 564, b'        randProvider = newRandProvider; // Update the randomness provider.'
src/utils/rand/ChainlinkV1RandProvider.sol, 55, b'        artGobblers = _artGobblers;'
script/deploy/DeployBase.s.sol, 49, b'        teamColdWallet = _teamColdWallet;'
script/deploy/DeployBase.s.sol, 52, b'        vrfCoordinator = _vrfCoordinator;'
script/deploy/DeployBase.s.sol, 53, b'        linkToken = _linkToken;'
script/deploy/DeployBase.s.sol, 70, b'        teamReserve = new GobblerReserve(ArtGobblers(gobblerAddress), teamColdWallet);'
script/deploy/DeployBase.s.sol, 71, b'        communityReserve = new GobblerReserve(ArtGobblers(gobblerAddress), teamColdWallet);'
script/deploy/DeployBase.s.sol, 78, b'        randProvider = new ChainlinkV1RandProvider(\n            ArtGobblers(gobblerAddress),\n            vrfCoordinator,\n            linkToken,\n            chainlinkKeyHash,\n            chainlinkFee\n        );'
script/deploy/DeployBase.s.sol, 86, b'        goo = new Goo(\n            // Gobblers contract address:\n            gobblerAddress,\n            // Pages contract address:\n            pageAddress\n        );'
script/deploy/DeployBase.s.sol, 99, b'        artGobblers = new ArtGobblers(\n            merkleRoot,\n            mintStart,\n            goo,\n            Pages(pageAddress),\n            address(teamReserve),\n            address(communityReserve),\n            randProvider,\n            gobblerBaseUri,\n            gobblerUnrevealedUri\n        );'
script/deploy/DeployBase.s.sol, 102, b'        pages = new Pages(mintStart, goo, teamColdWallet, artGobblers, pagesBaseUri);'
src/utils/token/PagesERC721.sol, 43, b'        artGobblers = _artGobblers;'
src/utils/GobblerReserve.sol, 24, b'        artGobblers = _artGobblers;'
src/Goo.sol, 83, b'        artGobblers = _artGobblers;'
src/Goo.sol, 84, b'        pages = _pages;'
lib/solmate/src/auth/Owned.sol, 30, b'        owner = _owner;'
lib/solmate/src/auth/Owned.sol, 40, b'        owner = newOwner;'
src/Pages.sol, 179, b'        goo = _goo;'
src/Pages.sol, 181, b'        community = _community;'
src/utils/token/PagesERC721.sol, 43, b'        artGobblers = _artGobblers;'
src/utils/rand/ChainlinkV1RandProvider.sol, 55, b'        artGobblers = _artGobblers;'



## none-critical issue foundings
### N01: EVENT IS MISSING INDEXED FIELDS
#### prof
src/ArtGobblers.sol, 236, b'    event RandomnessFulfilled(uint256 randomness);'
src/utils/rand/RandProvider.sol, 13, b'    event RandomBytesRequested(bytes32 requestId);'
src/utils/rand/RandProvider.sol, 14, b'    event RandomBytesReturned(bytes32 requestId, uint256 randomness);'

### N02: NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES
src/ArtGobblers.sol, 2, b'pragma solidity >=0.8.0;'
src/utils/rand/ChainlinkV1RandProvider.sol, 2, b'pragma solidity >=0.8.0;'
script/deploy/DeployBase.s.sol, 2, b'pragma solidity >=0.8.0;'
script/deploy/DeployRinkeby.s.sol, 2, b'pragma solidity >=0.8.0;'