Total Gas Optimizations: 12

## (1) <Array>.length Should Not Be Looked Up In Every Loop Of A For-loop

Severity: Gas Optimizations

The overheads outlined below are PER LOOP, excluding the first loop

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

## Proof Of Concept

	for (uint256 i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37

	for (uint256 i = 0; i < owners.length; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L114




## (2) ++i Costs Less Gas Than i++, Especially When It’s Used In For-loops (--i/i-- Too)

Severity: Gas Optimizations

Saves 6 gas per loop

## Proof Of Concept


	for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L251

	for (uint256 i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37



## Recommended Mitigation Steps

For example, use ++i instead of i++



## (3) It Costs More Gas To Initialize Variables To Zero Than To Let The Default Of Zero Be Applied

Severity: Gas Optimizations

## Proof Of Concept


	for (uint256 i = 0; i < numGobblers; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L592

	for (uint256 i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37

	for (uint256 i = 0; i < owners.length; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L114

	for (uint256 i = 0; i < amount; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L173

	for (uint256 i = 0; i < amount; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L186






## (4) Using > 0 Costs More Gas Than != 0 When Used On A Uint In A Require() Statement

Severity: Gas Optimizations

This change saves 6 gas per instance

## Proof Of Concept


	require(x > 0, "UNDEFINED");
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L142





## (5) Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Severity: Gas Optimizations

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

## Proof Of Concept

	mapping(address => bool) public hasClaimedMintlistGobbler;
	mapping(address => mapping(uint256 => uint256))) public getCopiesOfArtGobbledByGobbler;

https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L149

	mapping(address => UserData) public getUserData;
	mapping(address => mapping(address => bool)) public isApprovedForAll;

https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L59

	mapping(address => uint256) internal _balanceOf;
	mapping(address => mapping(address => bool)) internal _isApprovedForAll;

https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L52



## (6) <x> += <y> Costs More Gas Than <x> = <x> + <y> For State Variables

Severity: Gas Optimizations

## Proof Of Concept

	uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L844

	getUserData[from].emissionMultiple -= emissionMultiple;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L905

	getUserData[from].gobblersOwned -= 1;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L906

	getUserData[to].emissionMultiple += emissionMultiple;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L912

	getUserData[to].gobblersOwned += 1;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L913

	uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L244

	getUserData[to].gobblersOwned += uint32(amount);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L184





## (7) Using Private Rather Than Public For Constants, Saves Gas

Severity: Gas Optimizations

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

## Proof Of Concept


    uint256 public constant MAX_SUPPLY = 10000;
    uint256 public constant MINTLIST_SUPPLY = 2000;
    uint256 public constant LEGENDARY_SUPPLY = 10;
    uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
    uint256 public constant MAX_MINTABLE = MAX_SUPPLY
        - MINTLIST_SUPPLY
        - LEGENDARY_SUPPLY
        - RESERVED_SUPPLY;
    uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
    uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L112-L184



## Recommended Mitigation Steps

Set variable to private.



## (8) Public Functions To External

Severity: Gas Optimizations

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

## Proof Of Concept

	function gooBalance(address user) public view returns (uint256) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L757




## (9) Use of non-uint64 state variable is less efficient than uint256

Severity: Gas Optimizations

Lower than uint256 size storage variables are less gas efficient.
Using uint64 does not give any efficiency, actually, it is the opposite as EVM operates on default of 256-bit values so uint64 is more expensive in this case as it needs a conversion. It only gives improvements in cases where you can pack variables together, e.g. structs.

## Proof Of Concept


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

	uint32 emissionMultiple = getGobblerData[id].emissionMultiple;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L899

	uint128 public currentId;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L107

	uint128 public numMintedForCommunity;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L114

	uint64 idx;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L48

	uint32 emissionMultiple;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L50

	uint64 idx;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L38

	uint32 emissionMultiple;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L40

	uint32 gobblersOwned;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L49

	uint128 lastBalance;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L53

	uint64 lastTimestamp;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L55






## (10) ++i/i++ Should Be Unchecked{++i}/unchecked{i++} When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops

Severity: Gas Optimizations

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

## Proof Of Concept


	for (uint256 i = 0; i < numGobblers; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L592

	for (uint256 i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L37

	for (uint256 i = 0; i < owners.length; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L114

	for (uint256 i = 0; i < amount; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L173

	for (uint256 i = 0; i < amount; ++i) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L186





## (11) Use assembly to check for address(0)

Severity: Gas Optimizations

Save 6 gas per instance if using assembly to check for address(0)

e.g.
assembly {
	if iszero(_addr) {
		mstore(0x00, "AddressZero")
		revert(0x00, 0x20)
	}
}

## Proof Of Concept


	function balanceOf(address owner) external view returns (uint256) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L65

	function balanceOf(address owner) external view returns (uint256) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L58



## (12) Using Bools For Storage Incurs Overhead

Severity: Gas Optimizations

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

## Proof Of Concept


    mapping(address => mapping(address => bool)) public isApprovedForAll;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/tokens/ERC721.sol#L51

    mapping(address => bool) public hasClaimedMintlistGobbler;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L149

    mapping(address => mapping(address => bool)) public isApprovedForAll;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L37

    mapping(address => mapping(address => bool)) public isApprovedForAll;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L77

    mapping(address => mapping(address => bool)) internal _isApprovedForAll;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L70





