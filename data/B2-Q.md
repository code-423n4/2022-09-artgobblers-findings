## Missing Zero address check for immutable variables

Missing zero address check can lead to unintended issues, which may cause re-deployment of the contract.
 
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L50

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L51

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L181

## Use of Block.timestamp
    
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers, locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

Navigate to the following contracts.

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L341

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L399

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L455

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L513

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L661

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L763

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L826

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L904

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L911

## public functions not called by the contract should be declared external instead
Contracts are allowed to override their parents’ functions and change the visibility from external to public

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L79

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L124

## Event is missing indexed fields
Each event should use three indexed fields if there are three or more fields

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L232

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L233

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L234

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L240

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L134

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L136

## Avoid floating pragma
Use specific compilers in the pragma. Compilers should be locked to a particular version

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol


