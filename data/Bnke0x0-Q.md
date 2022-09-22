


### [L01] New min/max values should be checked against the current stored value

#### Impact
 the
 new min, the next update will likely have a similar value and 
immediately cause problems. The code should require that the current 
value falls within the new range
#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::44 => logisticLimit = _maxSellable + 1e18;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::51 => mintStart = _mintStart;
2022-09-artgobblers/src/ArtGobblers.sol::311 => mintStart = _mintStart;
2022-09-artgobblers/src/Pages.sol::177 => mintStart = _mintStart;
```


### [L02] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LinearVRGDA.sol::30 => perTimeUnit = _perTimeUnit;
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::47 => soldBySwitch = _soldBySwitch;
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::49 => switchTime = _switchTime;
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::51 => perTimeUnit = _perTimeUnit;
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::44 => logisticLimit = _maxSellable + 1e18;
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::47 => logisticLimitDoubled = logisticLimit * 2e18;
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::49 => timeScale = _timeScale;
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::27 => targetPrice = _targetPrice;
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::29 => decayConstant = wadLn(1e18 - _priceDecayPercent);
2022-09-artgobblers/script/deploy/DeployBase.s.sol::49 => teamColdWallet = _teamColdWallet;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::50 => merkleRoot = _merkleRoot;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::51 => mintStart = _mintStart;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::52 => vrfCoordinator = _vrfCoordinator;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::53 => linkToken = _linkToken;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::54 => chainlinkKeyHash = _chainlinkKeyHash;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::55 => chainlinkFee = _chainlinkFee;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::56 => gobblerBaseUri = _gobblerBaseUri;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::57 => gobblerUnrevealedUri = _gobblerUnrevealedUri;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::58 => pagesBaseUri = _pagesBaseUri;
2022-09-artgobblers/solmate/src/auth/Owned.sol::30 => owner = _owner;
2022-09-artgobblers/solmate/src/auth/Owned.sol::40 => owner = newOwner;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::58 => name = _name;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::59 => symbol = _symbol;
2022-09-artgobblers/src/ArtGobblers.sol::311 => mintStart = _mintStart;
2022-09-artgobblers/src/ArtGobblers.sol::312 => merkleRoot = _merkleRoot;
2022-09-artgobblers/src/ArtGobblers.sol::314 => goo = _goo;
2022-09-artgobblers/src/ArtGobblers.sol::315 => pages = _pages;
2022-09-artgobblers/src/ArtGobblers.sol::316 => team = _team;
2022-09-artgobblers/src/ArtGobblers.sol::317 => community = _community;
2022-09-artgobblers/src/ArtGobblers.sol::318 => randProvider = _randProvider;
2022-09-artgobblers/src/Goo.sol::83 => artGobblers = _artGobblers;
2022-09-artgobblers/src/Goo.sol::84 => pages = _pages;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::84 => name = _name;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::85 => symbol = _symbol;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::41 => name = _name;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::42 => symbol = _symbol;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::43 => artGobblers = _artGobblers;
```




### [L03] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`

#### Impact
Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). “Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.
#### Findings:
```
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::22 => keccak256(abi.encodePacked(root)),

```




### [L04] `_safeMint()` should be used rather than `_mint()` wherever possible

#### Impact
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### Findings:
```
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::194 => _mint(to, id);
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::209 => _mint(to, id);
2022-09-artgobblers/src/ArtGobblers.sol::356 => _mint(msg.sender, gobblerId);
2022-09-artgobblers/src/ArtGobblers.sol::389 => _mint(msg.sender, gobblerId);
2022-09-artgobblers/src/ArtGobblers.sol::469 => _mint(msg.sender, gobblerId);
2022-09-artgobblers/src/Goo.sol::102 => _mint(to, amount);
2022-09-artgobblers/src/Pages.sol::211 => _mint(msg.sender, pageId);
```




### [L05] Use Two-Step Transfer Pattern for Access Controls

#### Impact
Contracts implementing access control's, e.g. `owner`, should consider implementing a Two-Step Transfer pattern.

Otherwise it's possible that the role mistakenly transfers ownership to the wrong address, resulting in a loss of the role.
#### Findings:
```
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::46 => address owner;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::36 => address owner;
```





##### Non-Critical Issues



