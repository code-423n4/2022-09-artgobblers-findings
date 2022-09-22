### Missing 0 address check

Missing 0-address check, can lead to unintended issues, which may cause re-deployment of the contract

*There are 8 instances of this issue:*

```
File: src/ArtGobblers.sol

316:        team = _team;
317:        community = _community;

File: src/Goo.sol

83:        artGobblers = _artGobblers;
84:        pages = _pages;

File: src/Pages.sol

181:        community = _community;

File: src/utils/rand/ChainlinkV1RandProvider.sol

50:         address _vrfCoordinator,
51:          address _linkToken,

File: src/utils/GobblerReserve.sol

23:         constructor(ArtGobblers _artGobblers, address _owner) Owned(_owner) {       // address _owner

```


### Dependence on block.timestamp

Use of block attributes like block.timestamp are risky, as they can be manipulated by miners

*There are 7 instances of this issue:*

```
File: src/ArtGobblers.sol

341:        if (mintStart > block.timestamp) revert MintStartPending();
399:        uint256 timeSinceStart = block.timestamp - mintStart;
455:        getUserData[msg.sender].lastTimestamp = uint64(block.timestamp); // Store time alongside it.
513:         if (block.timestamp < nextRevealTimestamp) revert RequestTooEarly();
763:        uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
826:        getUserData[user].lastTimestamp = uint64(block.timestamp);

File: src/Pages.sol

222:        uint256 timeSinceStart = block.timestamp - mintStart;
```

### Use custom error

Custom errors from solidity compiler 0.8.4, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.

*There are 8 instances of this issue:*

```
File: src/ArtGobblers.sol

437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
696:               if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.
705:                if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");
711:                 revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.
885:               require(from == getGobblerData[id].owner, "WRONG_FROM");
887:               require(to != address(0), "INVALID_RECIPIENT");

889:        require(
890:           msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
891:            "NOT_AUTHORIZED"
892:         );

File: src/Pages.sol

266:        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```

### Update to latest chainlink VRF v2

The randomprovider used should be updated to the latest version , i.e v2

```
    /// @notice The address of a randomness provider. This provider will initially be
    /// a wrapper around Chainlink VRF v1, but can be changed in case it is fully sunset.
    RandProvider public randProvider;
```

### Avoid floating pragma

Use specific compilers in the pragma. Compilers should be locked to a particular version.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol