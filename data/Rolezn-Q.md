Total Low Severity: 3
Total Non-Critical Severity: 6

## (1) Use _safeMint instead of _mint

Severity: Low

According to openzepplin's ERC721, the use of _mint is discouraged, use _safeMint whenever possible.
https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_mint-address-uint256-

## Proof Of Concept

	_mint(msg.sender, gobblerId);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L356

	_mint(msg.sender, gobblerId);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L389

	_mint(to, amount);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L102

	_mint(msg.sender, pageId);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L211

	for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L251



## Recommended Mitigation Steps

Use _safeMint whenever possible instead of _mint



## (2) Missing Checks for Address(0x0) 

Severity: Low

## Proof Of Concept


	function transferOwnership(address _to)
    external
    onlyOwner()
  {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/chainlink/contracts/src/v0.6/Owned.sol#L30



## Recommended Mitigation Steps

Consider adding zero-address checks in the mentioned codebase.



## (3) Use Safetransfer Instead Of Transfer 

Severity: Low

It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

## Proof Of Concept


	: ERC721(nft).transferFrom(msg.sender, address(this), id);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L748

	artGobblers.transferFrom(address(this), to, ids[i]);
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L38



## Recommended Mitigation Steps

Consider using safeTransfer/safeTransferFrom or require() consistently.





## (4) Constants Should Be Defined Rather Than Using Magic Numbers

Severity: Non-Critical

## Proof Of Concept

            interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/tokens/ERC721.sol#L148

            interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L126

            interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L151

            interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L164





## (5) Missing parameter validation

Severity: Non-Critical

Some parameters of constructors are not checked for invalid values.

## Proof Of Concept

	address _owner
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/auth/Owned.sol#L29

	address _teamColdWallet
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L38

	address _vrfCoordinator
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L41

	address _linkToken
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L42

	address _team
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L294

	address _community
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L295

	address _artGobblers
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L82

	address _pages
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L82

	address _community
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L161

	address _owner
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L23

	address _vrfCoordinator
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol#L50

	address _linkToken
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol#L51



## Recommended Mitigation Steps

Validate the parameters.



## (6) Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

Severity: Non-Critical

## Proof Of Concept

	constructor(
        // Mint config:
        bytes32 _merkleRoot,
        uint256 _mintStart,
        // Addresses:
        Goo _goo,
        Pages _pages,
        address _team,
        address _community,
        RandProvider _randProvider,
        // URIs:
        string memory _baseUri,
        string memory _unrevealedUri
    )
        GobblersERC721("Art Gobblers", "GOBBLER")
        Owned(msg.sender)
        LogisticVRGDA(
            69.42e18, // Target price.
            0.31e18, // Price decay percent.
            // Max gobblers mintable via VRGDA.
            toWadUnsafe(MAX_MINTABLE),
            0.0023e18 // Time scale.
        )
    {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L287-L310

    constructor(address _artGobblers, address _pages) {
        artGobblers = _artGobblers;
        pages = _pages;
    } 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#82-L85

    constructor(
        // Mint config:
        uint256 _mintStart,
        // Addresses:
        Goo _goo,
        address _community,
        ArtGobblers _artGobblers,
        // URIs:
        string memory _baseUri
    )
        PagesERC721(_artGobblers, "Pages", "PAGE")
        LogisticToLinearVRGDA(
            4.2069e18, // Target price.
            0.31e18, // Price decay percent.
            9000e18, // Logistic asymptote.
            0.014e18, // Logistic time scale.
            SOLD_BY_SWITCH_WAD, // Sold by switch.
            SWITCH_DAY_WAD, // Target switch day.
            9e18 // Pages to target per day.
        )
    {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L156-L176

    constructor(ArtGobblers _artGobblers, address _owner) Owned(_owner) {
        artGobblers = _artGobblers;
    }
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L23-L25






## (7) Use a more recent version of Solidity

Severity: Non-Critical

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

## Proof Of Concept


	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/goo-issuance/src/LibGOO.sol#L2

	Found old version 0.8.0 of Solidity
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/tokens/ERC721.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/FixedPointMathLib.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/LibString.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/MerkleProofLib.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/VRGDAs/src/LinearVRGDA.sol#L2

	Found old version 0.8.0 of Solidity
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/VRGDAs/src/LogisticToLinearVRGDA.sol#L2

	Found old version 0.8.0 of Solidity
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/VRGDAs/src/LogisticVRGDA.sol#L2

	Found old version 0.8.0 of Solidity
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/VRGDAs/src/VRGDA.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployBase.s.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/script/deploy/DeployRinkeby.s.sol#L2

	Found old version 0.8.0 of Solidity
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Goo.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/rand/ChainlinkV1RandProvider.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L2

	Found old version 0.8.0 of Solidity 
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L2

	Found old version 0.8.0 of Solidity
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L2



## Recommended Mitigation Steps

Consider updating to a more recent solidity version.



## (8) Unused event may be unused code or indicative of missed emit/logic

Severity: Non-Critical

Events that are declared but not used may be indicative of unused declarations where it makes sense to remove them for better readability/maintainability/auditability, or worse indicative of a missing emit which is bad for monitoring or missing logic that would have emitted that event.

## Proof Of Concept


	event URI
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L31



## Recommended Mitigation Steps
Add emit or remove event declaration.



## (9) Large multiples of ten should use scientific notation

Severity: Non-Critical

Use (e.g. 1e6) rather than decimal literals (e.g. 100000), for better code readability.

## Proof Of Concept

        r := mul(x, 1000000000000000000)
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L11

        r := div(mul(x, 1000000000000000000), 86400)
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L21

        r := div(mul(x, 86400), 1000000000000000000)
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L31

        r := sdiv(mul(x, y), 1000000000000000000)
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L39

        r := sdiv(mul(x, 1000000000000000000), y)
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L48

        r := sdiv(r, 1000000000000000000)
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L63

        if iszero(and(iszero(iszero(y)), eq(sdiv(r, 1000000000000000000), x))) {
https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib/solmate/src/utils/SignedWadMath.sol#L73

    uint256 public constant MAX_SUPPLY = 10000;
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L112





