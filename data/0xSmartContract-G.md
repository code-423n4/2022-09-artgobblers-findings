### Gas Optimizations List
| Number | Optimization Details | Per gas saved | Context |
|:--:|:-------| :-----:| :-----:|
| [G-01] | No need ```return``` keyword for gas efficient (for external functions) | 3 | 2 |
| [G-02] | ``Shifting`` cheaper than ``division`` | 6 | 1 |
| [G-03] | Using ```uint256``` instead of ```bool``` in mappings is more gas efficient | 146 | 2 |
| [G-04] | Use ``assembly`` to write _address storage values_ | 33-399 | 13 |
| [G-05] | _Function ordering_ via ``methodID`` | 22 | All contracts |
| [G-06] |Use assembly to check for ```address(0)``` | 6 | 2 |
| [G-07] |Setting the ``constructor`` to ``payable`` | 13 | 6 |
| [G-08] |Catching the array length prior to loop | 13 | 1 |
| [G-09] |Use ```++index ```  instead of ```index++``` to increment a loop counter | 5 | 1 |
| [G-10] |Use ``Custom Errors`` rather than revert() / require() strings to save deployment gas | 68 | 14 |
| [G-11] |Using ```private``` rather than ```public``` _for constants_, saves gas | 10 |7 |
| [G-12] |_Functions guaranteed to revert_ when callled by normal users can be marked ``payable`` | 24 | 2 |
| [G-13] |Save gas by just catching to ``msg.sender`` (even if you never use it) | 2 | 1 |

Total 13 issues

### Suggestions
| Number | Suggestion Details |Context |
|:--:|:-------|:-------:|
| [S-01] |Missing `zero-address` check in `constructor | 9 |

Total 1 suggestion


## [G-01] No need ```return``` keyword for gas efficient (for external functions) [ 3 gas saved]

**Context:**
[ArtGobblers.sol#L541](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L541)
[ChainlinkV1RandProvider.sol#L69](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L69)

**Recommendation:**
You must remove the ```return``` keyword from the specified contexts.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

**Gas Report:**
src/ArtGobblers.sol/requestRandomSeed [ 3 gas saved]
src/utils/rand/ChainlinkV1RandProvider.sol/ requestRandomBytes [ 3 gas saved]


First gas test with return parameter (Current Code)
Second gas test by removing return parameter (Recommended code)

```js
Current Code: 
╭─────────────────────────────────────────────┬─────────────────┬────────┬────────┬─────────┬─────────╮
│ src/ArtGobblers.sol:ArtGobblers contract    ┆                 ┆        ┆        ┆         ┆         │
╞═════════════════════════════════════════════╪═════════════════╪════════╪════════╪═════════╪═════════╡
│ Function Name                               ┆ min             ┆ avg    ┆ median ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ requestRandomSeed                           ┆ 419             ┆ 47286  ┆ 58718  ┆ 58718   ┆ 45      │
╰─────────────────────────────────────────────┴─────────────────┴────────┴────────┴─────────┴─────────╯

Recommendation Code:
╭─────────────────────────────────────────────┬─────────────────┬────────┬────────┬─────────┬─────────╮
│ src/ArtGobblers.sol:ArtGobblers contract    ┆                 ┆        ┆        ┆         ┆         │
╞═════════════════════════════════════════════╪═════════════════╪════════╪════════╪═════════╪═════════╡
│ Function Name                               ┆ min             ┆ avg    ┆ median ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ requestRandomSeed                           ┆ 419             ┆ 47283  ┆ 58715  ┆ 58715   ┆ 45      │
╰─────────────────────────────────────────────┴─────────────────┴────────┴────────┴─────────┴─────────╯
```

```js
Current Code: 
╭─────────────────────────────────────────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/utils/rand/ChainlinkV1RandProvider.sol:ChainlinkV1RandProvider contract ┆                 ┆       ┆        ┆       ┆         │
╞═════════════════════════════════════════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Function Name                                                               ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ requestRandomBytes                                                          ┆ 232             ┆ 41869 ┆ 46248  ┆ 46248 ┆ 42      │
╰─────────────────────────────────────────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯

