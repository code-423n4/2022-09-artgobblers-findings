1: ArtGobblers.sol may not be able to set a new RandProvider

if current VRF is sunset or invalid will change RandProvider by call upgradeRandProvider()
upgradeRandProvider() detect gobblerRevealsData.waitingForSeed!=true
But it is very possible that the old RandProvider is no longer valid and can no longer provide randomSeed again, resulting in waitingForSeed always being true, thus making it impossible to change the RandProvider
Suggest adding an expiration time
````
    function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
        // Revert if waiting for seed, so we don't interrupt requests in flight.
---     if (gobblerRevealsData.waitingForSeed) revert SeedPending();
+++     if (gobblerRevealsData.waitingForSeed) {
+++        if (block.timestamp <= gobblerRevealsData.nextRevealTimestamp + 1 days) revert SeedPending();
+++        gobblerRevealsData.waitingForSeed = false;
+++        gobblerRevealsData.toBeRevealed = 0;
        }

        randProvider = newRandProvider; // Update the randomness provider.

        emit RandProviderUpgraded(msg.sender, newRandProvider);
    }
```

2:
ChainlinkV1RandProvider.sol event use the wrong requestId
requestRandomBytes() first emit then set requestId

```
    function requestRandomBytes() external returns (bytes32 requestId) {
        // The caller must be the ArtGobblers contract, revert otherwise.
        if (msg.sender != address(artGobblers)) revert NotGobblers();

        //********** requestId always eq 0 ***********//
        emit RandomBytesRequested(requestId);

        // Will revert if we don't have enough LINK to afford the request.
        return requestRandomness(chainlinkKeyHash, chainlinkFee);
    }
```

Recommend:

```
    function requestRandomBytes() external returns (bytes32 requestId) {
       ....

+++     requestId = requestRandomness(chainlinkKeyHash, chainlinkFee);

        emit RandomBytesRequested(requestId);

        // Will revert if we don't have enough LINK to afford the request.
---     return requestRandomness(chainlinkKeyHash, chainlinkFee);
    }
```


3: ArtGobblers.sol show burnd gobbler's tokenURI

If it has been burned it should prompt NOT_MINTED

```
    function tokenURI(uint256 gobblerId) public view virtual override returns (string memory) {
        // Between 0 and lastRevealed are revealed normal gobblers.
        if (gobblerId <= gobblerRevealsData.lastRevealedId) { 
---         if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.
+++         if (gobblerId == 0 || getGobblerData[gobblerId].owner==address(0)) revert("NOT_MINTED");

            return string.concat(BASE_URI, uint256(getGobblerData[gobblerId].idx).toString());
        }
```        


L-04:

ArtGobblers.sol gobblerRevealsData.nextRevealTimestamp is used to limit the time between two reveals to not less than 1 day
However, when setting the next time, use the last time + 1 days, but if no one mint Gobbler, then the last time will be much smaller than yesterday, especially at the beginning
It is recommended to change it to the current time + 1 days


```
    function requestRandomSeed() external returns (bytes32) {
    ...
---      gobblerRevealsData.nextRevealTimestamp = uint64(nextRevealTimestamp + 1 days);
+++      gobblerRevealsData.nextRevealTimestamp = uint64(block.timestamp + 1 days);        
```