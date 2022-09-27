# QA Report
* The current implementation of the `ArtGobblers.claimGobbler()` function allows each user to claim at most 1 gobbler. It will be better to extend this functionality to allow users to claim an amount of gobblers that will be specified in the merkle tree.
* The `mintFromGoo` in both of the `ArtGobblers` and the `Pages` contracts allows the users to pay goo from his virtual balance or from his actual goo balance. This functionality can be improved by allowing a user to use both if this balances - this will save the users from first adding goo to their virtual balance and then using it, or withdrawing goo from their virtual balance and then using their non virtual balance. This will of course relates to the case where the user doesn't have enough balance to pay in both his virtual and non-virtual balances.
    ```sol
    // From ArtGobblers
    useVirtualBalance
        ? updateUserGooBalance(msg.sender, currentPrice, GooBalanceUpdateType.DECREASE)
        : goo.burnForGobblers(msg.sender, currentPrice);

    // From Pages
    useVirtualBalance
        ? artGobblers.burnGooForPages(msg.sender, currentPrice)
        : goo.burnForPages(msg.sender, currentPrice);
    ```
* The `legendaryGobblerPrice` will return a value and won't revert even after all the legendary gobblers will be minted. This can confuse innocent users into thinking another legendary gobbler will be minted/is open for auction. This will happen in 2 unwanted cases - when all the legendary gobblers will be sold, and the value of `numMintedFromGoo` will be either 6391 or 6392. 6392 is the maximum number of gobblers that can be sold (i.e. `MAX_MINTABLE`), and when the `numMintedFromGoo` value will be one of the two I mentioned before, the `legendaryGobblerPrice` function will return unwanted values.
    ```sol
    // For numMintedFromGoo = 6391, the return value will be 
    FixedPointMathLib.unsafeDivUp(startPrice * 6391, 6392) = 99.98% of the start price
    // For numMintedFromGoo = 6392, the return value will be 
    FixedPointMathLib.unsafeDivUp(startPrice * 6392, 6392) = the start price
    ```
* Consider adding a check to the `acceptRandomSeed` function to verify that the contract is actually waiting for a seed. If somehow the chainlink oracle will call the function multiple times, this can be harmful to the contract.
* The `gobble` function doesn't allow an operator to feed the gobbler with an NFT. The current implementation reverts if `owner != msg.sender`, but it shouldn't revert if the `msg.sender` is an allowed operator, i.e. `isApprovedForAll[owner][msg.sender] == true`.
* The `updateUserGooBalance` is an internal function but it doesn't have an underscore in the beginning of its name.
* When a gobbler is burned to buy a legendary gobbler, all the data in the `getCopiesOfArtGobbledByGobbler` mapping becomes irrelevant. Consider saving the data differently to allow moving it to the legendary gobbler when it is burned.
* Users can spam the logs by providing 0 as the `numGobblersEach` argument to the `ArtGobblers.mintReservedGobblers()` function. This can also happen by providing `numPages = 0` to the `Pages.mintCommunityPages()` function
* Missing zero address checks - the constructors don't validate the input, i.e. don't check that the given addresses are not zero.
* The `ArtGobblers` contract is not an ERC721Receiver. It won't support gobbling of NFTs that their transferFrom function does the `onERC721Received` callback.
* Consider using the 2-step ownership transfer in order to make sure the new owner is reachable. That way mistakes like wrong address given as the new owner will be prevented.

