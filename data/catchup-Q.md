# Low Risk Issues


## 1. Missing non-zero address checks when setting important addresses

It is a good practice to include 0 address check when setting important addresses. There are pretty much no zero address checks anywhere. Some of the lines are listed below:

### Lines of code
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L314-L318
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83-L84
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L179-L181



# Non-Critical Issues


## 1. Missing indexed fields

Each event can have up to 3 indexed fields.
I believe it would be good to have the ```uint256 lastMintedGobblerId``` indexed in the below events, considering ```uint256 gobblerId``` was indexed right before that:
~~~
233:     event LegendaryGobblerMinted(address indexed user, uint256 indexed gobblerId, uint256[] burnedGobblerIds);
234:     event ReservedGobblersMinted(address indexed user, uint256 lastMintedGobblerId, uint256 numGobblersEach);
~~~
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L233-L234

Same is true for ```lastMintedPageId```
~~~
134:     event PagePurchased(address indexed user, uint256 indexed pageId, uint256 price);
135: 
136:     event CommunityPagesMinted(address indexed user, uint256 lastMintedPageId, uint256 numPages);   //@audit-issue none lastMintedPageId can be indexed. above pageId is indexed
~~~
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L134-L136



## 2. Floating pragma

All the contracts are using floating pragma of >=0.8.0
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
It is recommended to use the same solidity version for all the contracts.

References:
https://swcregistry.io/docs/SWC-103
https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used



## 3. Missing natspec comment

All the parameters have their natspec comment except for ```Pages _pages```
```
278:     /// @notice Sets VRGDA parameters, mint config, relevant addresses, and URIs.
279:     /// @param _merkleRoot Merkle root of mint mintlist.
280:     /// @param _mintStart Timestamp for the start of the VRGDA mint.
281:     /// @param _goo Address of the Goo contract.    //@audit-issue none tum parametrelerin natspeci var ama _pages yok.
282:     /// @param _team Address of the team reserve.
283:     /// @param _community Address of the community reserve.
284:     /// @param _randProvider Address of the randomness provider.
285:     /// @param _baseUri Base URI for revealed gobblers.
286:     /// @param _unrevealedUri URI for unrevealed gobblers.
287:     constructor(
288:         // Mint config:
289:         bytes32 _merkleRoot,
290:         uint256 _mintStart,
291:         // Addresses:
292:         Goo _goo,
293:         Pages _pages,
294:         address _team,
295:         address _community,
296:         RandProvider _randProvider,
297:         // URIs:
298:         string memory _baseUri,
299:         string memory _unrevealedUri
300:     )
```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L278-L300








