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

# ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 (SAME FOR --I VS I-- OR I -= 1)

_A common gas optimization which sometimes it miss to look up_


There are some `i++` being used everywhere across contracts.

Pre-increments and pre-decrements are cheaper.

Increment:

`i += 1` is the most expensive form
`i++` costs 6 gas less than `i += 1`
`++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

Decrement:

`i -= 1` is the most expensive form
`i--` costs 11 gas less than `i -= 1`
`--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)
Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name post-increment: