1)++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP



File: 2022-09-artgobblers\src\Pages.sol
  251,47:             for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
  
  
File: 2022-09-artgobblers\src\utils\GobblerReserve.sol
  37,49:             for (uint256 i = 0; i < ids.length; i++) {

2) ++i/i++ should be unchecked{++i}/unchecked{++i} when it is not possible for them to overflow,
as is the case when used in for- and while-loops   

File: 2022-09-artgobblers\src\ArtGobblers.sol
  
  432,43:             for (uint256 i = 0; i < cost; ++i) {
  592,50:             for (uint256 i = 0; i < numGobblers; ++i) {

File: 2022-09-artgobblers\src\utils\token\GobblersERC721.sol
  186,45:             for (uint256 i = 0; i < amount; ++i) {  
  
File: 2022-09-artgobblers\src\utils\token\GobblersERC1155B.sol
  114,52:             for (uint256 i = 0; i < owners.length; ++i) {
  173,45:             for (uint256 i = 0; i < amount; ++i) {  
  
File: 2022-09-artgobblers\src\Pages.sol
  251,47:             for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

File: 2022-09-artgobblers\src\utils\GobblerReserve.sol
  37,49:             for (uint256 i = 0; i < ids.length; i++) {

  
3)It costs more gas to initialize variables to zero than to let the default of zero be applied

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {  
 
File: 2022-09-artgobblers\src\ArtGobblers.sol
  
  432,43:             for (uint256 i = 0; i < cost; ++i) {
  592,50:             for (uint256 i = 0; i < numGobblers; ++i) {

File: 2022-09-artgobblers\src\utils\token\GobblersERC721.sol
  186,45:             for (uint256 i = 0; i < amount; ++i) {  
  
File: 2022-09-artgobblers\src\utils\token\GobblersERC1155B.sol
  114,52:             for (uint256 i = 0; i < owners.length; ++i) {
  173,45:             for (uint256 i = 0; i < amount; ++i) {  
  
File: 2022-09-artgobblers\src\Pages.sol
  251,47:             for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

File: 2022-09-artgobblers\src\utils\GobblerReserve.sol
  37,49:             for (uint256 i = 0; i < ids.length; i++) {


4)X = X + Y IS CHEAPER THAN X += Y 

File: 2022-09-artgobblers\src\ArtGobblers.sol
  439,37:                 burnedMultipleTotal += getGobblerData[id].emissionMultiple;
  456,54:             getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
  464,49:             legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
  662,62:                 getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
  844,68:             uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
  912,46:             getUserData[to].emissionMultiple += emissionMultiple;
  913,43:             getUserData[to].gobblersOwned += 1;
  
File: 2022-09-artgobblers\src\Pages.sol
  244,70:             uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);

File: 2022-09-artgobblers\src\utils\token\GobblersERC721.sol
  184,43:             getUserData[to].gobblersOwned += uint32(amount);  


5)Using private rather than public for constants, saves gas
If needed, the value can be read from the verified contract source code.
Savings are due to the compiler not having to create non-payable getter
functions for deployment calldata, and not adding another entry to the method ID table   
  
File: 2022-09-artgobblers\script\deploy\DeployRinkeby.s.sol
  13,12:     string public constant gobblerBaseUri = "https://testnet.ag.xyz/api/nfts/gobblers/";
  14,12:     string public constant gobblerUnrevealedUri = "https://testnet.ag.xyz/api/nfts/unrevealed";
  15,12:     string public constant pagesBaseUri = "https://testnet.ag.xyz/api/nfts/pages/";

File: 2022-09-artgobblers\src\ArtGobblers.sol
  112,13:     uint256 public constant MAX_SUPPLY = 10000;
  115,13:     uint256 public constant MINTLIST_SUPPLY = 2000;
  118,13:     uint256 public constant LEGENDARY_SUPPLY = 10;
  122,13:     uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
  126,13:     uint256 public constant MAX_MINTABLE = MAX_SUPPLY
  177,13:     uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
  180,13:     uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
  184,13:     uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
 
6)USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.2 to get compiler automatic inlining

Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

27 results - 27 files

File: 2022-09-artgobblers\script\deploy\DeployBase.s.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\script\deploy\DeployRinkeby.s.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\ArtGobblers.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\Goo.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\Pages.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\utils\GobblerReserve.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\utils\rand\ChainlinkV1RandProvider.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\utils\rand\RandProvider.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\utils\token\GobblersERC721.sol:
  1  // SPDX-License-Identifier: AGPL-3.0-only
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\utils\token\GobblersERC1155B.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

File: 2022-09-artgobblers\src\utils\token\PagesERC721.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity >=0.8.0;
  3  

7)<ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP
The overheads outlined below are PER LOOP, 

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset


 
 
File: 2022-09-artgobblers\src\utils\GobblerReserve.sol
  37,40:             for (uint256 i = 0; i < ids.length; i++) { 
  
File: 2022-09-artgobblers\src\utils\token\GobblersERC1155B.sol
 
  114,43:             for (uint256 i = 0; i < owners.length; ++i) {
  
  
  

8)Use custom errors rather than revert()/require() strings to save gas

File: 2022-09-artgobblers\src\ArtGobblers.sol
  437,17:                 require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
  885,9:         require(from == getGobblerData[id].owner, "WRONG_FROM");
  887,9:         require(to != address(0), "INVALID_RECIPIENT");
  889,9:         require(
                            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
                          "NOT_AUTHORIZED"
                        );

File: 2022-09-artgobblers\src\utils\token\GobblersERC721.sol
  62,9:         require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
  66,9:         require(owner != address(0), "ZERO_ADDRESS");
  95,9:         require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
  121,9:         require(
                          to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );
  137,9:         require(
                          to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );

File: 2022-09-artgobblers\src\utils\token\GobblersERC1155B.sol
  107,9:         require(owners.length == ids.length, "LENGTH_MISMATCH");
  149,13:             require(
                                ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
                    ERC1155TokenReceiver.onERC1155Received.selector,
                "UNSAFE_RECIPIENT"
            );
  185,13:             require(	
                              ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
                    ERC1155TokenReceiver.onERC1155BatchReceived.selector,
                "UNSAFE_RECIPIENT"
            );
			
File: 2022-09-artgobblers\src\utils\token\PagesERC721.sol
  55,9:         require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
  59,9:         require(owner != address(0), "ZERO_ADDRESS");
  85,9:         require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
  103,9:         require(from == _ownerOf[id], "WRONG_FROM");
  105,9:         require(to != address(0), "INVALID_RECIPIENT");
  107,9:         require(
                           msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
                       "NOT_AUTHORIZED"
                         );
  135,13:             require(
                             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
            );
  151,13:             require(	
                             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
            );  
							
9)++x is more efficient than x++

File: 2022-09-artgobblers\src\utils\token\PagesERC721.sol
  117,27:             _balanceOf[to]++;
  181,27:             _balanceOf[to]++;

10)STATE VARIABLES CAN BE PACKED INTO FEWER STORAGE SLOTS
If variables occupying the same slot are both written the same function or by the constructor,
avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper

File: 2022-09-artgobblers\src\ArtGobblers.sol
 
  724,9:               uint256 gobblerId,
  725,9:               address nft,
  726,9:               uint256 id,
  727,9:               bool isERC1155  
  
  
  Variable ordering with 5 slots instead of the current 6:
		uint256(32):shares,uint256(32):deadline, address(20):receiver,bool(1):approveMax,
		uint8(1):v,bytes32(32):r,bytes32(32):s
 