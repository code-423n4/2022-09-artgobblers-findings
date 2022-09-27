1.

It is best practice and also unnecessary to initialize variables in for loops as they get set to 0 by default in:

Contract: ArtGobblers.sol

	line 432
	line 592
	
Recommendation:

	for (uint256 i; i < cost; ++i) {
	for (uint256 i; i < numGobblers; ++i) {
	
Contract: GobblersERC721.sol

	line 186
	
Recommendation:

	for (uint256 i; i < amount; ++i) {
	
Contract: Pages.sol

	line 251
	
Recommendation:

	for (uint256 i; i < numPages; i++) _mint(community, ++lastMintedPageId);
	
Contract: GobblerReserve.sol

	line 37
	
Recommendation:

	for (uint256 i; i < ids.length; i++) {

2.

It is best practice to always start a new statement/sentence in capital letters, in comments, unless it's a statement indication. 
This will also bring more consistency throughout all code under review.

Contract: ArtGobblers.sol

	line 477

Recommendation:

	/// @return Price of legendary gobbler, in terms of gobblers.
	
3. 

It is best practice to use the safe library from OpenZeppelin to, in this case, make external transactions. 
- might be a function
- look deeper

Contract: ArtGobblers.sol

	line 748
	
Recommendation:
	
	: ERC721(nft).safeTransferFrom(msg.sender, address(this), id);
	
Contract: GobblerReserve.sol

	line 38
	
Recommendation:

	artGobblers.safeTransferFrom(address(this), to, ids[i]);
	
4.

Missing variable value indication in:

Even though uint variables are uint256 by default, it is best practice to always still indicate the value in:

	line 763
	
Recommendation:

	uint256(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
	
5.

Inconsistent spacing in comments:

Contract: SignedWadMath.sol

	line 105
	
Recommendation:

	// Evaluate using a (6, 7) - term rational approximation.
	
6.

Grammer issues in comments in:

Contract: ArtGobblers.sol

	line 165

	"non ledendary" should either be one word "nonlegendary" or hyphened "non-legendary"

	line 182

	"have" should be "has"	

	line 550

	"moduloing" should be "modeling"

	line 573

	"shuffle" should be "shuffles"

	line 678

	"state" should be "states"

	line 719

	"to feed" should be "feeds"

	line 819

	"increase" should be "increased"	
	
Contract: FixedPointMathLib.sol
	
	line 212

	"worst case" should be hyphened into "worst-case"
	
Contract: ERC721.sol
	
	line 220

	"which" should be "that"	
	
Contract: DeployBase.s.sol

	line 65

	"who" should be "that"
	
Contract: Pages.sol

	line 121 & 127

	"18 decimal" should be hyphened "18-decimal"

Contract: ChainlinkV1RandProvider.sol

	line 61

	"by" should be "be"
	
Contract: LogisticToLinearVRGDA.sol

	line 19, 23 & 27

	"18 decimal" and "fixed point" should be "18-decimal" and "fixed-point" respectively

	line 58

	the "a" before "number" should be "the"

Contract: Goo.sol

	line 99

	"to" should be "too"
	
Contract: LogisticVRGDA.sol


	line 17 & 22

	there is a counterintuitive "of tokens" here. The latter "of tokens" should be removed.

	line 19 & 29

	"18 decimal" and "fixed point" should be "18-decimal" and "fixed-point" respectively

	line 24

	"36 decimal" and "fixed point" should be "36-decimal" and "fixed-point" respectively

	line 57

	the "for" is unnecessary

Contract: LibString.sol

	line 11

	"word aligned" should be hyphened "world-aligned"
	
Contract: VRGDA.sol

	line 16 & 20

	"18 decimal" and "fixed point" should be "18-decimal" and "fixed-point" respectively

	line 55 & 56

	the "a" before "number" should be "the"

	line 56

	the "for" is unnecessary
	
Contract: LibGOO.sol

	line 15

	the second "to" should be "too"

Contract: LogisticVRGDA.sol


	line 17 & 22

	there is a counterintuitive "of tokens" here. The latter "of tokens" should be removed.

	line 19 & 29

	"18 decimal" and "fixed point" should be "18-decimal" and "fixed-point" respectively

	line 24

	"36 decimal" and "fixed point" should be "36-decimal" and "fixed-point" respectively

	line 56 & 57

	the "a" before "number" should be "the"

	line 57

	the "for" is unnecessary