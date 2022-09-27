# GAS
## Public function visibility can be made external
### Summary
Functions should have the strictest visibility possible. Public functions may lead to more gas usage by forcing the copy of their parameters to memory from calldata.
### Details
If a function is never called from the contract it should be marked as external. This will save gas.
### Github Permalinks
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/auth/Owned.sol#L39-L43

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L146-L151

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L126-L140

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L25

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L111-L124

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L693-L712

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L35-L37

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L66-L74

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L265-L269

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L39-L43

https://github.com/transmissions11/goo-issuance/blob/5fe1e7d8a0c42a97c2a95d0547209f28dcbedb0b/src/LibGOO.sol#L17-L41

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L76-L80

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/auth/Owned.sol#L39-L43

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L146-L151

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L126-L140

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L25

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L111-L124

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L693-L712

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L35-L37

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L66-L74

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L265-L269

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L39-L43

https://github.com/transmissions11/goo-issuance/blob/5fe1e7d8a0c42a97c2a95d0547209f28dcbedb0b/src/LibGOO.sol#L17-L41

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L76-L80


### Mitigation
Consider changing visibility from public to external.


## use of custom errors rather than revert() / require() error message
### Summary
Custom errors reduce 38 gas if the condition is met and 22 gas otherwise.
Also reduces contract size and deployment costs.
### Details
Since version 0.8.4 the use of custom errors rather than revert() / require() saves gas as noticed in
https://blog.soliditylang.org/2021/04/21/custom-errors/
https://github.com/code-423n4/2022-04-pooltogether-findings/issues/13

