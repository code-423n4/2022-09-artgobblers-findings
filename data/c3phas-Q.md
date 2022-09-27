

## QA
## Missing checks for address(0x0) when assigning values to address state variables
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L316-L317
```solidity
File: /src/ArtGobblers.sol
316:        team = _team;
317:        community = _community;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L181
```solidity
File: /src/Pages.sol
181:        community = _community;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Goo.sol#L83-L84
```solidity
File: /src/Goo.sol
83:        artGobblers = _artGobblers;
84:        pages = _pages;
```

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L30
```solidity
File: /src/auth/Owned.sol
30:        owner = _owner;
```

## constants should be defined rather than using magic numbers
There are several occurrences of literal values with unexplained meaning .Literal values in the codebase without an explained meaning make the code harder to read, understand and maintain, thus hindering the experience of developers, auditors and external contributors alike.

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name. 

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L304
```solidity
File: /src/ArtGobblers.sol
304:            69.42e18, // Target price.
305:            0.31e18, // Price decay percent.

308:            0.0023e18 // Time scale.
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L168-L171
```solidity
File: /src/Pages.sol
168:            4.2069e18, // Target price.
169:            0.31e18, // Price decay percent.
170:            9000e18, // Logistic asymptote.
171:            0.014e18, // Logistic time scale.

174:            9e18 // Pages to target per day.
```

https://github.com/transmissions11/goo-issuance/blob/648e65e66e43ff5c19681427e300ece9c0df1437/src/LibGOO.sol#L38
```solidity
File: /src/LibGOO.sol

//@audit:1e18 
38:                (emissionMultiple * lastBalanceWad * 1e18).sqrt()
```

https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L29
```solidity
File: /src/VRGDA.sol

//@audit:1e18
29:        decayConstant = wadLn(1e18 - _priceDecayPercent);
```

https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L44
```solidity
File: /src/LogisticVRGDA.sol

//@audit: 1e18
44:        logisticLimit = _maxSellable + 1e18;

//@audit: 2e18
47:        logisticLimitDoubled = logisticLimit * 2e18;

//@audit: 1e18
62:            return -unsafeWadDiv(wadLn(unsafeDiv(logisticLimitDoubled, sold + logisticLimit) - 1e18), timeScale);
```

## public functions not called by the contract should be declared external instead
Contracts are allowed to override their parents' functions and change the visibility from external to public.

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L693
```solidity
File: /src/ArtGobblers.sol
693:    function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L265
```solidity
File: /src/Pages.sol
265:    function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
```

## Typos/Grammer
https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L17
```solidity
File: /src/LogisticVRGDA.sol

//@audit: of tokens of tokens --> of tokens (repeated words)
17:    /// @dev The maximum number of tokens of tokens to sell + 1. We add

//@audit: of tokens of tokens --> of tokens (repeated words)
22:    /// @dev The maximum number of tokens of tokens to sell + 1 multiplied
```


## Natspec is incomplete/Missing
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/rand/ChainlinkV1RandProvider.sol#L61-L70
```solidity
File: /src/utils/rand/ChainlinkV1RandProvider.sol

//@audit: Missing @return requestId

61:    /// @notice Request random bytes from Chainlink VRF. Can only by called by the ArtGobblers contract.
62:    function requestRandomBytes() external returns (bytes32 requestId) {

70:    }
```

## Code Structure Deviates From Best-Practice

### Description:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier. Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. 
Some constructs deviate from this recommended best-practice: Modifiers are in the middle of contracts. External/public functions are mixed with internal/private ones.

**The following contract do not adhere to the standard**


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol

The above only represents a small part of the project that doesn't adhere to the best practices, but majority of the code is not following this best practices