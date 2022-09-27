# Gas Optimizations 

## Executive Summary
Codebase looks mature and well thought-out. Documentation is complete and testing coverage is high.
Gas Optimized very well, Hard to top the optimzooors.
Below are listed a few gas savings suggestions.


## 1. Don't Initialize Variables with Default Value
Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

## POC 
``` 
  src\ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {
  src\ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {
  src\Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
  src\utils\GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
  src\utils\token\GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
  src\utils\token\GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {
  src\utils\token\GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```

## 2. Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

## POC 
``` 
src\utils\GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
src\utils\token\GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
```