### Github Permalinks

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L437
                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L885
        require(from == getGobblerData[id].owner, "WRONG_FROM");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L887
        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L889
        require(

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L36
        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L40
        require(owner != address(0), "ZERO_ADDRESS");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L69
        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L87
        require(from == _ownerOf[id], "WRONG_FROM");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L89
        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L91
        require(

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L118
        require(

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L134
        require(

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L158
        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L160
        require(_ownerOf[id] == address(0), "ALREADY_MINTED");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L175
        require(owner != address(0), "NOT_MINTED");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L196
        require(

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L211
        require(

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L107
        require(owners.length == ids.length, "LENGTH_MISMATCH");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L149
            require(

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L185
            require(

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L142
        require(x > 0, "UNDEFINED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L62
        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L66
        require(owner != address(0), "ZERO_ADDRESS");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L95
        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L121
        require(

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L137
        require(

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L55
        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L59
        require(owner != address(0), "ZERO_ADDRESS");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L85
        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L103
        require(from == _ownerOf[id], "WRONG_FROM");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L105
        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L107
        require(

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L135
            require(

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L151
            require(

https://github.com/transmissions11/VRGDAs/blob/8d958618dbb15407a4a2ea2788ce9cc5399ebe61/src/VRGDA.sol#L32
        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/auth/Owned.sol#L20
        require(msg.sender == owner, "UNAUTHORIZED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L696
            if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L705
        if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L711
        revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L90
        if (x >= 135305999368893231589) revert("EXP_OVERFLOW");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L266
        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

### Mitigation
replace each error message in a require by a custom error

## duplicated require() check should be refactored
### Summary
duplicated require() / revert() checks should be
refactored to a modifier or function to save gas
### Details
Event appears twice and can be reduced

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L437
                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L885
        require(from == getGobblerData[id].owner, "WRONG_FROM");

- - - -

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L89
        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L158
        require(to != address(0), "INVALID_RECIPIENT");

- - - -

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L36
        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L175
        require(owner != address(0), "NOT_MINTED");


- - - -
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L696
            if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L705
        if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L711
        revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.

- - - -


### Mitigation
refactor this checks to different functions to save gas

## Store using Struct over multiple mappings
### Summary
All these variables could be combine in a Struct in order to reduce the gas cost. 
### Details
As noticed in: 
https://gist.github.com/alexon1234/b101e3ac51bea3cbd9cf06f80eaa5bc2
When multiple mappings that access the same addresses, uints, etc, all of them can be mixed into an struct and then that data accessed like:
mapping(datatype => newStructCreated) newStructMap;
Also, you have this post where it explains the benefits of using Structs over mappings 
https://medium.com/@novablitz/storing-structs-is-costing-you-gas-774da988895e

### Github Permalinks
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L31
    mapping(uint256 => address) internal _ownerOf;
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L49
    mapping(uint256 => address) public getApproved;
- - - 
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L33
    mapping(address => uint256) internal _balanceOf;
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L51
    mapping(address => mapping(address => bool)) public isApprovedForAll;
- - - 
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L44
    mapping(uint256 => GobblerData) public getGobblerData;
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L75
    mapping(uint256 => address) public getApproved;
- - - 
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L59
    mapping(address => UserData) public getUserData;
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L77
    mapping(address => mapping(address => bool)) public isApprovedForAll;
- - - 
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L50
    mapping(uint256 => address) internal _ownerOf;
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L68
    mapping(uint256 => address) public getApproved;
- - - 
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L52
    mapping(address => uint256) internal _balanceOf;
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L70
    mapping(address => mapping(address => bool)) internal _isApprovedForAll;

### Mitigation
Consider mixing different mappings into an struct when able in order to save gas.


## Using private rather than public for constants saves gas
### Summary
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

### Github Permalinks

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L112
    uint256 public constant MAX_SUPPLY = 10000;

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L115
    uint256 public constant MINTLIST_SUPPLY = 2000;

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L118
    uint256 public constant LEGENDARY_SUPPLY = 10;

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L122
    uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L126
    uint256 public constant MAX_MINTABLE = MAX_SUPPLY

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L177
    uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L180
    uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/src/ArtGobblers.sol#L184
    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/script/deploy/DeployRinkeby.s.sol#L13
    string public constant gobblerBaseUri = "https://testnet.ag.xyz/api/nfts/gobblers/";

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/script/deploy/DeployRinkeby.s.sol#L14
    string public constant gobblerUnrevealedUri = "https://testnet.ag.xyz/api/nfts/unrevealed";

https://github.com/code-423n4/2022-09-vtvl/blob/26dda235d38d0f870c1741c9f7eef03229172bbe/script/deploy/DeployRinkeby.s.sol#L15
    string public constant pagesBaseUri = "https://testnet.ag.xyz/api/nfts/pages/";
### Mitigation
Consider replacing public for private in constants for gas saving.

## Index initialized in for loop
### Summary
In for loops is not needed to initialize indexes to 0 as it is the default uint/int value. This saves gas.

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L432
            for (uint256 i = 0; i < cost; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L592
            for (uint256 i = 0; i < numGobblers; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L114
            for (uint256 i = 0; i < owners.length; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L173
            for (uint256 i = 0; i < amount; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L186
            for (uint256 i = 0; i < amount; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L251
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/GobblerReserve.sol#L37
            for (uint256 i = 0; i < ids.length; i++) {


### Mitigation
Don't initialize variables to default value


## use of i++ in loop rather than ++i
### Summary
++i costs less gas than i++, especially when it's used in for loops
### Details
using ++i doesn't affect the flow of regular for loops and improves gas cost

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L251
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/GobblerReserve.sol#L37
            for (uint256 i = 0; i < ids.length; i++) {


### Mitigation
Substitute to ++i 

## ++i costs less gas compared to i++ or i+=1, the same happens with --i and i-- or i-=1
### Summary
++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). 
This statement is true even with the optimizer enabled. 

### Details
i++ increments i and returns the initial value of i . 
Which means:
uint i = 1;
i++; // == 1 but i == 2

But ++i returns the actual incremented value:

uint i = 1;
++i; // == 2 and i == 2 too, so no need for a temporary variable

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

### Github Permalinks
`+= 1`
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L464
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L913
`-=`
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L906
`var--`
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L99
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L179
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/PagesERC721.sol#L115
`var++`
https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol#L101
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/PagesERC721.sol#L117

### Mitigation
Replace to ++i or --i as needed.


## increments can be unchecked in loops
### Summary
Unchecked operations as the ++i on for loops are cheaper than checked one.

### Details
In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline..

    The code would go from:

    for (uint256 i; i < numIterations; i++) {
    // ...
    }

    to

    for (uint256 i; i < numIterations;) {
      // ...
      unchecked { ++i; }
    }
    The risk of overflow is inexistent for a uint256 here.

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L432
            for (uint256 i = 0; i < cost; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L592
            for (uint256 i = 0; i < numGobblers; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L114
            for (uint256 i = 0; i < owners.length; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L173
            for (uint256 i = 0; i < amount; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC721.sol#L186
            for (uint256 i = 0; i < amount; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/Pages.sol#L251
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/GobblerReserve.sol#L37
            for (uint256 i = 0; i < ids.length; i++) {

### Mitigation
Add unchecked ++i at the end of all the for loop where it's not expected to overflow and remove them from the for header


## <array>.length should no be looked up in every loop of a for-loop
### Summary
In loops not assigning the length to a variable so memory accessed a lot (caching local variables)

### Details
The overheads outlined below are PER LOOP, excluding the first loop
storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L114
            for (uint256 i = 0; i < owners.length; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/GobblerReserve.sol#L37
            for (uint256 i = 0; i < ids.length; i++) {

### Mitigation
Assign the length of the array.length to a local variable in loops for gas savings


## Variables should be cached when used several times
### Summary
Variables read more than once improves gas usage when cached into local variable
### Details
In loops or state variables, this is even more gas saving

### Github Permalinks
`getGobblerData[id].owner`
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L437-L441

`getGobblerData[currentId].idx`
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L623-L625

`getGobblerData[swapId].idx`
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L615-L617
### Mitigation
Cache variables used more than one into a local variable.


## Shift right instead of dividing by 2 
### Summary
Shifting is cheaper than dividing by 2

### Details
A division by 2 can be calculated by shifting one to the right.
While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity’s division operation also includes a division-by-0 prevention which is bypassed using shifting.

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L462
cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L462
cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
### Mitigation
Consider replacing / 2 with >> 1 here

## Functions guaranteed to revert when called by normal users can be marked payable
### Summary
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function.

Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Details
The extra opcodes avoided are:
CALLVALUE (2), DUP1 (3), ISZERO (3), PUSH2 (3), JUMPI (10), PUSH1 (3), DUP1 (3), REVERT(0), JUMPDEST (1), POP (2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L560
    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/auth/Owned.sol#L39
    function setOwner(address newOwner) public virtual onlyOwner {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/GobblerReserve.sol#L34
    function withdraw(address to, uint256[] calldata ids) external onlyOwner {

### Mitigation
Consider adding payable to save gas

## <X> += <Y> costs more gas than <X> = <X> + <Y> for state variables
### Summary
x+=y costs more gas than x=x+y for state variables

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L456
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L464
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L662
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L844
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L912
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L913
https://github.com/code-423n4/2022-09-artgobblers/blob/6e0df2e5e82b51856e451d028a44593ef18c74b1/src/Pages.sol#L244
https://github.com/code-423n4/2022-09-artgobblers/blob/f3d4522ecfb6f02e6ca4ecd564d38e81d3021d4e/src/utils/token/GobblersERC721.sol#L184
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L905
7https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L906

### Mitigation
Don't use += for state variables as it cost more gas.

## Unused named returns
### Summary
Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity 
### Details
Also as returns variable is ignored, it wastes extra gas

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/GobblersERC1155B.sol#L55
    function ownerOf(uint256 id) public view virtual returns (address owner) {

https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/utils/SignedWadMath.sol#L82
function wadExp(int256 x) pure returns (int256 r) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/token/PagesERC721.sol#L72
    function isApprovedForAll(address owner, address operator) public view returns (bool isApproved) {

https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/utils/rand/ChainlinkV1RandProvider.sol#L62
    function requestRandomBytes() external returns (bytes32 requestId) {

### Mitigation
Remove return or returns when both used

## Make constant state variables that do not change
### Summary
State variables which value isn't changed by any function in the contract, can be declared as a constant state variable to save some gas during deployment.

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/01a169fdaa6c7dac9f5cc3fda6bdacfabaa7b824/script/deploy/DeployRinkeby.s.sol#L7-L11

### Mitigation
- Add constant to state variables that do not change

## Make immutable state variables that do not change but assigned in the constructor
### Summary
State variables which value isn't changed by any function in the contract but constructor, can be declared as a immutable state variable to save some gas during deployment.

### Github Permalinks
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L320-L321
https://github.com/code-423n4/2022-09-artgobblers/blob/6e0df2e5e82b51856e451d028a44593ef18c74b1/src/Pages.sol#L183
### Mitigation
- Add immutable to state variables that do not change but which value is assigned in constructor

