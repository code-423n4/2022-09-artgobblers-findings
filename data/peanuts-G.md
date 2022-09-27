    ## TABLE OF CONTENTS

- [G-01] No need to explicitly initialize variables with default value
- [G-02] Division by two should use bit shifting
- [G-03] Lock floating pragma // Updating solidity version to the latest saves gas

## [G-01] No need to explicitly initialize variables with default value

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas. In the similar code above, the initialization of uint256 i = 0 can be simplified to uint256 i.

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432


## [G-02] Division by two should use bit shifting

<x> / 2 is the same as <x> >> 1. The DIV opcode uses 5 gas, whereas SHR only costs 3 gas.

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L462

## [G-03] Lock floating pragma // Updating solidity version to the latest saves gas

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

https://swcregistry.io/docs/SWC-103
