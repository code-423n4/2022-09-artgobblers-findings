# [NC-01] Non library/interface files should use fixed compiler version

Locking the pragma helps to ensure that contracts do not accidentally get deployed using an outdated compiler version.

Note that pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or a package.

The 5 contracts highlighted bellow are not libraries/abstract/interfaces and are floating the pragma.

```
src/ArtGobblers.sol,
src/Goo.sol,
src/Pages.sol,
src/utils/GobblerReserve.sol,
src/utils/rand/ChainlinkV1RandProvider.sol,
```

# [NC-02] Use the ternary operator where possible

The instance bellow could be replaced with a ternary operator. Maybe that's the team choice due to the prettier-ignore comment above, altought the prettier disabling might be due to the line width limit instead.

```
// If we've minted the full interval or beyond it, the price has decayed to 0.
if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;
// Otherwise decay the price linearly based on what fraction of the interval has been minted.
else return FixedPointMathLib.unsafeDivUp(startPrice * (LEGENDARY_AUCTION_INTERVAL - numMintedSinceStart), LEGENDARY_AUCTION_INTERVAL);
```

Refactoring to a ternary expression.

```
// If we've minted the full interval or beyond it, the price has decayed to 0.
// Otherwise decay the price linearly based on what fraction of the interval has been minted.
return numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL
    ? 0
    : FixedPointMathLib.unsafeDivUp(startPrice * (LEGENDARY_AUCTION_INTERVAL - numMintedSinceStart), LEGENDARY_AUCTION_INTERVAL);
```

# [NC-03] Replace magic numbers with constants where possible

```
69.42e18, // Target price.
0.31e18, // Price decay percent.
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L304-L305

```
0.0023e18 // Time scale.
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L308


```
9e18 // Pages to target per day.
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L174

```
4.2069e18, // Target price.
0.31e18, // Price decay percent.
9000e18, // Logistic asymptote.
0.014e18, // Logistic time scale.
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L168-L171

# [NC-04] Missing zero-address checks in constructors

Missing checks for zero-addresses may lead to infunctional protocol and force the need for redeployments, if the variable addresses are initialized incorrectly.

```
address _team,
address _community,
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L294-L295

```
address _community,
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L161

# [NC-05] Place public functions bellow external functions

The solidity style guide from the [docs](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends to add public functions bellow external functions. 

Most chucks separated by comments in the code are adding public bellow external, but the snippet bellow shows public above external.

```

    /*//////////////////////////////////////////////////////////////
                                GOO LOGIC
    //////////////////////////////////////////////////////////////*/

    /// @notice Calculate a user's virtual goo balance.
    /// @param user The user to query balance for.
    function gooBalance(address user) public view returns (uint256) {
        ...
    }

    /// @notice Add goo to your emission balance,
    /// burning the corresponding ERC20 balance.
    /// @param gooAmount The amount of goo to add.
    function addGoo(uint256 gooAmount) external {
        ...
    }

    /// @notice Remove goo from your emission balance, and
    /// add the corresponding amount to your ERC20 balance.
    /// @param gooAmount The amount of goo to remove.
    function removeGoo(uint256 gooAmount) external {
        ...
    }

    /// @notice Burn an amount of a user's virtual goo balance. Only callable
    /// by the Pages contract to enable purchasing pages with virtual balance.
    /// @param user The user whose virtual goo balance we should burn from.
    /// @param gooAmount The amount of goo to burn from the user's virtual balance.
    function burnGooForPages(address user, uint256 gooAmount) external {
        ...
    }
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L751-L800

# [NC-06] Missing NATSPEC

The function bellow is declared in `ArtGobblers.sol` and consumed by `GobblerReserve.sol`. Adding NATSPEC will improve documentation.

```
function transferFrom(
    address from,
    address to,
    uint256 id
) public override {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L880-L884

# [NC-07] Inconsistent use of named return variables

Some functions return named variables, others return explicit values.

Following function return explicit value

```
function gobblerPrice() public view returns (uint256) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L396

Following function return return a named variable

```
function claimGobbler(bytes32[] calldata proof) external returns (uint256 gobblerId) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L339

Consider adopting a consistent approach to return values throughout the codebase by removing all named return variables, explicitly declaring them as local variables, and adding the necessary return statements where appropriate. This would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.

# [NC-08] Public functions not called by the contract should be declared external

```
function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L693

```
function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L265