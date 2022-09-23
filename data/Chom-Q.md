## gobblerIds length should be checked equal to cost instead of less than cost

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L411-L420

```
    function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) {
        gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold; // Assign id.

        // If the gobbler id would be greater than the max supply, there are no remaining legendaries.
        if (gobblerId > MAX_SUPPLY) revert NoRemainingLegendaryGobblers();

        // This will revert if the auction hasn't started yet, no need to check here as well.
        uint256 cost = legendaryGobblerPrice();

        if (gobblerIds.length < cost) revert InsufficientGobblerAmount(cost);
```

Currently, it allows supplying more gobblerIds than cost. But in fact, the user should supply exact gobblerIds count as a cost. It should use == instead of <

```
    function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) {
        gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold; // Assign id.

        // If the gobbler id would be greater than the max supply, there are no remaining legendaries.
        if (gobblerId > MAX_SUPPLY) revert NoRemainingLegendaryGobblers();

        // This will revert if the auction hasn't started yet, no need to check here as well.
        uint256 cost = legendaryGobblerPrice();

        if (gobblerIds.length == cost) revert InsufficientGobblerAmount(cost);
```