Recommendation Code:
╭─────────────────────────────────────────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/utils/rand/ChainlinkV1RandProvider.sol:ChainlinkV1RandProvider contract ┆                 ┆       ┆        ┆       ┆         │
╞═════════════════════════════════════════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Function Name                                                               ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ requestRandomBytes                                                          ┆ 232             ┆ 41866 ┆ 46245  ┆ 46245 ┆ 42      │
╰─────────────────────────────────────────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯ 
```

## [G-02] ``Shifting`` cheaper than ``division`` [6 gas saved]

**Context:**
[ArtGobblers.sol#L462](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L462)

**Description:**
A division by 2 can be calculated by shifting one to the right. While the ```DIV``` opcode uses ```5 gas```, the ```SHR``` opcode only uses ```3 gas``` Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

**Recommendation:**
Instead of dividing by ``2`` use ```>> 1```.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

Current:
[ArtGobblers.sol#L462](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L462)
```js
legendaryGobblerAuctionData.startPrice = uint120(
                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
            );
```
Recommendation:
```js
legendaryGobblerAuctionData.startPrice = uint120(
                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE >>1 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
            );
```
**Gas Report:**
```js
Current Code: 
╭─────────────────────────────────────────────┬─────────────────┬────────┬────────┬─────────┬─────────╮
│ src/ArtGobblers.sol:ArtGobblers contract    ┆                 ┆        ┆        ┆         ┆         │
╞═════════════════════════════════════════════╪═════════════════╪════════╪════════╪═════════╪═════════╡
│ Deployment Cost                             ┆ Deployment Size ┆        ┆        ┆         ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                               ┆ min             ┆ avg    ┆ median ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ mintLegendaryGobbler                        ┆ 911             ┆ 72774  ┆ 31201  ┆ 456707  ┆ 23      │
╰─────────────────────────────────────────────┴─────────────────┴────────┴────────┴─────────┴─────────╯

Recommendation Code:
╭─────────────────────────────────────────────┬─────────────────┬────────┬────────┬─────────┬─────────╮
│ src/ArtGobblers.sol:ArtGobblers contract    ┆                 ┆        ┆        ┆         ┆         │
╞═════════════════════════════════════════════╪═════════════════╪════════╪════════╪═════════╪═════════╡
│ Deployment Cost                             ┆ Deployment Size ┆        ┆        ┆         ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                               ┆ min             ┆ avg    ┆ median ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ mintLegendaryGobbler                        ┆ 911             ┆ 72768  ┆ 31193  ┆ 456700  ┆ 23      │
╰─────────────────────────────────────────────┴─────────────────┴────────┴────────┴─────────┴─────────╯
```

## [G-03] Using ```uint256``` instead of ```bool``` in mappings is more gas efficient [146 gas per instance]

**Context:** 
[ArtGobblers.sol#L149](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L149)
[ArtGobblers.sol#L212](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L212)

**Description:**
OpenZeppelin uint256 and bool comparison: 
Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled. The values being non-zero value makes deployment a bit more expensive.

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past.

**Recommendation:**
Use  uint256(1) and uint256(2) instead of bool.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
     
        c0._setPolicyPermissions(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4, 79, true);
        c1._setPolicyPermissions(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4, 79, 2);
    }
}

contract Contract0 {
    mapping(address => mapping(uint256  => bool)) public modulePermissions;
    error boolcheck();

    function _setPolicyPermissions(address addr_, uint256 id, bool log) external {
       modulePermissions[addr_][id] = log; 
       if(modulePermissions[addr_][id] = false) revert boolcheck();
    }
}
contract Contract1 {
    mapping(address => mapping(uint256  => uint256)) public modulePermissions;
    error boolcheck();

    // Use uint256 instead of bool    (1 = false , 2 = true) 
    function _setPolicyPermissions(address addr_, uint256 id, uint256 log) external {
       modulePermissions[addr_][id] == log; 
       if(modulePermissions[addr_][id] == 1) revert boolcheck();
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 88335                                     ┆ 473             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _setPolicyPermissions                     ┆ 2750            ┆ 2750 ┆ 2750   ┆ 2750 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 87135                                     ┆ 467             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _setPolicyPermissions                     ┆ 2604            ┆ 2604 ┆ 2604   ┆ 2604 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-04] Use ``assembly`` to write _address storage values_ [39 - 399 gas per instance]

**Context:**
[ArtGobblers.sol#L320](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L320)
[ArtGobblers.sol#L321](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L321)
[ArtGobblers.sol#L314](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L314)
[ArtGobblers.sol#L315](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L315)
[ArtGobblers.sol#L316](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316)
[ArtGobblers.sol#L317](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L317)
[Goo.sol#L83](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83)
[Goo.sol#L84](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L84)
[Pages.sol#L179](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L179)
[Pages.sol#L181](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L181)
[Pages.sol#L183](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L183)
[GobblerReserve.sol#L24](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L24)
[ChainlinkV1RandProvider.sol#L55](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L55)

**Description:** 
Use assembly to write address storage values.
399 gas saves per instance for ```string``` 
  39 gas saves per instance for ```address```
uint256 also saves no gas

**Proof Of Concept: (for string)**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTestFoundry is DSTest {

    Contract1 c1;
    Contract2 c2;

    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }

    function testGas() public {
        c1.setRewardTokenAndAmount("Test");
        c2.setRewardTokenAndAmount("Test");
    }
}


contract Contract1  {

    string reward;

    function setRewardTokenAndAmount(string memory reward_) external {
        reward = reward_;
    }
}


contract Contract2  {

    string reward;

    function setRewardTokenAndAmount(string memory reward_) external {
        assembly {
            sstore(reward.slot, reward_)           
        }
    }
}
```
**Gas Report:**
```js
╭──────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/test.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞══════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 167614                               ┆ 869             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount              ┆ 23012           ┆ 23012 ┆ 23012  ┆ 23012 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭──────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/test.sol:Contract2 contract ┆                 ┆       ┆        ┆       ┆         │
╞══════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 75523                                ┆ 409             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount              ┆ 22613           ┆ 22613 ┆ 22613  ┆ 22613 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```

