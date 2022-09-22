# Replace `require(condition, "ERROR")` by `if(!condition) revert Error();` pattern for gas savings.

List of affected assets in scope:

```
$ grep -rn 'require\s*(' src/ArtGobblers.sol lib/solmate/src/utils/FixedPointMathLib.sol lib/solmate/src/tokens/ERC721.sol src/utils/token/GobblersERC1155B.sol lib/solmate/src/utils/SignedWadMath.sol src/utils/token/GobblersERC721.sol src/utils/token/PagesERC721.sol script/deploy/DeployBase.s.sol src/Pages.sol src/utils/rand/ChainlinkV1RandProvider.sol lib/VRGDAs/src/LogisticToLinearVRGDA.sol lib/solmate/src/utils/MerkleProofLib.sol script/deploy/DeployRinkeby.s.sol src/Goo.sol lib/VRGDAs/src/LogisticVRGDA.sol lib/solmate/src/utils/LibString.sol lib/VRGDAs/src/VRGDA.sol lib/goo-issuance/src/LibGOO.sol lib/solmate/src/auth/Owned.sol lib/VRGDAs/src/LinearVRGDA.sol src/utils/GobblerReserve.sol src/utils/rand/RandProvider.sol

src/ArtGobblers.sol:437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
src/ArtGobblers.sol:885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
src/ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
src/ArtGobblers.sol:889:        require(
lib/solmate/src/utils/FixedPointMathLib.sol:43:            // Equivalent to require(denominator != 0 && (x == 0 || (x * y) / x == y))
lib/solmate/src/utils/FixedPointMathLib.sol:62:            // Equivalent to require(denominator != 0 && (x == 0 || (x * y) / x == y))
lib/solmate/src/tokens/ERC721.sol:36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:40:        require(owner != address(0), "ZERO_ADDRESS");
lib/solmate/src/tokens/ERC721.sol:69:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
lib/solmate/src/tokens/ERC721.sol:87:        require(from == _ownerOf[id], "WRONG_FROM");
lib/solmate/src/tokens/ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:91:        require(
lib/solmate/src/tokens/ERC721.sol:118:        require(
lib/solmate/src/tokens/ERC721.sol:134:        require(
lib/solmate/src/tokens/ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
lib/solmate/src/tokens/ERC721.sol:160:        require(_ownerOf[id] == address(0), "ALREADY_MINTED");
lib/solmate/src/tokens/ERC721.sol:175:        require(owner != address(0), "NOT_MINTED");
lib/solmate/src/tokens/ERC721.sol:196:        require(
lib/solmate/src/tokens/ERC721.sol:211:        require(
src/utils/token/GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
src/utils/token/GobblersERC1155B.sol:149:            require(
src/utils/token/GobblersERC1155B.sol:185:            require(
lib/solmate/src/utils/SignedWadMath.sol:57:        // Equivalent to require(x == 0 || (x * y) / x == y)
lib/solmate/src/utils/SignedWadMath.sol:72:        // Equivalent to require(y != 0 && ((x * 1e18) / 1e18 == x))
lib/solmate/src/utils/SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
src/utils/token/GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
src/utils/token/GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
src/utils/token/GobblersERC721.sol:121:        require(
src/utils/token/GobblersERC721.sol:137:        require(
src/utils/token/PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
src/utils/token/PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
src/utils/token/PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
src/utils/token/PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
src/utils/token/PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
src/utils/token/PagesERC721.sol:107:        require(
src/utils/token/PagesERC721.sol:135:            require(
src/utils/token/PagesERC721.sol:151:            require(
lib/VRGDAs/src/VRGDA.sol:32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
lib/solmate/src/auth/Owned.sol:20:        require(msg.sender == owner, "UNAUTHORIZED");

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