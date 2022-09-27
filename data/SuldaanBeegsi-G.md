First issue: Change order - for better slot allocation - ArtGobblers.sol
-- line 723-727

function gobble(
        uint256 gobblerId,
        address nft,
        uint256 id,
        bool isERC1155

Change the order the order to:
function gobble(
        uint256 gobblerId,
        uint256 id,
	address nft,
        bool isERC1155

Second issue: General can be implimated for the line above 

Packing Booleans:
Bools are stored as unit8 has default, but only 1 bit is used. For better slot allocation when dealing with multi opal bool values. Packing booleans , where you create a function that packs and unpacks the booleans into a single variable. 

Proof of consept : https://ethereum.stackexchange.com/questions/77099/efficient-bit-packing

PS. 1 Scroll a bit down to se the implementation. 

Third issue: 
 
line 251 - Pages.sol

for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

issue is use of i++, should be ++i to save gas.

proof of concept: https://ethereum.stackexchange.com/questions/133161/why-does-i-cost-less-gas-than-i