**Proof Of Concept: (for address)**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTestFoundry is DSTest {

    Contract1 c1;
    Contract2 c2;

    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }

    function testGas() public {
        c1.setRewardTokenAndAmount(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4);
        c2.setRewardTokenAndAmount(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4);    }
}


contract Contract1  {

    address reward;

    function setRewardTokenAndAmount(address reward_) external {
        reward = reward_;
    }
}


contract Contract2  {

    address reward;

    function setRewardTokenAndAmount(address reward_) external {
        assembly {
            sstore(reward.slot, reward_)           
        }
    }
}
```
**Gas Report:**
```js
╭──────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/test.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞══════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 48499                                ┆ 273             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount              ┆ 22363           ┆ 22363 ┆ 22363  ┆ 22363 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭──────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/test.sol:Contract2 contract ┆                 ┆       ┆        ┆       ┆         │
╞══════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 35287                                ┆ 207             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount              ┆ 22324           ┆ 22324 ┆ 22324  ┆ 22324 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```

## [G-05] _Function ordering_ via ``methodID`` [22 gas per instance]

**Context:** 
All Contracts

**Description:** 
Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ``` ArtGobblers.sol``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://coinsbench.com/advanced-gas-optimizations-tips-for-solidity-85c47f413dc5

ArtGobblers.sol function names can be named and sorted according to METHOD ID

```js
Sighash   |   Function Signature
========================
acede5fd  =>  claimGobbler(bytes32[])
c9bddac6  =>  mintFromGoo(uint256,bool)
10f255f5  =>  gobblerPrice()
201db8dd  =>  mintLegendaryGobbler(uint256[])
d71d672f  =>  legendaryGobblerPrice()
e1da26c6  =>  requestRandomSeed()
13b45a47  =>  acceptRandomSeed(bytes32,uint256)
beb31911  =>  upgradeRandProvider(RandProvider)
743ee994  =>  revealGobblers(uint256)
c87b56dd  =>  tokenURI(uint256)
6cfdbcae  =>  gobble(uint256,address,uint256,bool)
d075fbba  =>  gooBalance(address)
0b38049f  =>  addGoo(uint256)
9cc397fb  =>  removeGoo(uint256)
2f689b85  =>  burnGooForPages(address,uint256)
33349cde  =>  updateUserGooBalance(address,uint256,GooBalanceUpdateType)
65ca58b8  =>  mintReservedGobblers(uint256)
fa522a15  =>  getGobblerEmissionMultiple(uint256)
11f93e78  =>  getUserEmissionMultiple(address)
23b872dd  =>  transferFrom(address,address,uint256)
```

