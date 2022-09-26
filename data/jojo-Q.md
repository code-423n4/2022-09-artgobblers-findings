## Missing checks for address`(0x0)` when assigning values to address state variables in constructors

### Affected Contracts

- GobblerReserve.sol#24
- PagesERC721.sol#44
- Pages.sol#180-181
- Goo.sol#83-84
- ArtGoobles.sol#314-318

### Description

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters.

### Impact

Accidental use of zero addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

### Mitigation

In the functions mentioned above, add the following `require` statement before setting variables:

```solidity
require(var != address(0), "var is the zero address");
```