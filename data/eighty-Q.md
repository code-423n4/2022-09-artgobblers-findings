# Transparency Issue

As stated in [VRF Security Considerations [V1]](https://docs.chain.link/docs/vrf/v1/security/#use-requestid-to-match-randomness-requests-with-their-fulfillment-in-order), the `requestId` plays an important role in ensuring fairness and prevent abuse of multiple requests to the oracle. Since the main component ([ArtGobblers.sol#L541](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L541)) doesn't actively manage the `requestId` returned, preventing fairness may be delegated to third parties monitoring the contract. Thus, the misused named return variable in [src/utils/rand/ChainlinkV1RandProvider.sol#L62-L70](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L62-L70) (code below) results in an `0x00` event:

```solidity
function requestRandomBytes() external returns (bytes32 requestId) {
    // The caller must be the ArtGobblers contract, revert otherwise.
    if (msg.sender != address(artGobblers)) revert NotGobblers();

    emit RandomBytesRequested(requestId);

    // Will revert if we don't have enough LINK to afford the request.
    return requestRandomness(chainlinkKeyHash, chainlinkFee); 
}
```

## Recommendations

```solidity
function requestRandomBytes() external returns (bytes32 requestId) {
    // The caller must be the ArtGobblers contract, revert otherwise.
    if (msg.sender != address(artGobblers)) revert NotGobblers();

    // Will revert if we don't have enough LINK to afford the request.
    requestId = requestRandomness(chainlinkKeyHash, chainlinkFee);
    
    emit RandomBytesRequested(requestId);
}
```