## [G-06] Use assembly to check for ```address(0)``` [6 gas per instance]

**Context:** 
[ArtGobblers.sol#L887](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887)
[PagesERC721.sol#L105](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L105)

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    function testGas() public view {
        c0.setOperator(address(this));
        c1.setOperator(address(this));
    }
}
contract Contract0 {
    function setOperator(address operator_) public pure {
        require(operator_) != address(0), "INVALID_RECIPIENT");    }
}
contract Contract1 {
    function setOperator(address operator_) public pure {
        assembly {
            if iszero(operator_) {
                mstore(0x00, "Callback_InvalidParams")
                revert(0x00, 0x20)
            }
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 50899                                     ┆ 285             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 258             ┆ 258 ┆ 258    ┆ 258 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44893                                     ┆ 255             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 252             ┆ 252 ┆ 252    ┆ 252 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

## [G-07] Setting the ``constructor`` to ``payable`` [13 gas per instance]

**Context:**
[ArtGobblers.sol#L287](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L287)
[Goo.sol#L82](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L82)
[Pages.sol#L156](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L156)
[GobblerReserve.sol#L23](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L23)
[ChainlinkV1RandProvider.sol#L48](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L48)
[PagesERC721.sol#L36](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L36)

**Description:**
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

**Recommendation:**
Set the constructor to ```payable```.

**Proof Of Concept:**
https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/5?u=pcaversaccio

The optimizer was turned on and set to 10000 runs.

```js
contract GasTestFoundry is DSTest {
    
    Contract1 c1;
    Contract2 c2;
    
    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }
    
    function testGas() public {
        c1.x();
        c2.x();
    }
}

contract Contract1 {
    
    uint256 public dummy;
    constructor() payable {
        dummy = 1;
    }
    
    function x() public {
    }
}

contract Contract2 {
    
    uint256 public dummy;
    constructor() {
        dummy = 1;
    }
    
    function x() public {
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 49563                                     ┆ 159             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤

╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 49587                                     ┆ 172             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
```

## [G-08] Catching the array length prior to loop [13 gas per  6 arrays instance]

**Context:** 
[GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)


**Description:** 
One can save gas by caching the array length (in stack) and using that set variable in the loop. Replace state variable reads and writes within loops with local variable reads and writes. This is done by assigning state variable values to new local variables, reading and/or writing the local variables in a loop, then after the loop assigning any changed local variables to their equivalent state variables.

**Recommendation:** 
Simply do something like so before the for loop:  ```uint length = variable.length``` Then add length in place of  ```variable.length```  in the for loop.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract1 c1;
    Contract2 c2;

    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }

    function testGas() public {
        address[] memory Inputs = new address[](6);
        Inputs[0] = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
        Inputs[1] = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
        Inputs[2] = 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db;
        Inputs[3] = 0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB;
        Inputs[4] = 0x617F2E2fD72FD9D5503197092aC168c91465E7f2;
        Inputs[5] = 0x17F6AD8Ef982297579C203069C1DbfFE4348c372;

        c1.executeProposal(Inputs);
        c2.executeProposal(Inputs);
    }
}

contract Contract1 {

    function executeProposal(address[] memory test) public pure {
        uint256 a = test.length;
        for (uint256 step; step  < a; ) {
            unchecked {
                ++step;
            }
        }
    }
}

contract Contract2 { 

    function executeProposal(address[] memory test) public pure {
        for (uint256 step; step < test.length; ) {
            unchecked {
                ++step;
            }
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 92941                                     ┆ 496             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ executeProposal                           ┆ 1653            ┆ 1653 ┆ 1653   ┆ 1653 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 92541                                     ┆ 494             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ executeProposal                           ┆ 1666            ┆ 1666 ┆ 1666   ┆ 1666 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-09] Use ```++index ```  instead of ```index++``` to increment a loop counter [5 gas per instance]

**Context:**
[Pages.sol#L251](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)

**Description:**
Due to reduced stack operations, using ++index saves 5 gas per iteration.

**Recommendation:**
Use ++index to increment a loop counter.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.
```js
contract GasTest is DSTest {

    Contract0 c0;
    Contract1 c1;

   function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }

   function testGas() public {

        c0.foo();
        c1.foo();
    }
}

contract Contract0 {

    function foo() external {
        uint256 versionNFTDropCollection;
        ++versionNFTDropCollection;
    }
}

contract Contract1 {

    function foo() external {
        uint256 versionNFTDropCollection;
        versionNFTDropCollection++;
    }
}
```
**Gas Report:**
```js
╭──────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/test.sol:Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞══════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 42893                                ┆ 245             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                  ┆ 193             ┆ 193 ┆ 193    ┆ 193 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭──────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/test.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞══════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                      ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 43293                                ┆ 247             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                        ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                  ┆ 198             ┆ 198 ┆ 198    ┆ 198 ┆ 1       │
╰──────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

## [G-10] Use ``Custom Errors`` rather than revert() / require() strings to save deployment gas [68 gas per instance]

**Context:** 
[ArtGobblers.sol#L437](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L437)
[ArtGobblers.sol#L885](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L885)
[ArtGobblers.sol#L887](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L887)
[ArtGobblers.sol#L889](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L889)
[PagesERC721.sol#L85](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L85)
[PagesERC721.sol#L103](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L103)
[PagesERC721.sol#L105](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L105)
[PagesERC721.sol#L107](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L107)
[PagesERC721.sol#L135](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L135)
[PagesERC721.sol#L151](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L151)
[GobblersERC721.sol#L95](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L95)
[GobblersERC721.sol#L121](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L121)
[GobblersERC1155B.sol#L149](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L149)
[GobblersERC1155B.sol#L185](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L185)

**Description:**
Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas.
https://blog.soliditylang.org/2021/04/21/custom-errors/

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;

    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
       c0.foo();
       c1.foo();
       }
    }

contract Contract0 {

    uint256 version;
    error Error_Message();

    function foo() external {
        if (version != 0) {
        revert Error_Message();
        }
    }
}

contract Contract1 {
 
    uint256 version;

    function foo() external {
    require(version == 0, "error");
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 33287                                     ┆ 196             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 2242            ┆ 2242 ┆ 2242   ┆ 2242 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 41693                                     ┆ 239             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 2310            ┆ 2310 ┆ 2310   ┆ 2310 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-11] Using ```private``` rather than ```public``` _for constants_, saves gas [ 10 gas per instance]

**Context:**
[ArtGobblers.sol#L112](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112)
[ArtGobblers.sol#L115](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115)
[ArtGobblers.sol#L118](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118)
[ArtGobblers.sol#L122](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L122)
[ArtGobblers.sol#L177](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L177)
[ArtGobblers.sol#L180](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L180)
[ArtGobblers.sol#L184](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L184)

**Proof Of Concept**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        
        c0.gas();
        c1.gas();
    }
}

contract Contract0 {

    uint256 public constant BASE_GAS = 36000;
   
    function gas() external returns(uint256 gasPrice) {
        gasPrice = gasleft() + BASE_GAS;
    }
}

contract Contract1 {

    uint256 private constant BASE_GAS = 36000;

    function gas() external returns(uint256 gasPrice) {
        gasPrice = gasleft() + BASE_GAS;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 43693                                     ┆ 249             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ gas                                       ┆ 263             ┆ 263 ┆ 263    ┆ 263 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 40893                                     ┆ 235             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ gas                                       ┆ 253             ┆ 253 ┆ 253    ┆ 253 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

## [G-12] _Functions guaranteed to revert_ when callled by normal users can be marked ``payable`` [24 gas per instance]

**Context:** 
[ArtGobblers.sol#L560](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L560)
[GobblerReserve.sol#L34](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L34)

**Description:**
If a function modifier or require such as onlyOwner-admin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

**Recommendation:**
Functions guaranteed to revert when called by normal users can be marked payable  (for only ```onlyowner or admin``` functions)

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.foo();
        c1.foo();
    }
}

contract Contract0 {
    uint256 versionNFTDropCollection;
    
    function foo() external {
        versionNFTDropCollection++;
    }
}

contract Contract1 {
    uint256 versionNFTDropCollection;
    
    function foo() external payable {
        versionNFTDropCollection++;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44293                                     ┆ 252             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22308           ┆ 22308 ┆ 22308  ┆ 22308 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 41893                                     ┆ 240             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22284           ┆ 22284 ┆ 22284  ┆ 22284 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```

## [G-13] Save gas by just catching to ``msg.sender`` (even if you never use it) [ 2 gas per function run]

**Context:**
[ArtGobblers.sol#L339](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L339)

**Proof Of Concept**
The optimizer was turned on and set to 1000000 runs.

```js
ArtGobblers.sol#L339
Current Code :

 function claimGobbler(bytes32[] calldata proof) external returns (uint256 gobblerId) {
        // If minting has not yet begun, revert.
        if (mintStart > block.timestamp) revert MintStartPending();

        // If the user has already claimed, revert.
        if (hasClaimedMintlistGobbler[msg.sender]) revert AlreadyClaimed();

        // If the user's proof is invalid, revert.
        if (!MerkleProofLib.verify(proof, merkleRoot, keccak256(abi.encodePacked(msg.sender)))) revert InvalidProof();

        hasClaimedMintlistGobbler[msg.sender] = true;

        unchecked {
            // Overflow should be impossible due to supply cap of 10,000.
            emit GobblerClaimed(msg.sender, gobblerId = ++currentNonLegendaryId);
        }

        _mint(msg.sender, gobblerId);
    }

Recommendation Code:

 function claimGobbler(bytes32[] calldata proof) external returns (uint256 gobblerId) {
        // If minting has not yet begun, revert.
        if (mintStart > block.timestamp) revert MintStartPending();

        address _msgSender = msg.sender;    // This is fantastic optimization

        // If the user has already claimed, revert.
        if (hasClaimedMintlistGobbler[msg.sender]) revert AlreadyClaimed();

        // If the user's proof is invalid, revert.
        if (!MerkleProofLib.verify(proof, merkleRoot, keccak256(abi.encodePacked(msg.sender)))) revert InvalidProof();

        hasClaimedMintlistGobbler[msg.sender] = true;

        unchecked {
            // Overflow should be impossible due to supply cap of 10,000.
            emit GobblerClaimed(msg.sender, gobblerId = ++currentNonLegendaryId);
        }

        _mint(msg.sender, gobblerId);
    }
```
**Gas Report:**
```js
Current Code :
╭─────────────────────────────────────────────┬─────────────────┬────────┬────────┬─────────┬─────────╮
│ src/ArtGobblers.sol:ArtGobblers contract    ┆                 ┆        ┆        ┆         ┆         │
╞═════════════════════════════════════════════╪═════════════════╪════════╪════════╪═════════╪═════════╡
│ Deployment Cost                             ┆ Deployment Size ┆        ┆        ┆         ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 4034227                                     ┆ 22345           ┆        ┆        ┆         ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                               ┆ min             ┆ avg    ┆ median ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ claimGobbler                                ┆ 574             ┆ 47364  ┆ 48139  ┆ 93282   ┆ 6       │
╰─────────────────────────────────────────────┴─────────────────┴────────┴────────┴─────────┴─────────╯

Recommendation Code:
╭─────────────────────────────────────────────┬─────────────────┬────────┬────────┬─────────┬─────────╮
│ src/ArtGobblers.sol:ArtGobblers contract    ┆                 ┆        ┆        ┆         ┆         │
╞═════════════════════════════════════════════╪═════════════════╪════════╪════════╪═════════╪═════════╡
│ Deployment Cost                             ┆ Deployment Size ┆        ┆        ┆         ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 4033827                                     ┆ 22343           ┆        ┆        ┆         ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                               ┆ min             ┆ avg    ┆ median ┆ max     ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ claimGobbler                                ┆ 574             ┆ 47362  ┆ 48137  ┆ 93278   ┆ 6       │
╰─────────────────────────────────────────────┴─────────────────┴────────┴────────┴─────────┴─────────╯
```

## [S-01] Missing `zero-address` check in `constructor`

**Context:**
[ArtGobblers.sol#L294](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L294)
[ArtGobblers.sol#L295](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L295)
[ArtGobblers.sol#L292](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L292)
[ArtGobblers.sol#L293](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L293)
[ArtGobblers.sol#L296](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L296)
[Goo.sol#L82](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L82)
[Pages.sol#L160](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L160)
[Pages.sol#L161](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L161)
[Pages.sol#L162](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L162)

**Description:**
Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly. It also wast gas as it requires the redeployment of the contract.


