# 1. Unnecessary Maths with Constants costs extra Gas.
In `ArtGobblers.sol`:

`RESERVED_SUPPLY ` is calculated using already declared constants variables, which is just costing extra gas.
It can be declared `1598` directly, no need to do the extra maths:
```diff
-    uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;

+    uint256 public constant RESERVED_SUPPLY = 1598;
```

The same goes for the `MAX_MINTABLE`:
```diff
-    uint256 public constant MAX_MINTABLE = MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY - RESERVED_SUPPLY;

+    uint256 public constant MAX_MINTABLE = 6392;
```

And the same goes for the `FIRST_LEGENDARY_GOBBLER_ID ` & `LEGENDARY_AUCTION_INTERVAL `

# 2. Revert `Custom Errors` instead of Strings & use `If-Else+Custom-Errors` instead of `Require` Statements
[ArtGobblers.sol#L696](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L696), [ArtGobblers.sol#L705)](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L705), [ArtGobblers.sol#L711](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L711) [Pages.sol#L266](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L266) & [ArtGobblers.sol#L885](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L885), [ArtGobblers.sol#L887](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L887), [ArtGobblers.sol#L889](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L889), [GobblersERC721.sol#L62](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L62), [GobblersERC721.sol#L95](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L95), [GobblersERC721.sol#L121](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L121), [PagesERC721.sol#L55](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L55), [PagesERC721.sol#L59](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L59), [PagesERC721.sol#L85](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L85), [PagesERC721.sol#L103](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L103), [PagesERC721.sol#L105](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L105), [PagesERC721.sol#L107](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L107), [PagesERC721.sol#L135](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L135), [PagesERC721.sol#L151](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L151)

# 3. `i++` costs extra gas, use `++i` instead
[Pages.sol#L251](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251), [GobblerReserve.sol#L37](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37)