1. Artgobblers.sol :  `++legendaryGobblerAuctionData.numSold` cost less gas compared to `legendaryGobblerAuctionData.numSold += 1`

This case was the same as [this](https://code4rena.com/reports/2022-05-opensea-seaport#g-06-ordercombinersol-totalfilteredexecutions-costs-less-gas-compared-to-totalfilteredexecutions--1)

So it consider to replacing by set `++legendaryGobblerAuctionData.numSold` rather than `legendaryGobblerAuctionData.numSold += 1`

2. x += y cost more gas than x = x + y For state variables

in case same as [this](https://code4rena.com/reports/2022-08-rigor#g-11-x--y-costs-more-gas-than-x--x--y-for-state-variables) 

File: ArtGobblers.sol [Line.906](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L906)

```
      getUserData[from].gobblersOwned -= 1;
```

The above should be modified to :

```
       getUserData[from].gobblersOwned = getUserData[from].gobblersOwned - 1;
```

Same case : [Line.913] (https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L913)


3. Unchecked can be cost less gas by prefix than postfix

In this case was same to [this](https://code4rena.com/reports/2022-08-rigor#g-04-x-is-more-efficient-than-xsaves-6-gas)

Files : 

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251


4. changed uint256 i = 0 into uint256 i for saving more gas

Files : 

1.) https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432

2.) https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592

3.) https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251

4.) https://github.com/code-423n4/2022-09 artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186

5. Custom Error 

Files : 

1.) https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L437





