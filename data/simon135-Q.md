## No address 0 check 
```
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
        mintStart = _mintStart;
        merkleRoot = _merkleRoot;

        goo = _goo;
        pages = _pages;
        team = _team;
        community = _community;
        randProvider = _randProvider;

        BASE_URI = _baseUri;
        UNREVEALED_URI = _unrevealedUri;
```
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L287-L321
https://github.com/code-423n4/2022-09-artgobblers/blob/fb54f92ffcb0c13e72c84cde24c138866d9988e8/src/ArtGobblers.sol#L560
