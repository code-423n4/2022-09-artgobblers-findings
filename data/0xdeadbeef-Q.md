## In the file Pages.sol: 
Line 103	    uint256 public immutable mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 158	        uint256 _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 177	        mintStart = _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 195	    function mintFromGoo(uint256 maxPrice, bool useVirtualBalance) external returns (uint256 pageId) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 211	            _mint(msg.sender, pageId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 222	        uint256 timeSinceStart = block.timestamp - mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 239	    function mintCommunityPages(uint256 numPages) external returns (uint256 lastMintedPageId) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 251	            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 253	            currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 161: _community
```
## In the file ArtGobblers.sol: 
Line 156	    uint256 public immutable mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 290	        uint256 _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 311	        mintStart = _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 327	        gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days): 
: _safemint() should be used instead of _mint() function whereever possible

Line 341	        if (mintStart > block.timestamp) revert MintStartPending(): 
: _safemint() should be used instead of _mint() function whereever possible

Line 356	        _mint(msg.sender, gobblerId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 368	    function mintFromGoo(uint256 maxPrice, bool useVirtualBalance) external returns (uint256 gobblerId) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 389	        _mint(msg.sender, gobblerId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 399	        uint256 timeSinceStart = block.timestamp - mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 411	    function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 469	            _mint(msg.sender, gobblerId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 482	        uint256 mintedFromGoo = numMintedFromGoo: 
: _safemint() should be used instead of _mint() function whereever possible

Line 490	            if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo): 
: _safemint() should be used instead of _mint() function whereever possible

Line 711	        revert("NOT_MINTED"); // Unminted legendaries and invalid token ids: 
: _safemint() should be used instead of _mint() function whereever possible

Line 786	        goo.mintForGobblers(msg.sender, gooAmount): 
: _safemint() should be used instead of _mint() function whereever possible

Line 839	    function mintReservedGobblers(uint256 numGobblersEach) external returns (uint256 lastMintedGobblerId) : 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 294: _team
	line 295: _community
	line 725: nft
	line 757: user
	line 793: user
	line 814: user
	line 872: user
	line 881: from
```


## In the file GobblersERC1155B.sol: 
Line 135	    function _mint: 
 _safemint() should be used instead of _mint() function whereever possible

Line 192	        return lastMintedId; // Return the new last minted id: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 55: owner
	line 59: owner
	line 79: operator
	line 86: from
	line 86: from
```
## In the file GobblersERC721.sol: 
Line 160	    function _mint(address to, uint256 id) internal : 
 _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 92: spender
	line 102: operator
	line 109: from
	line 109: from
	line 109: from
```
## In the file PagesERC721.sol: 
Line 173	    function _mint(address to, uint256 id) internal : 
 _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 82: spender
	line 92: operator
	line 99: from
	line 99: from
	line 99: from
```
## In the file DeployBase.s.sol: 
Line 20	    uint256 private immutable mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 40	        uint256 _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 51	        mintStart = _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 91	            mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 102	        pages = new Pages(mintStart, goo, teamColdWallet, artGobblers, pagesBaseUri): 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 38: _teamColdWallet
	line 41: _vrfCoordinator
	line 42: _linkToken
	line 94: teamReserve
	line 95: communityReserve
```
## In the file ArtGobblers.sol: 
Line 156	    uint256 public immutable mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 290	        uint256 _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 311	        mintStart = _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 327	        gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days): 
: _safemint() should be used instead of _mint() function whereever possible

Line 341	        if (mintStart > block.timestamp) revert MintStartPending(): 
: _safemint() should be used instead of _mint() function whereever possible

Line 356	        _mint(msg.sender, gobblerId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 368	    function mintFromGoo(uint256 maxPrice, bool useVirtualBalance) external returns (uint256 gobblerId) {//@audit: 理论上逻辑没问题 但是需要看下属每个函数是否能绕过: 
: _safemint() should be used instead of _mint() function whereever possible

Line 389	        _mint(msg.sender, gobblerId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 399	        uint256 timeSinceStart = block.timestamp - mintStart;//@audit: this will revert if not properly se: 
: _safemint() should be used instead of _mint() function whereever possible

Line 411	    function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 469	            _mint(msg.sender, gobblerId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 482	        uint256 mintedFromGoo = numMintedFromGoo: 
: _safemint() should be used instead of _mint() function whereever possible

Line 490	            if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo): 
: _safemint() should be used instead of _mint() function whereever possible

Line 711	        revert("NOT_MINTED"); // Unminted legendaries and invalid token ids: 
: _safemint() should be used instead of _mint() function whereever possible

Line 786	        goo.mintForGobblers(msg.sender, gooAmount): 
: _safemint() should be used instead of _mint() function whereever possible

Line 839	    function mintReservedGobblers(uint256 numGobblersEach) external returns (uint256 lastMintedGobblerId) : 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 294: _team
	line 295: _community
	line 725: nft
	line 757: user
	line 793: user
	line 814: user
	line 872: user
	line 881: from
```
## In the file attack_ArtGobblers.sol: 
Line 98	            _mint(msg.sender, gobblerId): 
 _safemint() should be used instead of _mint() function whereever possible

## In the file Goo.sol: 
Line 101	    function mintForGobblers(address to, uint256 amount) external only(artGobblers) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 102	        _mint(to, amount): 
 _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 82: _artGobblers
	line 101: to
	line 108: from
	line 115: from
```
## In the file Pages.sol: 
Line 103	    uint256 public immutable mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 158	        uint256 _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 177	        mintStart = _mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 195	    function mintFromGoo(uint256 maxPrice, bool useVirtualBalance) external returns (uint256 pageId) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 211	            _mint(msg.sender, pageId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 222	        uint256 timeSinceStart = block.timestamp - mintStart: 
: _safemint() should be used instead of _mint() function whereever possible

Line 239	    function mintCommunityPages(uint256 numPages) external returns (uint256 lastMintedPageId) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 251	            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 253	            currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 161: _community
```
## In the file GobblerReserve.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 23: _owner
	line 34: to
```
## In the file GobblersERC1155B.sol: 
Line 135	    function _mint: 
 _safemint() should be used instead of _mint() function whereever possible

Line 192	        return lastMintedId; // Return the new last minted id: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 55: owner
	line 59: owner
	line 79: operator
	line 86: from
	line 86: from
```
## In the file GobblersERC721.sol: 
Line 160	    function _mint(address to, uint256 id) internal : 
 _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 92: spender
	line 102: operator
	line 109: from
	line 109: from
	line 109: from
```
