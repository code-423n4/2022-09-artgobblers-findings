QA Report
===

### Variable used before assignment causing erroneous event emmited

**Severity**: Low


In `requestRandomBytes()` in [ChainlinkV1RandProvider.sol](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L66), `requestId` is set as the return variable. It is then used (before being assigned a value) to emit the `RandomBytesRequested` event. On the following line, an explicit return is used.

```solidity
    /// @notice Request random bytes from Chainlink VRF. Can only by called by the ArtGobblers contract.
    function requestRandomBytes() external returns (bytes32 requestId) {
        // The caller must be the ArtGobblers contract, revert otherwise.
        if (msg.sender != address(artGobblers)) revert NotGobblers();

        emit RandomBytesRequested(requestId);

        // Will revert if we don't have enough LINK to afford the request.
        return requestRandomness(chainlinkKeyHash, chainlinkFee);
    }

```

**Impact**

An incorrect event is emitted and logged permanently.

**Recommendation**: 
Fix it
```diff
@@ -63,10 +63,9 @@ contract ChainlinkV1RandProvider is RandProvider, VRFConsumerBase {
         // The caller must be the ArtGobblers contract, revert otherwise.
         if (msg.sender != address(artGobblers)) revert NotGobblers();

-        emit RandomBytesRequested(requestId);
-
         // Will revert if we don't have enough LINK to afford the request.
-        return requestRandomness(chainlinkKeyHash, chainlinkFee);
+        emit RandomBytesRequested(requestId = requestRandomness(chainlinkKeyHash, chainlinkFee));
+
     }
```

-----------------------

### Attacker can spam `mintReservedGobblers` for an annoying DoS attack

**Severity**: Low

[mintReservedGobblers()](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol) is a non-permissioned external function that can be called by anyone. If the function were called with 0, it would complete successfully, including a emitting an event at the end.

**Impact**
This would mostly just be annoying. If front end was listening to this event, it could possibly have an effect. It would clutter up the recent events and event log queries.

**Recommendation**: 
```diff
@@ -836,7 +828,9 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
     /// the supply of goo minted gobblers and the supply of gobblers minted to reserves.
     function mintReservedGobblers(uint256 numGobblersEach) external returns (uint256 lastMintedGobblerId) {
+        require(numGobblersEach > 0, "CAN'T MINT ZERO");
```

-----------------------

### Use a less buggy version of Solidity

**Severity**: Low

[foundry.toml](https://github.com/code-423n4/2022-09-artgobblers/blob/main/foundry.toml#L2) has the compiler set to 0.8.13.

Solidity versions 0.8.15 - 0.8.17 fix several bugs introduced in earlier versions of Solidity, 2 of which were introduced in 0.8.13.


**Impact**

None of the fixed bugs were deemed to directly effect the ArtGobblers contracts.


**Recommendation**: 

It is recommended to upgrade to the latest version of Solidity, or else take a more conservative approach and use an older version such as 0.8.4 which includes many needed features, but has been around longer and can be deemed more stable.

