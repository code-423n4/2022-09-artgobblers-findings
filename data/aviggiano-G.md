# Replace `require(condition, "ERROR")` by `if(!condition) revert Error();` pattern for gas savings.

List of affected pieces of code:

```
$ grep -rn 'require\s*(' src script lib/VRGDAs/src lib/goo-issuance/src
src/ArtGobblers.sol:439:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
src/ArtGobblers.sol:887:        require(from == getGobblerData[id].owner, "WRONG_FROM");
src/ArtGobblers.sol:889:        require(to != address(0), "INVALID_RECIPIENT");
src/ArtGobblers.sol:891:        require(
src/utils/token/GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
src/utils/token/GobblersERC1155B.sol:149:            require(
src/utils/token/GobblersERC1155B.sol:185:            require(
src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
src/utils/token/PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
src/utils/token/PagesERC721.sol:107:        require(
src/utils/token/PagesERC721.sol:135:            require(
src/utils/token/PagesERC721.sol:151:            require(
src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
src/utils/token/GobblersERC721.sol:121:        require(
src/utils/token/GobblersERC721.sol:137:        require(
lib/VRGDAs/src/examples/LinearNFT.sol:50:            require(msg.value >= price, "UNDERPAID"); // Don't allow underpaying.
lib/VRGDAs/src/examples/LogisticNFT.sol:61:            require(msg.value >= price, "UNDERPAID"); // Don't allow underpaying.
lib/VRGDAs/src/examples/LogisticToLinearNFT.sol:68:            require(msg.value >= price, "UNDERPAID"); // Don't allow underpaying.
lib/VRGDAs/src/VRGDA.sol:32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
```

E.g. for the first occurrence
### Before
```
src/ArtGobblers.sol:439:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
```
```
forge test --match-test testMintLegendaryGobblerWithUnownedId --gas-report
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                            ┆ min             ┆ avg     ┆ median  ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ mintLegendaryGobbler                     ┆ 182111          ┆ 182111  ┆ 182111  ┆ 182111  ┆ 1       │
```

### After
```
src/ArtGobblers.sol:439:                if(getGobblerData[id].owner != msg.sender) revert WrongFrom();
```
```
forge test --match-test testMintLegendaryGobblerWithUnownedId --gas-report
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                            ┆ min             ┆ avg     ┆ median  ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ mintLegendaryGobbler                     ┆ 182039          ┆ 182039  ┆ 182039  ┆ 182039  ┆ 1       │
```

The other findings are analogous.