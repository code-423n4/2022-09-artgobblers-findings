# Gas Optimization


## ++i costs less gas than i++ and i+=1 (--i/i--/i-=1 too)

        File: lib/solmate/src/tokens/ERC721.sol

            _balanceOf[to]++;

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L164

        File: lib/solmate/src/tokens/ERC721.sol

            _balanceOf[to]++;

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L101

        File: src/utils/token/PagesERC721.sol

            _balanceOf[to]++;

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L117

        File: src/utils/token/PagesERC721.sol

            _balanceOf[to]++;

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L181

        File: src/Pages.sol

            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L251

        File: src/utils/GobblerReserve.sol

            for (uint256 i = 0; i < ids.length; i++) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37

        File: lib/solmate/src/tokens/ERC721.sol

            _balanceOf[from]--;

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L99

        File: lib/solmate/src/tokens/ERC721.sol

            _balanceOf[owner]--;

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L179

        File: src/utils/token/PagesERC721.sol

            _balanceOf[from]--;

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L115


## Setting a variable to its default value costs unnecessary gas.

        File: src/ArtGobblers.sol

            for (uint256 i = 0; i < cost; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L432

        File: src/ArtGobblers.sol

            for (uint256 i = 0; i < numGobblers; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L592

        File: lib/solmate/src/utils/FixedPointMathLib.sol

                    z := 0

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L89

        File: src/utils/token/GobblersERC1155B.sol

            for (uint256 i = 0; i < owners.length; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L114

        File: src/utils/token/GobblersERC1155B.sol

            for (uint256 i = 0; i < amount; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L173

        File: src/utils/token/GobblersERC721.sol

            for (uint256 i = 0; i < amount; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L186

        File: src/Pages.sol

            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L251

        File: src/utils/GobblerReserve.sol

            for (uint256 i = 0; i < ids.length; i++) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37


## Array length (or variable for looping condition check) should not be looked up in every loop. Instead, use a variable to store the length before the loop starts.

        File: src/utils/token/GobblersERC1155B.sol

            for (uint256 i = 0; i < owners.length; ++i) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L114

        File: src/utils/GobblerReserve.sol

            for (uint256 i = 0; i < ids.length; i++) {

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37


## Declare errors for revert, instead of using string, to reduce gas.

        File: src/ArtGobblers.sol

                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L437

        File: src/ArtGobblers.sol

        require(from == getGobblerData[id].owner, "WRONG_FROM");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L885

        File: src/ArtGobblers.sol

        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L887

        File: src/ArtGobblers.sol

        require(
            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
        );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L891

        File: lib/solmate/src/tokens/ERC721.sol

        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L36

        File: lib/solmate/src/tokens/ERC721.sol

        require(owner != address(0), "ZERO_ADDRESS");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L40

        File: lib/solmate/src/tokens/ERC721.sol

        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L69

        File: lib/solmate/src/tokens/ERC721.sol

        require(from == _ownerOf[id], "WRONG_FROM");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L87

        File: lib/solmate/src/tokens/ERC721.sol

        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L89

        File: lib/solmate/src/tokens/ERC721.sol

        require(
            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
        );

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L93

        File: lib/solmate/src/tokens/ERC721.sol

        require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L122

        File: lib/solmate/src/tokens/ERC721.sol

        require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L138

        File: lib/solmate/src/tokens/ERC721.sol

        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L158

        File: lib/solmate/src/tokens/ERC721.sol

        require(_ownerOf[id] == address(0), "ALREADY_MINTED");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L160

        File: lib/solmate/src/tokens/ERC721.sol

        require(owner != address(0), "NOT_MINTED");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L175

        File: lib/solmate/src/tokens/ERC721.sol

        require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, "") ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L200

        File: lib/solmate/src/tokens/ERC721.sol

        require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, data) ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L215

        File: src/utils/token/GobblersERC1155B.sol

        require(owners.length == ids.length, "LENGTH_MISMATCH");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L107

        File: src/utils/token/GobblersERC1155B.sol

            require(
                ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
                    ERC1155TokenReceiver.onERC1155Received.selector,
                "UNSAFE_RECIPIENT"
            );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L152

        File: src/utils/token/GobblersERC1155B.sol

            require(
                ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
                    ERC1155TokenReceiver.onERC1155BatchReceived.selector,
                "UNSAFE_RECIPIENT"
            );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L188

        File: lib/solmate/src/utils/SignedWadMath.sol

        require(x > 0, "UNDEFINED");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L142

        File: lib/solmate/src/auth/Owned.sol

        require(msg.sender == owner, "UNAUTHORIZED");

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L20

        File: src/utils/token/GobblersERC721.sol

        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L62

        File: src/utils/token/GobblersERC721.sol

        require(owner != address(0), "ZERO_ADDRESS");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L66

        File: src/utils/token/GobblersERC721.sol

        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L95

        File: src/utils/token/GobblersERC721.sol

        require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L125

        File: src/utils/token/GobblersERC721.sol

        require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
        );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L141

        File: src/utils/token/PagesERC721.sol

        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L55

        File: src/utils/token/PagesERC721.sol

        require(owner != address(0), "ZERO_ADDRESS");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L59

        File: src/utils/token/PagesERC721.sol

        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L85

        File: src/utils/token/PagesERC721.sol

        require(from == _ownerOf[id], "WRONG_FROM");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L103

        File: src/utils/token/PagesERC721.sol

        require(to != address(0), "INVALID_RECIPIENT");

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L105

        File: src/utils/token/PagesERC721.sol

        require(
            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
        );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L109

        File: src/utils/token/PagesERC721.sol

            require(
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
            );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L138

        File: src/utils/token/PagesERC721.sol

            require(
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
            );

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L154

        File: lib/VRGDAs/src/VRGDA.sol

        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");

https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L32


## Hardcode hash values instead of calculate keccak256() in runtime, to reduce gas.

        File: script/deploy/DeployRinkeby.s.sol

            keccak256(abi.encodePacked(root)),

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/script/deploy/DeployRinkeby.s.sol#L22


## Cache array/mapping items as local variables instead of access them repeatedly

        File: src/ArtGobblers.sol

            getUserData[msg.sender].lastBalance = uint128(gooBalance(msg.sender)); // Checkpoint balance.
            getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.
            // We subtract the amount of gobblers burned, and then add 1 to factor in the new legendary.
            getUserData[msg.sender].gobblersOwned = uint32(getUserData[msg.sender].gobblersOwned - cost + 1);

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L454-L458

        File: src/ArtGobblers.sol

                getUserData[currentIdOwner].lastBalance = uint128(gooBalance(currentIdOwner));
                getUserData[currentIdOwner].lastTimestamp = uint64(block.timestamp);
                getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L660-L662

        File: src/ArtGobblers.sol

            getUserData[user].emissionMultiple,
            getUserData[user].lastBalance,
            uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L761-L763

        File: src/ArtGobblers.sol

        getUserData[user].lastBalance = uint128(updatedBalance);
        getUserData[user].lastTimestamp = uint64(block.timestamp);

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L825-L826

        File: src/ArtGobblers.sol

            getUserData[from].lastBalance = uint128(gooBalance(from));
            getUserData[from].lastTimestamp = uint64(block.timestamp);
            getUserData[from].emissionMultiple -= emissionMultiple;
            getUserData[from].gobblersOwned -= 1;

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L903-L906

        File: src/ArtGobblers.sol

            getUserData[to].lastBalance = uint128(gooBalance(to));
            getUserData[to].lastTimestamp = uint64(block.timestamp);
            getUserData[to].emissionMultiple += emissionMultiple;
            getUserData[to].gobblersOwned += 1;

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L910-L913

        File: src/ArtGobblers.sol

        if (hasClaimedMintlistGobbler[msg.sender]) revert AlreadyClaimed();

        // If the user's proof is invalid, revert.
        if (!MerkleProofLib.verify(proof, merkleRoot, keccak256(abi.encodePacked(msg.sender)))) revert InvalidProof();

        hasClaimedMintlistGobbler[msg.sender] = true;

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L344-L349








