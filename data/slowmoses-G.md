## (1) Use Pre Increment Instead of Post Increment

Pre-increment uses a little less gas.

***

_balanceOf[from]--;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L115

_balanceOf[to]++;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L117

_balanceOf[to]++;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L181

for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L251

for (uint256 i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37

_balanceOf[from]--;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L99

_balanceOf[to]++;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L101

_balanceOf[to]++;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L164

_balanceOf[owner]--;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L179

## (2) Cache Array Length

Array length should be cached instead of requesting it over and over in a for loop.
The cost per iteration depends on the array type, 100 gas for storage, 3 each for memory and calldata.
Caching the value costs only 3 gas.

***

for (uint256 i = 0; i < owners.length; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L114

for (uint256 i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37

## (3) Use Custom Errors Instead of Strings for Revert and Require

Require strings and revert strings use about 50 more gas than custom errors.

***

require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L437

if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L696

if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L705

revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L711

require(from == getGobblerData[id].owner, "WRONG_FROM");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L885

require(to != address(0), "INVALID_RECIPIENT");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L887

require(
    msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
    "NOT_AUTHORIZED"
);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L889-L892

require(owners.length == ids.length, "LENGTH_MISMATCH");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L107

require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L62

require(owner != address(0), "ZERO_ADDRESS");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L66

require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L95

require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L55

require(owner != address(0), "ZERO_ADDRESS");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L59

require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L85

require(from == _ownerOf[id], "WRONG_FROM");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L103

require(to != address(0), "INVALID_RECIPIENT");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L105

if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L266

require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L36

require(owner != address(0), "ZERO_ADDRESS");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L40

require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L69

require(from == _ownerOf[id], "WRONG_FROM");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L87

require(to != address(0), "INVALID_RECIPIENT");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L89

require(to != address(0), "INVALID_RECIPIENT");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L158

require(_ownerOf[id] == address(0), "ALREADY_MINTED");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L160

require(owner != address(0), "NOT_MINTED");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L175

if (x >= 135305999368893231589) revert("EXP_OVERFLOW");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L90

require(x > 0, "UNDEFINED");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L142

require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L32

require(msg.sender == owner, "UNAUTHORIZED");
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L20

## (4) Boolean Variables Should not be Used for Storage

Using UINT256(1) for true and UINT256(2) for false costs less gas than using boolean variables.

***

mapping(address => bool) public hasClaimedMintlistGobbler;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L149

mapping(address => mapping(address => bool)) public isApprovedForAll;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L37

mapping(address => mapping(address => bool)) public isApprovedForAll;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L77

mapping(address => mapping(address => bool)) internal _isApprovedForAll;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L70

mapping(address => mapping(address => bool)) public isApprovedForAll;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L51

## (5) Do not Initialize to Default Value

Non-const variables should not be initialized to their default value.
Just using the default costs less gas.
For example use: uint256 abc;
Instead of: uint256 abc=0;

***

for (uint256 i = 0; i < cost; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L432

for (uint256 i = 0; i < numGobblers; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L592

for (uint256 i = 0; i < owners.length; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L114

for (uint256 i = 0; i < amount; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L173

for (uint256 i = 0; i < amount; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L186

for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L251

for (uint256 i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37

## (6) Avoid Ints or Uints Smaller Than 256 Bits

Using ints or uints smaller than 256 bits can cost extra gas since the EVM operates with 32 bytes at a time.
Consider using int256 or uint256. You can downcast them when necessary.

***

uint128 public numMintedFromGoo;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L159

uint128 public currentNonLegendaryId;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L167

uint128 startPrice;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L189

uint128 numSold;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L191

uint64 randomSeed;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L204

uint64 nextRevealTimestamp;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L206

uint56 lastRevealedId;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L208

uint56 toBeRevealed;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L210

uint32 emissionMultiple = getGobblerData[id].emissionMultiple; // Caching saves gas.
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L899

uint64 idx;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L48

uint32 emissionMultiple;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L50

uint64 idx;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L38

uint32 emissionMultiple;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L40

uint128 public currentId;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L107

uint128 public numMintedForCommunity;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L114


