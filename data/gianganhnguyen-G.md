# 1. [G-1]  ++number  cost less gas compare number++ or number += 1

File GobblerReserve.sol, line 37:

    for (uint256 i = 0; i < ids.length; i++) {

Becomes:

    for (uint256 i = 0; i < ids.length; ++i) {

File Pages.sol, line 251:

    for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

Becomes:

    for (uint256 i = 0; i < numPages; ++i) _mint(community, ++lastMintedPageId);

File ArtGobblers.sol, line 464:

    legendaryGobblerAuctionData.numSold += 1;

Becomes:

    ++legendaryGobblerAuctionData.numSold;

File ArtGobblers.sol, line 906:

    getUserData[from].gobblersOwned -= 1;

Becomes:

    --getUserData[from].gobblersOwned;

File ArtGobblers.sol, line 913:

    getUserData[to].gobblersOwned += 1;

Becomes:

    ++getUserData[to].gobblersOwned;

##### Instances include:

 File PagesERC721.sol, line 115, 117.

# 2. [G-2] Initialize variable without assign equal 0  is cheaper than initialize with assign equal 0

File ArtGobblers.sol, line 432:

    for (uint256 i = 0; i < cost; ++i) {

Becomes:

    for (uint256 i; i < cost; ++i) {

##### Instances include:

 File ArtGobblers.sol, line 592.
 File Pages.sol, line 251.
 File GobblerReserve.sol, line 37.
 File GobblersERC721.sol, line 186.
 File GobblersERC1155B.sol, line 114, 173.

# 3. [G-3] Cache the length of arrays in the loops for saving gas

File GobblerReserve.sol, line 37:

    for (uint256 i = 0; i < ids.length; i++) {

Becomes:

    uint256 length = ids.length;
    for (uint256 i; i < length; ++i) {

File GobblersERC1155B.sol, line 114:

    for (uint256 i = 0; i < owners.length; ++i) {

Becomes:

    uint256 length = owners.length;
    for (uint256 i; i < length; ++i) {

# 4. [G-4]  <x> += <y> cost more gas than <x> = <x> + <y>

 File ArtGobblers.sol, line 439:

    burnedMultipleTotal += getGobblerData[id].emissionMultiple;

Becomes:

    burnedMultipleTotal = burnedMultipleTotal + getGobblerData[id].emissionMultiple;

 File ArtGobblers.sol, line 905:

    getUserData[from].emissionMultiple -= emissionMultiple;

Becomes:

    getUserData[from].emissionMultiple = getUserData[from].emissionMultiple - emissionMultiple;

##### Instances include:

 File ArtGobblers.sol, line 456, 662, 844, 912.
 File Pages.sol, line 244.
 File GobblersERC721.sol, line 184.