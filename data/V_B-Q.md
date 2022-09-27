### 1. Suggestion: two-step ownership transition in Owned contract

It is reasonable to add a two-step ownership transition: in the first stage owner proposes to transfer ownership, and in the second new owner accepts ownership by calling a special function.

### 2. changing bool isERC1155 to enum

There is `gobble` function in `ArtGobblers` contract. It accepts `bool isERC1155` as an input parameter, which indicates whether the work of art is an ERC1155 or ERC721 token. It will be better to use `enum` instead of bool variable in this case. This is so because the boolean variable name and description state that it only gives information on whether is it an ERC1155 token or not, but does not provide any reasonable info about bellonging to other possible token standards.