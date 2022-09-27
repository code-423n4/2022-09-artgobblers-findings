# Art Gobblers Contest QA Report

## Summary

The following QA issues were found during the code audit:

1. Unsafe ERC20 Operation(s) (2 instances)
2. Unspecific Compiler Version Pragma (9 instances)

Total 11 instances of 2 issues.

## 1. Unsafe ERC20 Operation(s) (2 instances)

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard. It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

```solidity
src/ArtGobblers.sol::748 => : ERC721(nft).transferFrom(msg.sender, address(this), id);

src/utils/GobblerReserve.sol::38 => artGobblers.transferFrom(address(this), to, ids[i]);
```

## 2. Unspecific Compiler Version Pragma (9 instances)

Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version.

```solidity
src/ArtGobblers.sol::2 => pragma solidity >=0.8.0;

src/Goo.sol::2 => pragma solidity >=0.8.0;

src/Pages.sol::2 => pragma solidity >=0.8.0;

src/utils/GobblerReserve.sol::2 => pragma solidity >=0.8.0;

src/utils/rand/ChainlinkV1RandProvider.sol::2 => pragma solidity >=0.8.0;

src/utils/rand/RandProvider.sol::2 => pragma solidity >=0.8.0;

src/utils/token/GobblersERC1155B.sol::2 => pragma solidity >=0.8.0;

src/utils/token/GobblersERC721.sol::2 => pragma solidity >=0.8.0;

src/utils/token/PagesERC721.sol::2 => pragma solidity >=0.8.0;
```