### [N01] Adding a return statement when the function defines a named return variable, is redundant



#### Findings:
```
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::42 => return _balanceOf[owner];
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::193 => return lastMintedId;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::61 => return _balanceOf[owner];
```




### [N02] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::45 => revert(0, 0)
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::64 => revert(0, 0)
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::116 => revert(0, 0)
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::127 => revert(0, 0)
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::142 => revert(0, 0)
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::151 => revert(0, 0)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::59 => revert(0, 0)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::74 => revert(0, 0)
```




### [N03] constants should be defined rather than using magic numbers

#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::44 => logisticLimit = _maxSellable + 1e18;
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::47 => logisticLimitDoubled = logisticLimit * 2e18;
2022-09-artgobblers/goo-issuance/src/ibGOO.sol::38 => (emissionMultiple * lastBalanceWad * 1e18).sqrt()
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::9 => address public immutable root = 0x1D18077167c1177253555e45B4b5448B11E30b4b;
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::32 => 0.1e18,
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::12 => uint256 internal constant WAD = 1e18; // The scalar of ETH and most ERC20s.
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::15 => return mulDivDown(x, y, WAD); // Equivalent to (x * y) / WAD rounded down.
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::19 => return mulDivUp(x, y, WAD); // Equivalent to (x * y) / WAD rounded up.
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::23 => return mulDivDown(x, WAD, y); // Equivalent to (x * WAD) / y rounded down.
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::27 => return mulDivUp(x, WAD, y); // Equivalent to (x * WAD) / y rounded up.
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::115 => if shr(128, x) {
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::170 => z := 181 // The "correct" value is 1, but this saves a multiplication later.
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::177 => if iszero(lt(y, 0x10000000000000000000000000000000000)) {
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::178 => y := shr(128, y)
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::181 => if iszero(lt(y, 0x1000000000000000000)) {
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::185 => if iszero(lt(y, 0x10000000000)) {
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::189 => if iszero(lt(y, 0x1000000)) {
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::210 => z := shr(18, mul(z, add(y, 65536))) // A mul() is saved from starting z at 181.
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::11 => r := mul(x, 1000000000000000000)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::21 => r := div(mul(x, 1000000000000000000), 86400)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::31 => r := div(mul(x, 86400), 1000000000000000000)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::39 => r := sdiv(mul(x, y), 1000000000000000000)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::48 => r := sdiv(mul(x, 1000000000000000000), y)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::63 => r := sdiv(r, 1000000000000000000)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::70 => r := mul(x, 1000000000000000000)
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::73 => if iszero(and(iszero(iszero(y)), eq(sdiv(r, 1000000000000000000), x))) {
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::95 => x = (x << 78) / 5**18;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::100 => int256 k = ((x << 96) / 54916777467707473351141471128 + 2**95) >> 96;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::101 => x = x - k * 54916777467707473351141471128;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::108 => y = ((y * x) >> 96) + 57155421227552351082224309758442;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::110 => p = ((p * y) >> 96) + 28719021644029726153956944680412240;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::111 => p = p * x + (4385272521454847904659076985693276 << 96);
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::115 => q = ((q * x) >> 96) + 50020603652535783019961831881945;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::116 => q = ((q * x) >> 96) - 533845033583426703283633433725380;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::117 => q = ((q * x) >> 96) + 3604857256930695427073651918091429;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::118 => q = ((q * x) >> 96) - 14423608567350463180887372962807573;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::119 => q = ((q * x) >> 96) + 26449188498355588339934803723976023;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::136 => r = int256((uint256(r) * 3822833074963236453042738258902158003155416615667) >> uint256(195 - k));
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::169 => p = ((p * x) >> 96) + 24828157081833163892658089445524;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::170 => p = ((p * x) >> 96) + 43456485725739037958740375743393;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::171 => p = ((p * x) >> 96) - 11111509109440967052023855526967;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::172 => p = ((p * x) >> 96) - 45023709667254063763336534515857;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::173 => p = ((p * x) >> 96) - 14706773417378608786704636184526;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::174 => p = p * x - (795164235651350426258249787498 << 96);
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::178 => int256 q = x + 5573035233440673466300451813936;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::179 => q = ((q * x) >> 96) + 71694874799317883764090561454958;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::180 => q = ((q * x) >> 96) + 283447036172924575727196451306956;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::181 => q = ((q * x) >> 96) + 401686690394027663651624208769553;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::182 => q = ((q * x) >> 96) + 204048457590392012362485061816622;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::183 => q = ((q * x) >> 96) + 31853899698501571402653359427138;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::184 => q = ((q * x) >> 96) + 909429971244387300277376558375;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::201 => r *= 1677202110996718588342820967067443963516166;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::203 => r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::205 => r += 600920179829731861736702779321621459595472258049074101567377883020018308;
2022-09-artgobblers/src/ArtGobblers.sol::112 => uint256 public constant MAX_SUPPLY = 10000;
2022-09-artgobblers/src/ArtGobblers.sol::159 => uint128 public numMintedFromGoo;
2022-09-artgobblers/src/ArtGobblers.sol::167 => uint128 public currentNonLegendaryId;
2022-09-artgobblers/src/ArtGobblers.sol::304 => 69.42e18, // Target price.
2022-09-artgobblers/src/ArtGobblers.sol::305 => 0.31e18, // Price decay percent.
2022-09-artgobblers/src/ArtGobblers.sol::308 => 0.0023e18 // Time scale.
2022-09-artgobblers/src/ArtGobblers.sol::673 => // Moduloing by 1 << 64 (2 ** 64) is equivalent to a uint64 cast.
2022-09-artgobblers/src/Goo.sol::58 => contract Goo is ERC20("Goo", "GOO", 18) {
2022-09-artgobblers/src/Pages.sol::122 => int256 internal constant SWITCH_DAY_WAD = 233e18;
2022-09-artgobblers/src/Pages.sol::128 => int256 internal constant SOLD_BY_SWITCH_WAD = 8336.760939794622713006e18;
2022-09-artgobblers/src/Pages.sol::168 => 4.2069e18, // Target price.
2022-09-artgobblers/src/Pages.sol::169 => 0.31e18, // Price decay percent.
2022-09-artgobblers/src/Pages.sol::170 => 9000e18, // Logistic asymptote.
2022-09-artgobblers/src/Pages.sol::171 => 0.014e18, // Logistic time scale.
2022-09-artgobblers/src/Pages.sol::174 => 9e18 // Pages to target per day.
```




