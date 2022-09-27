ArtGobblers Gas Report

### G-01 Cache Array Length Outside of Loop

When an array's length is specified in a for loop, it is calculated on each iteration. Storing the length in a temporary variable and using that will save gas after the first loop iteration.

### Instances found

src/utils/token/GobblersERC1155B.sol::114 => for (uint256 i = 0; i < owners.length; ++i) {
src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {

### G-02 Use Predecrement instead of Postdecrement in simple loops

When incrementing an integer, ++i (return value then increment) is more gas efficient than i++ (increment then return value). When using a counter and ignoring the value, ++i is more optimal for most use cases.

### Instances found

src/Pages.sol::251 => for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
src/utils/GobblerReserve.sol::37 => for (uint256 i = 0; i < ids.length; i++) {