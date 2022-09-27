- claimGobbler - team needs to make 300 extra wallets to claim their tokens
    
    Since claiming is for [1 gobbler for 1 address](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L344), then the team can only claim 1 gobbler to their cold wallet. This means the core team needs to create 300 wallet addresses and withdraw the gobblers separately.
    
    This takes some gas and considerable human effort and adds security risk with management for this amount of wallets.
    
    You could also consider creating a for loop to retrieve all of the gobblers in a single function call.
    
    
- consider adding update Chainlink fee method
    
    Currently fee is fixed. If Chainlink changes their fee, then the randomness request could possibly fail. You would need to deploy a new RandomProvider.
    
    [https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol/#L69](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol/#L69)
    
- VRF1 is deprecated
    
    According to [https://docs.chain.link/docs/vrf/v1/security/](https://docs.chain.link/docs/vrf/v1/security/), the VRF1 is deprecated, meaning VRF2 should be preferred. 
    
    Art Gobblers uses [V1](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol/#L14).
    
    Deprecated products are more likely to be discontinued. This does not seem like a big problem because you can update your random provider. 
    
- disable `PagesERC721.approve/setApprovalForAll` , since it is not used in the transferFrom function
    
    If you approve an NFT spender, you should be able to transfer the token. However, in `transferFrom`, the function will revert, because it checks only owner, not approval.
    
    ```json
    function transferFrom(
        address from,
        address to,
        uint256 id
    ) public {
        require(from == _ownerOf[id], "WRONG_FROM");
    ```
    
    [https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol/#L103](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol/#L103)
    
    `setApprovalForAll`
    
    [https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol/#L94](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol/#L94)
    
    also in `ArtGobblers`
    
    [https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L885](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L885)
    
    [https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L733](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L733)
    
    However, if you disable the `setApprovedForAll`, then you would remove a possibility where users would delegate their art. For instance they could delegate their Pages NFT to a DAO, which would then gobble them.
    
    This is currently impossible, because you check `mg.sender` in the `gobble()` function. To enable this kind of NFT delegation, you would need to check the approval here as well.
    
    ```json
    function gobble() external {
        // The caller must own the gobbler they're feeding.
        if (owner != msg.sender) revert OwnerMismatch(owner);
    ```
    
    [https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L733](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol/#L733)

- disable gobbling of `ERC1155`
    
    Pages are ERC721. There is no obvious reason to allow gobbling of ERC1155.