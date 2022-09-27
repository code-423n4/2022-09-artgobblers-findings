# Use custom errors rather than `revert()`/`require()` strings to save deployment gas

Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L437
```
        require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885-L892

```
        require(from == getGobblerData[id].owner, "WRONG_FROM");

        require(to != address(0), "INVALID_RECIPIENT");

        require(
            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
        );
```

there are some more `require()` function inside `GobblersERC721.sol`, `GobblersERC1155B.sol`, and `PagesERC721.sol`