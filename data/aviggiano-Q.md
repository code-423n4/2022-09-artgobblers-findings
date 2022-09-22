# 1. Add approval verification on NFT ownership checks

The function `gobble` of `ArtGobblers` contains logic to feed a gobbler a work of art, requiring the `gobblerId` to belong to the `msg.sender`. 
In order to make this function more generic and fully compliant with EIP-721 and EIP-1155, it may be worth it to add also "approval" checks `getApproved` and `isApprovedForAll`:

Before:
```
src/ArtGobblers.sol-732-        // The caller must own the gobbler they're feeding.
src/ArtGobblers.sol:733:        if (owner != msg.sender) revert OwnerMismatch(owner);
```

After
```
src/ArtGobblers.sol-732-        // The caller must own the gobbler they're feeding or have approved the sender
src/ArtGobblers.sol:733:        if(owner != msg.sender || !isApprovedForAll(owner, msg.sender) || !isERC1155 && getApproved(tokenId) != msg.sender) revert OwnerMismatch(owner);
```

# 2. Use explicit 256-bit integer type whenever possible

Before
```
src/ArtGobblers.sol:763:            uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
```

After
```
src/ArtGobblers.sol:763:            uint256(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
```

# 3. Remove duplicate word in documentation

Before
```
src/ArtGobblers.sol:451:            // Update the user's user data struct in one big batch. We add burnedMultipleTotal to their
```

After
```
src/ArtGobblers.sol:451:            // Update the user's data struct in one big batch. We add burnedMultipleTotal to their
```
