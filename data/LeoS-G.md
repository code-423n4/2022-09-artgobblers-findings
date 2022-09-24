## [G-01] Unnecessary public constant
Declaring a private constant is cheaper than a public one. In some case, a constant can be declared as private to save gas. It is the case if the constant don't need to be called outside the contract. A user could still read the value directly in the code instead of calling it, if needed.

3 instances:

 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112-L118

Consider changing those constants to private. (The code still pass all the test with these changes.)

With those changes, these evolutions in gas average report can be observe:

    ArtGobblers: Deployment: 4033627 -> 4018610 (-15017)
    .BASE_URI: 4755 ->  4733 (-22)
    .FIRST_LEGENDARY_GOBBLER_ID: 452 -> 407 (-45)
    .RESERVED_SUPPLY: 472 -> 494 (+22)
    .acceptRandomSeed: 1769 -> 1791 (+22)
    .addGoo: 26198 -> 26220 (+2)
    .balanceOf: 668 -> 646 (-22)
    .burnGooForPages: 6040 -> 5973 (-67)
    .getCopiesOfArtGobbledByGobbler: 771 -> 793 (+22)
    .getTargetSaleTime: 866 -> 844 (-22)
    .getUserEmissionMultiple: 801 -> 735 (-66)
    .getVRGDAPrice: 1677 -> 1721 (+44)
    .gobble: 22003 -> 21972 (-31)
    .gobblerPrice: 1640 -> 1662 (+22)
    .gobblerRevealsData: 1151 -> 1084 (-67)
    .gooBalance: 2704 -> 2771 (+67)
    .legendaryGobblerPrice: 1646 -> 1624 (-22)
    .mintFromGoo: 30201 -> 30180 (-21)
    .mintLegendaryGobbler: 72774 -> 72796 (+22)
    .onERC1155Received: 860 ->  838 (-22)
    randProvider: 1426 -> 1448 (+22)
    requestRandomSeed: 47286 -> 47264 (-22)
    revealGobblers: 525805 -> 525783 (-22)
    targetPrice: 262 -> 328 (+66)
    upgradeRandProvider: 4804 -> 4868 (+64)

*Most of the evolutions compensate each other but in total, taking into account the number of calls, this configuration is more interesting.*

## [G-02] `external` function for the owner can be marked as `payable`.
If a function is guaranteed to revert when called by a normal user, this function can be marked as `payable` to avoid the check to know if a payment is provided.

2 instances:

 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L560
 - https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L34

Consider adding `payable` keyword.

With those changes, these evolutions in gas average report can be observe:

    ArtGobblers: Deployment :  4033627 -> 4197646 (+164019)
    ArtGobblers: upgradeRandProvider: 4804 -> 4780 (-24)
    GobblerReserve: Deployment: 251133 ->  256339 (+5206)
    GobblerReserve: withdraw: 25227 -> 25203 (-24)