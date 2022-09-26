### Feeding gobblers unapproved NFT's

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L723


    function gobble(
        uint256 gobblerId,
        address nft,
        uint256 id,
        bool isERC1155
    ) external {
        // Get the owner of the gobbler to feed.
        address owner = getGobblerData[gobblerId].owner;

        // The caller must own the gobbler they're feeding.
        if (owner != msg.sender) revert OwnerMismatch(owner);

        // Gobblers have taken a vow not to eat other gobblers.
        if (nft == address(this)) revert Cannibalism();

        unchecked {
            // Increment the # of copies gobbled by the gobbler. Unchecked is
            // safe, as an NFT can't have more than type(uint256).max copies.
            ++getCopiesOfArtGobbledByGobbler[gobblerId][nft][id];
        }

        emit ArtGobbled(msg.sender, gobblerId, nft, id);

        isERC1155
            ? ERC1155(nft).safeTransferFrom(msg.sender, address(this), id, 1, "")
            : ERC721(nft).transferFrom(msg.sender, address(this), id);
    }


When feeding gobblers only checks on address owner and msg.sender are made when in reality line79 GoblersERC1155B, line 102 GoblersERC721 both use "setApprovalForAll" and "Approve" which can be used as a check to ensure that a work around for using an NFT your not approved to use cannot be found.


Gobblerrs.sol line82

    function approve(address spender, uint256 id) external {
        address owner = _ownerOf[id];


GobblersERC721.sol line92

    function setApprovalForAll(address operator, bool approved) public virtual {
        isApprovedForAll[msg.sender][operator] = approved;

        emit ApprovalForAll(msg.sender, operator, approved);
    }