**ArtGobblers**
- L399/401 - When a variable is only used once, it is not necessary to create a variable, the operation where it is used can be done directly.

- L432/592 - It is not necessary to set a variable with its default value, since when creating the instance it is set with that parameter. This generates an extra gas expense.


**PagesERC721**
- L115/117/181 - It is less expensive to make --variable than to make variable--, this applies equally to ++.


**GobblersERC1155B**
- L107/109/114 - When a variable is used more than once it is recommended to create a variable in memory with that value, in this case that happens with the length of an array, it is better to create a variable and not consult it multiple times.

- L114/173 - It is not necessary to set a variable with its default value, since when creating the instance it is set with that parameter. This generates an extra gas expense.


**Pages**
- L222/227 - When a variable is only used once, it is not necessary to create a variable, the operation where it is used can be done directly.

- L251 - It is not necessary to set a variable with its default value, since when creating the instance it is set with that parameter. This generates an extra gas expense.

- L251 - It is less expensive to make --variable than to make variable--, this applies equally to ++.


**GobblerReserve**
- L37 - When a variable is only used once, it is not necessary to create a variable, the operation where it is used can be done directly.

- L37 - When a variable is used more than once it is recommended to create a variable in memory with that value, in this case that happens with the length of an array, it is better to create a variable and not consult it multiple times.

- L37 - It is less expensive to make --variable than to make variable--, this applies equally to ++.


**lib/solmate/src/tokens/ERC721.sol**
- L99/101/164/179 - It is less expensive to make --variable than to make variable--, this applies equally to ++.


**lib/VRGDAs/src/VRGDA.sol**
- L29/32 - Instead of using the variable call in storage: decayConstant twice, it is less expensive to create a variable in memory, validate that one and then set it, like so:
	int256 _decayConstant = wadLn(1e18 - _priceDecayPercent);
        require(_decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
        decayConstant = _decayConstant;


**lib/solmate/src/auth/Owned.sol**
- L20 - The require can be modified by ifs and custom errors, this would reduce the gas cost of the validation.
