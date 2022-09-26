# QA report

## Low Risk
| L-N    |Issue|Instances|
|:------:|:----|:-------:|
| [L&#x2011;01] | Missing zero address checks | 9 |

Total: 9 instances over 1 issues

## Non-critical

| N-N    |Issue|Instances|
|:------:|:----|:-------:|
| [N&#x2011;01] | Use `++<VAR>` and `--<VAR>` in all cases | 5 |
| [N&#x2011;02] | Old `mintStart` timestamp | 11 |

Total: 16 instances over 2 issues

## Low Risk

### [L-01] Missing zero address checks

```solidity
File: /src/utils/GobblerReserve.sol

/*_artGobblers parameter*/
23    constructor(ArtGobblers _artGobblers, address _owner) Owned(_owner) {

/*_owner parameter*/
23    constructor(ArtGobblers _artGobblers, address _owner) Owned(_owner) {
```

```solidity
File: /src/Pages.sol

160        Goo _goo,

161        address _community,

162        ArtGobblers _artGobblers,
```

```solidity
File: /src/utils/token/PagesERC721.sol

37        ArtGobblers _artGobblers,
```

```solidity
File: /src/ArtGobblers.sol

292        Goo _goo,

293        Pages _pages,

294        address _team,

295        address _community,

296        RandProvider _randProvider,
```

## Non-critical

### [N-01] Use `++<VAR>` and `--<VAR>` in all cases

Align the code to use `++<VAR>` and `--<VAR>` instead of `<VAR>++` and `<VAR>--`

```solidity
File: /src/utils/GobblerReserve.sol

37            for (uint256 i = 0; i < ids.length; i++) {
```

```solidity
File: /src/Pages.sol

251            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

```solidity
File: /src/utils/token/PagesERC721.sol

115            _balanceOf[from]--;

117            _balanceOf[to]++;

181            _balanceOf[to]++;
```

### [N-02] Old `mintStart` timestamp

The timestamp `1656369768` it's the date `Mon Jun 27 2022 22:42:48 GMT+0000`, was 3 months ago approx(my current timestamp it's `1664230384`)
I recommend pass the `mintStart` as parameter of the constructor and make sure it will be greater than the current timestamp
Also can set inside the `DeployRinkeby` contract like, `uint256 public immutable mintStart = block.timestamp + <DELTA_TO_START>;`

```solidity
File: /script/deploy/DeployRinkeby.s.sol

11    uint256 public immutable mintStart = 1656369768;
```
