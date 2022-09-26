**lib/solmate/src/tokens/ERC721.sol**
- The contract does not comply with the EIP 721 standard, therefore, in order not to generate confusion, it should have another name. Among the modifications are: changes in the names of the inputs and functions such as balanceOf() and ownerOf() that are public when in the ERC721 standard they are external.

- The functions ownerOf() and balanceOf() are public but they are not used in any function of the contract, also if the contract that inherits this abstract contract wants to consult these two data, it would be less expensive directly doing it in the variables in storage and not through of a function


**lib/VRGDAs/src/LogisticToLinearVRGDA.sol**
- L6 - It is not necessary to import VRGDA, since it is not used in the entire contract, this generates unnecessary gas costs in the deploy.


**lib/goo-issuance/src/LibGOO.sol**
- The library in general does not return any value that is useful to obtain something, since the only function it has receives an input and returns it, but first it executes code to define variables that are never used.



