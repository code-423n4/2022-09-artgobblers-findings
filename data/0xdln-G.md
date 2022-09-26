## G01 Don't Initialize Variables with Default Value

#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {
2022-09-artgobblers/src/ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```

## G02 `++i`/`i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for`- and `while`-loops

#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::432 => for (uint256 i = 0; i < cost; ++i) {
2022-09-artgobblers/src/ArtGobblers.sol::592 => for (uint256 i = 0; i < numGobblers; ++i) {
2022-09-artgobblers/src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::173 => for (uint256 i = 0; i < amount; ++i) {
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::186 => for (uint256 i = 0; i < amount; ++i) {
```

## G03 `++i` costs less gas than `i++`, especially when it's used in for-loops (`--i`/`i--` too)

```
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
```

### G04 Cache Array Length Outside of Loop

#### Findings:
```
2022-09-artgobblers/src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
```

## G05 Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version `0.8.4`.

#### Findings:
```
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::107 => require(owners.length == ids.length, "LENGTH_MISMATCH");
solmate/src/utils/SignedWadMath.sol::142 => solmate/src/utils/SignedWadMath.sol::142
```