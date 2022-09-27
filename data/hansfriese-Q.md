## [N-01] Use two-phase ownership transfers
- https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L39
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L83
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L12

`ArtGobblers` and `GobblerReserve` contracts inherite the `Owned` contract and use `setOwner()` function without any two-phase ownership transfers.

## [N-02] Immutable addresses should be 0-checked
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L314-L317
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L179-L181
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L24
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/rand/ChainlinkV1RandProvider.sol#L55

## [N-03] Event is missing indexed fields
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L233-L234
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L240
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L134-L136
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L17
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L29
- https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L18

Each event should use three indexed fields if there are three or more fields