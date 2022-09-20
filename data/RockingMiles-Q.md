## Mult instead div in compares


To improve algorithm precision instead using division in comparison use multiplication in the following scenario:
        
        Instead a < b / c use a * c < b. 
    
In all of the big and trusted contracts this rule is maintained.
    
### Code instance:

        Pages.sol, 248, if (newNumMintedForCommunity > ((lastMintedPageId = currentId) + numPages) / 10) revert ReserveImbalance(); 



## Missing fee parameter validation


Some fee parameters of functions are not checked for invalid values. Validate the parameters:
        
        
### Code instances:

        ChainlinkV1RandProvider.constructor (_chainlinkFee)
        DeployBase.s.constructor (_chainlinkFee)



## Not verified input


external / public functions parameters should be validated to make sure the address is not 0.
Otherwise if not given the right input it can mistakenly lead to loss of user funds.    

### Code instances:

        GobblersERC721.sol.safeTransferFrom from
        GobblersERC721.sol.safeTransferFrom to
        GobblersERC1155B.sol.setApprovalForAll operator
        Goo.sol.burnForPages from
        PagesERC721.sol.safeTransferFrom to
        GobblerReserve.sol.withdraw to
        PagesERC721.sol.setApprovalForAll operator
        GobblersERC721.sol.approve spender
        GobblersERC721.sol.setApprovalForAll operator
        Goo.sol.burnForGobblers from
        PagesERC721.sol.approve spender
        PagesERC721.sol.transferFrom to
        Goo.sol.mintForGobblers to



## Named return issue

Users can mistakenly think that the return value is the named return, but it is actually the actualreturn statement that comes after. To know that the user needs to read the code and is confusing.
Furthermore, removing either the actual return or the named return will save gas. 

### Code instances:

        PagesERC721.sol, isApprovedForAll
        GobblersERC1155B.sol, ownerOf
        ChainlinkV1RandProvider.sol, requestRandomBytes



## Check transfer receiver is not 0 to avoid burned money


Transferring tokens to the zero address is usually prohibited to accidentally avoid "burning" tokens by sending them to an unrecoverable zero address.


### Code instances:

        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L148
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L171
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L124
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L189
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/GobblerReserve.sol#L38
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L135
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L186



## Add a timelock

To give more trust to users: functions that set key/critical variables should be put behind a timelock.

### Code instances:

        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L102
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC1155B.sol#L79
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/PagesERC721.sol#L92



## Unsafe Cast

use openzeppilin's safeCast in:
 
### Code instances:

        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/Pages.sol#L239 : unsafe cast uint128(numPages)
        https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L174 : unsafe cast uint32(amount)



## transfer return value of a general ERC20 is ignored

Need to use safeTransfer instead of transfer. As there are popular tokens, such as USDT that transfer/trasnferFrom method doesn’t return anything. The transfer return value has to be checked (as there are some other tokens that returns false instead revert), that means you must 
 1. Check the transfer return value
Another popular possibility is to add a whiteList.
Those are the appearances (solidity file, line number, actual line):

### Code instance:

        GobblerReserve.sol, 38 (withdraw), artGobblers.transferFrom(address(this), to, ids[i]);