### [N04] Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability


#### Findings:
```
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::177 => if iszero(lt(y, 0x10000000000000000000000000000000000)) {
```




### [N05] Use a more recent version of solidity


#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/goo-issuance/src/ibGOO.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/auth/Owned.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/LibString.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/ArtGobblers.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/Goo.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/Pages.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/GobblerReserve.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/rand/ChainlinkV1RandProvider.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/rand/RandProvider.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::2 => pragma solidity >=0.8.0;
```



### [N06] Use a more recent version of solidity


#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::64 => // to occur, we'll continue using the standard logistic VRGDA schedule.
2022-09-artgobblers/goo-issuance/src/ibGOO.sol::11 => using FixedPointMathLib for uint256;
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::207 => // sqrt(y) using sqrt(65536) * 181/1024 * (a + 1) = 181/4 * (y + 65536)/65536 = 181 * (y + 65536)/2^18.
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::105 => // Evaluate using a (6, 7)-term rational approximation.
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::166 => // Evaluate using a (8, 8)-term rational approximation.
2022-09-artgobblers/src/ArtGobblers.sol::84 => using LibString for uint256;
2022-09-artgobblers/src/ArtGobblers.sol::85 => using FixedPointMathLib for uint256;
2022-09-artgobblers/src/ArtGobblers.sol::334 => /// @notice Claim from mintlist, using a merkle proof.
2022-09-artgobblers/src/ArtGobblers.sol::574 => /// new gobblers using entropy from a random seed.
2022-09-artgobblers/src/Pages.sol::79 => using LibString for uint256;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::6 => /// @notice ERC1155B implementation optimized for ArtGobblers by using the ownerOf storage slot to store attribute data.
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::63 => // We avoid branching by using assembly to take
2022-09-artgobblers/src/utils/token/PagesERC721.sol::176 => // are set using a monotonically increasing counter and only
```




