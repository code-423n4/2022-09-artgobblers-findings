### Missing `@param` statement
___
[ArtGobblers.sol: L278-300](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L278-L300)
```solidity
    /// @notice Sets VRGDA parameters, mint config, relevant addresses, and URIs.
    /// @param _merkleRoot Merkle root of mint mintlist.
    /// @param _mintStart Timestamp for the start of the VRGDA mint.
    /// @param _goo Address of the Goo contract.
    /// @param _team Address of the team reserve.
    /// @param _community Address of the community reserve.
    /// @param _randProvider Address of the randomness provider.
    /// @param _baseUri Base URI for revealed gobblers.
    /// @param _unrevealedUri URI for unrevealed gobblers.
    constructor(
        // Mint config:
        bytes32 _merkleRoot,
        uint256 _mintStart,
        // Addresses:
        Goo _goo,
        Pages _pages,
        address _team,
        address _community,
        RandProvider _randProvider,
        // URIs:
        string memory _baseUri,
        string memory _unrevealedUri
    )
```
`@param` statement is missing for `_pages` 
___
___

### Long single line comments 
In theory, comments over 79 characters should wrap using multi-line comment syntax. Even if somewhat longer comments are acceptable and a scroll bar is provided, very long comments can interfere with readability. Most of the long comments in `Art Gobblers` are very informative and do wrap. However their readability could be improved by the indentation of the second and any subsequent lines. Below are five examples of lines that may or may not already wrap, but in any case could benefit from indentation:
___
[ArtGobblers.sol: L474-476](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L473-L476)
```solidity
    /// @notice Calculate the legendary gobbler price in terms of gobblers, according to a linear decay function.
    /// @dev The price of a legendary gobbler decays as gobblers are minted. The first legendary auction begins when
    /// 1 LEGENDARY_AUCTION_INTERVAL worth of gobblers are minted, and the price decays linearly while the next interval of
    /// gobblers are minted. Every time an additional interval is minted, a new auction begins until all legendaries have been sold.
```
Suggestion:
```solidity
    /// @notice Calculate the legendary gobbler price in terms of gobblers, 
    ///   according to a linear decay function.
    /// @dev The price of a legendary gobbler decays as gobblers are minted. 
    ///   The first legendary auction begins when 1 LEGENDARY_AUCTION_INTERVAL 
    ///   worth of gobblers are minted, and the price decays linearly while the
    ///   next interval of gobblers are minted. Every time an additional interval
    ///   is minted, a new auction begins until all legendaries have been sold.
```
___
[ArtGobblers.sol: L485-486](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L485-L486)
```solidity
            // The number of gobblers minted at the start of the auction is computed by multiplying the # of
            // intervals that must pass before the next auction begins by the number of gobblers in each interval.
```
Suggestion:
```solidity
            // The number of gobblers minted at the start of the auction 
            //   is computed by multiplying the # of intervals that must pass before
            //   the next auction begins by the number of gobblers in each interval.
```
___
[ArtGobblers.sol: L489](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L489)
```solidity
            // If not enough gobblers have been minted to start the auction yet, return how many need to be minted.
```
Suggestion:
```solidity
            // If not enough gobblers have been minted to start the auction yet, 
            //   return how many need to be minted.
```
___
[GobblersERC1155B.sol: L6](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L6)
```solidity
/// @notice ERC1155B implementation optimized for ArtGobblers by using the ownerOf storage slot to store attribute data.
```
Suggestion:
```solidity
/// @notice ERC1155B implementation optimized for ArtGobblers by using 
///   the ownerOf storage slot to store attribute data.
```
___
[LibGOO.sol: L13-16](https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L13-L16)
```solidity
    /// @notice Compute goo balance based on emission multiple, last balance, and time elapsed.
    /// @param emissionMultiple The multiple on emissions to consider when computing the balance.
    /// @param lastBalanceWad The last checkpointed balance to apply the emission multiple over time to, scaled by 1e18.
    /// @param timeElapsedWad The time elapsed since the last checkpoint, scaled by 1e18.
```
Suggestion:
```solidity
    /// @notice Compute goo balance based on emission multiple, 
    ///   last balance, and time elapsed.
    /// @param emissionMultiple The multiple on emissions to consider 
    ///   when computing the balance.
    /// @param lastBalanceWad The last checkpointed balance to apply the
    ///   emission multiple over time to, scaled by 1e18.
    /// @param timeElapsedWad The time elapsed since the last checkpoint, 
    ///   scaled by 1e18.
```
___
___

### Indications of unfinished work or open items should be addressed
Language referring to open items should be addressed and modified or removed. Below are two instances:
___
[ArtGobblers.sol: L378-381](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L378-L381)
```solidity
        // price, either from virtual balance or ERC20 balance.
        useVirtualBalance
            ? updateUserGooBalance(msg.sender, currentPrice, GooBalanceUpdateType.DECREASE)
            : goo.burnForGobblers(msg.sender, currentPrice);
```
___
[ArtGobblers.sol: L746-748](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L746-L748)
```solidity
        isERC1155
            ? ERC1155(nft).safeTransferFrom(msg.sender, address(this), id, 1, "")
            : ERC721(nft).transferFrom(msg.sender, address(this), id);
```
___
___
 