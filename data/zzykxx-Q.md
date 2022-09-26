# Low risk & QA

1. Lack of a 2-step ownership transfer
2. Gobbler accounting can be wrong
3. There can be more than 1 reveal per 24 hours
4. Truncation at 120 bits instead of 128
5. The idx of legendary gobblers is always 0

## [1] Lack of a 2-step ownership transfer

In `ArtGobblers.sol` the function `setOwner` sets the owner variable directly, meaning that passing the wrong address could lead to the contract having an owner which is a dead address.

No more gobblers can be revealed if the owner is a dead address and `randProvider` stops working.

Implementing a 2-step ownership transfer where the future owner has to accept the ownership by making a call to `ArtGobblers.sol` might reduce stress levels by quite a bit when and if ownership has to be transferred.

This same reasoning applies to `GobblerReserve.sol`, which in case of a dead address as owner could cause a loss up to 20% of gobblers and 10% of pages.

Submitting as low risk because of heavy external requirements.

## [2] Gobbler accounting can be wrong

At [ArtGobblers.sol#L458](https://github.com/artgobblers/art-gobblers/blob/master/src/ArtGobblers.sol#L458) a `1` is added to `gobblersOwned` to factor in the legendary, but then `_mint()` adds `1` again, meaning that the variable `gobblersOwned`is off by `1` per legendary gobbler minted. This is not exploitable, but could lead to problems in external projects that use `balanceOf` to get an address gobblers balance.

## [3] Truncation at 120 bits instead of 128

At [ArtGobblers.sol#L461](https://github.com/artgobblers/art-gobblers/blob/master/src/ArtGobblers.sol#L461) start price is truncated to 120 bits instead of 128, not exploitable and likely to be a typo.

## [4] There can be more than 1 reveal per 24 hours

At [ArtGobblers.sol#L535](https://github.com/artgobblers/art-gobblers/blob/master/src/ArtGobblers.sol#L535) the `nextRevealTimestamp` variable is set to the current `nextRevealTimestamp + 1 day`, but this can still result in a past timestamp if `requestRandomSeed()` wasn't previously called in the past `24 hours`. Could be considered a design choice, but it's nice to be aware of it. Not exploitable per my findings.

## [5] The idx of legendary gobblers is always 0

The function `mintLegendaryGobbler()` never sets the idx of the minted legendary gobbler.