### [N07] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields
#### Findings:
```
2022-09-artgobblers/solmate/src/auth/Owned.sol::11 => event OwnerUpdated(address indexed user, address indexed newOwner);
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::15 => event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
2022-09-artgobblers/src/ArtGobblers.sol::229 => event GooBalanceUpdated(address indexed user, uint256 newGooBalance);
2022-09-artgobblers/src/ArtGobblers.sol::231 => event GobblerClaimed(address indexed user, uint256 indexed gobblerId);
2022-09-artgobblers/src/ArtGobblers.sol::232 => event GobblerPurchased(address indexed user, uint256 indexed gobblerId, uint256 price);
2022-09-artgobblers/src/ArtGobblers.sol::233 => event LegendaryGobblerMinted(address indexed user, uint256 indexed gobblerId, uint256[] burnedGobblerIds);
2022-09-artgobblers/src/ArtGobblers.sol::234 => event ReservedGobblersMinted(address indexed user, uint256 lastMintedGobblerId, uint256 numGobblersEach);
2022-09-artgobblers/src/ArtGobblers.sol::236 => event RandomnessFulfilled(uint256 randomness);
2022-09-artgobblers/src/ArtGobblers.sol::237 => event RandomnessRequested(address indexed user, uint256 toBeRevealed);
2022-09-artgobblers/src/ArtGobblers.sol::238 => event RandProviderUpgraded(address indexed user, RandProvider indexed newRandProvider);
2022-09-artgobblers/src/ArtGobblers.sol::240 => event GobblersRevealed(address indexed user, uint256 numGobblers, uint256 lastRevealedId);
2022-09-artgobblers/src/Pages.sol::134 => event PagePurchased(address indexed user, uint256 indexed pageId, uint256 price);
2022-09-artgobblers/src/Pages.sol::136 => event CommunityPagesMinted(address indexed user, uint256 lastMintedPageId, uint256 numPages);
2022-09-artgobblers/src/utils/rand/RandProvider.sol::13 => event RandomBytesRequested(bytes32 requestId);
2022-09-artgobblers/src/utils/rand/RandProvider.sol::14 => event RandomBytesReturned(bytes32 requestId, uint256 randomness);
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::13 => event TransferSingle(
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::21 => event TransferBatch(
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::29 => event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::31 => event URI(string value, uint256 indexed id);
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::17 => event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
2022-09-artgobblers/src/utils/token/PagesERC721.sol::18 => event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
```




### [N08] Use of sensitive/NC-inclusive terms


#### Findings:
```
2022-09-artgobblers/src/ArtGobblers.sol::212 => bool waitingForSeed;
2022-09-artgobblers/src/ArtGobblers.sol::727 => bool isERC1155
```



### [N09] Unused file


#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LinearVRGDA.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/goo-issuance/src/ibGOO.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/script/deploy/DeployBase.s.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/solmate/src/utils/LibString.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/ArtGobblers.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/Goo.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/Pages.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/utils/GobblerReserve.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/utils/rand/ChainlinkV1RandProvider.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/utils/rand/RandProvider.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::1 => // SPDX-License-Identifier: MIT
2022-09-artgobblers/src/utils/token/PagesERC721.sol::1 => // SPDX-License-Identifier: MIT
```




### [N10] `2**<n> - 1` should be re-written as `type(uint<n>).max`

#### Impact
Earlier versions of solidity can use uint<n>(-1) instead. Expressions not including the - 1 can often be re-written to accomodate the change (e.g. by using a > rather than a >=, which will also save some gas)
#### Findings:
```
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::100 => int256 k = ((x << 96) / 54916777467707473351141471128 + 2**95) >> 96;
```




###  [N11] Solidity versions greater than the current version should not be included in the pragma range

#### Impact
The below pragmas should be `<` 0.9.0, not `>=`
#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/goo-issuance/src/ibGOO.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/script/deploy/DeployBase.s.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/script/deploy/DeployRinkeby.s.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/auth/Owned.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/FixedPointMathLib.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/LibString.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/ArtGobblers.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/Goo.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/Pages.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/GobblerReserve.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/rand/ChainlinkV1RandProvider.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/rand/RandProvider.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/GobblersERC721.sol::2 => pragma solidity >=0.8.0;
2022-09-artgobblers/src/utils/token/PagesERC721.sol::2 => pragma solidity >=0.8.0;
```




