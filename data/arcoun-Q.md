# QA Report

# Low Risk Issues

## 1. ArtGobblers is not EIP-1155 compliant as an ERC1155TokenReceiver

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

The `ArtGobblers` contract can receive ERC1155 tokens using the safe receiver functions inherited from `ERC1155TokenReceiver` but it does not return the related interface in the `supportsInterface` method (ERC165).

EIP-1155 states that:
> Smart contracts MUST implement the ERC-165 `supportsInterface` function and signify support for the `ERC1155TokenReceiver` interface to accept transfers.

The `supportsInterface` method is currently inherited from `ERC721` which does not support ERC1155. The `ArtGobblers` contract should implement its own `supportsInterface` method to support both ERC721 and ERC1155TokenReceiver interfaces.

## 2. ArtGobblers are not ERC1155 NFTs

https://github.com/code-423n4/2022-09-artgobblers/blob/main/README.md?plain=1#L213
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L83

The README.md file states that Art Gobblers are ERC1155 NFTs but they are not. The ArtGobblers contract implements the token as an ERC721 token which can receive ERC1155 NFTs.

# Non-critical Issues

## 1. Use 1e18 instead of 1000000000000000000

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol

Use `1e18` instead of `1000000000000000000` to increase readability.
