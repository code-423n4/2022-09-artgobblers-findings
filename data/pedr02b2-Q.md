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


When feeding gobblers, only checks on address owner and msg.sender are made, when in reality line79 GoblersERC1155B, line 102 GoblersERC721 both use "setApprovalForAll" and "Approve" which can be used as a check to ensure that a work around for using an NFT your not approved to use cannot be found. in the test file "pages" is used to see if you own the nft but this check does not appear to be in the function either 

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/test/ArtGobblers.t.sol#L963
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L723


### No checks on if NFT ID has been gobbled before

There does not seem to be any tracking as to what NFTs have been fed to gobblers, I see that there is a mapping to increment the # in this mapping of gobbler id to nft contracts fed and that it maps the gobbler id to nft contract Ids, and increments but does not track NFT address as well as Id.

		addresses /// @notice Maps gobbler ids to NFT contracts and their ids to the # of those NFT ids gobbled by the gobbler.

    	mapping(uint256 => mapping(address => mapping(uint256 => uint256))) public getCopiesOfArtGobbledByGobbler; 

But it does not appear to be used, I would assume it would maybe used to make sure the same Ids are not used or similar but that does not seem to be a concerne either (at least it appears not to check if the same id have been used before) this would be good to use as a check to make sure the same Ids of nfts are not being used to feed the gobbler, or use this in a public veiwable function that ties address(this) too ids of copiesOfArtGobbledByGobbler, so that a user can see what Ids etc have been used on thier gobbler.

### No Checks on NFT type (can user accidentely feed a high value NFT)

No check on what NFT you can feed the gobbler, this could leave this function open to an inexperienced user feeding a gobbler an NFT of high value accidentely, unless this is how the dev intended this function to work(that any NFT can be used) as atm user can simply add "Address, Id" of any nft user owns, Instigate a check here to see what exaclty the NFT is for example "Pages" as i see this used in the TEST file it would be a good use case here to ensure the right NFT is being used.

Best practice here would be to add checks for the following as well as does user own the gobbler, is user msg.sender, does user own the NFT they want to feed the gobbler and or is user approved to use said NFT, has it been used before (ID reuse) is it ERC1155, ERC721, is it the correct NFT (not one user owns thats worth £2k)