### [N12] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external.
#### Findings:
```
2022-09-artgobblers/VRGDAs/src/LinearVRGDA.sol::41 => function getTargetSaleTime(int256 sold) public view virtual override returns (int256) {
2022-09-artgobblers/VRGDAs/src/LogisticToLinearVRGDA.sol::62 => function getTargetSaleTime(int256 sold) public view virtual override returns (int256) {
2022-09-artgobblers/VRGDAs/src/LogisticVRGDA.sol::60 => function getTargetSaleTime(int256 sold) public view virtual override returns (int256) {
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::43 => function getVRGDAPrice(int256 timeSinceStart, uint256 sold) public view virtual returns (uint256) {
2022-09-artgobblers/VRGDAs/src/VRGDA.sol::59 => function getTargetSaleTime(int256 sold) public view virtual returns (int256);
2022-09-artgobblers/solmate/src/auth/Owned.sol::39 => function setOwner(address newOwner) public virtual onlyOwner {
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::25 => function tokenURI(uint256 id) public view virtual returns (string memory);
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::35 => function ownerOf(uint256 id) public view virtual returns (address owner) {
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::39 => function balanceOf(address owner) public view virtual returns (uint256) {
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::66 => function approve(address spender, uint256 id) public virtual {
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::76 => function setApprovalForAll(address operator, bool approved) public virtual {
2022-09-artgobblers/solmate/src/tokens/ERC721.sol::146 => function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {
2022-09-artgobblers/src/ArtGobblers.sol::396 => function gobblerPrice() public view returns (uint256) {
2022-09-artgobblers/src/ArtGobblers.sol::478 => function legendaryGobblerPrice() public view returns (uint256) {
2022-09-artgobblers/src/ArtGobblers.sol::693 => function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {
2022-09-artgobblers/src/ArtGobblers.sol::757 => function gooBalance(address user) public view returns (uint256) {
2022-09-artgobblers/src/Pages.sol::219 => function pagePrice() public view returns (uint256) {
2022-09-artgobblers/src/Pages.sol::265 => function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::55 => function ownerOf(uint256 id) public view virtual returns (address owner) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::59 => function balanceOf(address owner, uint256 id) public view virtual returns (uint256 bal) {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::73 => function uri(uint256 id) public view virtual returns (string memory);
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::79 => function setApprovalForAll(address operator, bool approved) public virtual {
2022-09-artgobblers/src/utils/token/GobblersERC1155B.sol::124 => function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {
2022-09-artgobblers/src/utils/token/PagesERC721.sol::72 => function isApprovedForAll(address owner, address operator) public view returns (bool isApproved) {
```




### [N13] Numeric values having to do with time should use time units for readability

#### Impact
There are units for seconds, minutes, hours, days, and weeks
#### Findings:
```
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::15 => /// @dev Takes an integer amount of seconds and converts it to a wad amount of days.
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::17 => /// @dev Not meant for negative second amounts, it assumes x is positive.
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::25 => /// @dev Takes a wad amount of days and converts it to an integer amount of seconds.
2022-09-artgobblers/solmate/src/utils/SignedWadMath.sol::27 => /// @dev Not meant for negative day amounts, it assumes x is positive.
2022-09-artgobblers/src/ArtGobblers.sol::326 => // Reveal for initial mint must wait a day from the start of the mint.
2022-09-artgobblers/src/ArtGobblers.sol::327 => gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days);
2022-09-artgobblers/src/ArtGobblers.sol::508 => /// @dev Can only be called every 24 hours at the earliest.
2022-09-artgobblers/src/ArtGobblers.sol::533 => // We want at most one batch of reveals every 24 hours.
2022-09-artgobblers/src/ArtGobblers.sol::535 => gobblerRevealsData.nextRevealTimestamp = uint64(nextRevealTimestamp + 1 days);
2022-09-artgobblers/src/Pages.sol::120 => /// @dev The day the switch from a logistic to translated linear VRGDA is targeted to occur.
2022-09-artgobblers/src/Pages.sol::173 => SWITCH_DAY_WAD, // Target switch day.
2022-09-artgobblers/src/Pages.sol::174 => 9e18 // Pages to target per day.
```







