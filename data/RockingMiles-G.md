## Unnecessary equals boolean


Boolean variables can be checked within conditionals directly without the use of equality operators to true/false.

### Code instance:

        PagesERC721.sol, 73: if (operator == address(artGobblers)) return true; // Skip approvals for the ArtGobblers contract.



## State variables that could be set immutable

In the following files there are state variables that could be set immutable to save gas. 

### Code instances:

        symbol in GobblersERC721.sol
        name in GobblersERC721.sol



## Change transferFrom to transfer

'transferFrom(address(this), *, **)' could be replaced by the following more gas efficient 'transfer(*, **)'
               This replacement is more gas efficient and improves the code quality.

### Code instance:

        GobblerReserve.sol, 38 : artGobblers.transferFrom(address(this), to, ids[i]);



## Use bytes32 instead of string to save gas whenever possible


    Use bytes32 instead of string to save gas whenever possible.
    String is a dynamic data structure and therefore is more gas consuming then bytes32.

    
### Code instances:

        DeployRinkeby.s.sol (L13), string public constant gobblerBaseUri = "https:
        DeployRinkeby.s.sol (L15), string public constant pagesBaseUri = "https: 
        DeployRinkeby.s.sol (L14), string public constant gobblerUnrevealedUri = "https:



## Use != 0 instead of > 0


Using != 0 is slightly cheaper than > 0. (see https://github.com/code-423n4/2021-12-maple-findings/issues/75 for similar issue)


### Code instance:

        Pages.sol, 248: change 'balance > 0' to 'balance != 0'
