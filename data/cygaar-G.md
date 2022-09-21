This line: https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L412 can be placed in an `unchecked` block. It cannot overflow due to the check on line 415.
\
\
In Pages.sol, you can rewrite `mintCommunityPages` as follows:
```
function mintCommunityPages(uint128 numPages) external returns (uint256 lastMintedPageId) {
        unchecked {
            // Optimistically increment numMintedForCommunity, may be reverted below.
            // Overflow in this calculation is possible but numPages would have to
            // be so large that it would cause the loop to run out of gas quickly.
            uint128 newNumMintedForCommunity = numMintedForCommunity += numPages;

            // Ensure that after this mint pages minted to the community reserve won't comprise more than
            // 10% of the new total page supply. currentId is equivalent to the current total supply of pages.
            if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance();

            // Mint the pages to the community reserve while updating lastMintedPageId.
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

            currentId = uint128(lastMintedPageId); // Update currentId with the last minted page id.

            emit CommunityPagesMinted(msg.sender, lastMintedPageId, numPages);
        }
    }
```
This ends up saving gas overall, although a couple of the tests do incur increased gas consumption.
\
\
In `safeTransferFrom` for ERC1155, you can save gas in this specific project by changing the location of `data` from `calldata` to `memory`. sure if it's worth making the change in case future contracts call the 1155 contract in the future