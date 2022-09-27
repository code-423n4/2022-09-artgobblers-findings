1. Missing checks for address(0x0) when assigning values to address state variables

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters. Impact: Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316-L318
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L179-L182
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83-L84

