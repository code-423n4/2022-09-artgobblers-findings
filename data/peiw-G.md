-  It costs more gas to initialize NON-CONSTANT/NON-IMMUTABLE variables to zero than to let the default of zero be applied.
	- Instance: 
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592

- `++i (--)` costs less gas than `i++ (--)`, especially when in FOR loops.
	- `++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration).
	- Instance: 
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L115
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L115
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L181



- `x += y` cost more gas than `x = x + y` for state variables.
	- Instance:
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L464
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L906
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L913
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L184



- Use custom errors rather than `REVERT()/REQUIRE()` strings to save gas.
	- Custom errors are available from solidity version 0.8.4. Custom errors save \~50 gas each time, they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas.
	- Instances:
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L696
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887



- Usage of `UINTS/INTS` smaller than 32 bytes incurs overhead.
	-  When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed.
	- Instances:
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L899
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L167
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L107



- `<ARRAY>.length` should not be looked up in every loop of a for-loop.
	- Instance: 
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114


- Using `PRIVATE` rather than `PUBLIC` for `CONSTANT`, saves gas.
	- Instances:
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115
		- https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118