# QA Findings (low / non-critical)
## [Q-1] Gobblers Can Gobble Themselves Via Wrappers
**Affected files:**
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L736)

**Description:**
The `gobble` method attempts to block Gobbler's gobbling other gobblers, however this can be circumvented by creating a Gobbler wrapping contract, bypassing the `msg.sender == address(this)` check.

**Recommendation:**
This issue is relatively hard to mitigate without negatively impacting the immutability of Gobblers. However if the inability for Gobblers to gobble other Gobblers is strongly desired novel subjective dispute resolution mechanisms aka "decentralized courts"  like Kleros may be used to enforce such restrictions without too heavily impacting the immutability and decentralization of Gobblers.

## [Q-2] `legendaryGobblerPrice` Reports Price Even After Legendary Gobblers Sell Out
**Affected files:**
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L478)

**Description:**
The `legendaryGobblerPrice` method still reports a price even after all 10 legendary Gobblers have been sold. This may lead to unforeseen consequences or even bugs in downstream applications such as e.g. price oracles who may directly depend on the value.

**Recommendation:**
It is recommended the `legendaryGobblerPrice` method be changed so that it either returns `type(uint256).max` once all Gobblers have sold out to indicate they can no longer be purchased with Gobblers, or that the method simply reverts. The method could also be left as is as this has no direct effect on the `ArtGobblers` contract itself, however this fact should then definitely be documented to ensure developers can plan for any potential resulting edge cases in their applications.

## [Q-3] Randomness Provider Upgrade Can Be Temporarily Blocked and Delayed 
**Affected files:**
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L560)

**Description:**
If the owner of the `ArtGobblers` contract attempts a random provider upgrade an attacker may revert the upgrade transaction and temporarily prevent it from being called for a short "blackout window". This can be achieved by front-running calls to `upgradeRandProvider` with a call to `requestRandomSeed`.  Exploiting this also requires that the `requestRandomSeed` cooldown has expired.

The upgrade blackout window can open any time as soon the reveal cooldown expires and lasts from when the new seed is requested until that request is fulfilled. The upgrade blackout window may be extended by an attacker via block stuffing or other attacks against the randomness provider mechanism or even the Ethereum network itself.

This may not be so critical if an upgrade is required because e.g. Chainlink VRF v1 is sunset which would likely come with days if not months of advanced notice. However in a scenario where a critical vulnerability is discovered in the randomness provider it may be much more critical to upgrade it in a timely manner, an added delay of even a couple blocks could allow an attacker to cause substantial additional damage depending on the vulnerability discovered.

Note on severity: Due to the requirement for a particular class of randomness provider vulnerabilities which do not seem to currently exist in Chainlink VRF v1 this issue has been deemed as "low".
 
**Recommendation:**
Require new randomness always be requested in two steps: 1. inititate seed request 2. request seed. A minimum delay between the two steps should be enforced. This will ensure that as long as the seed request has not been initiated the owner can easily complete a randomness provider upgrade without worrying about race conditions. Note that this is still vulnerable to block stuffing attacks. Alternatively always allow the owner to upgrade the randomness provider regardless of whether an existing request is in progress, this comes at the tradeoff of allowing the owner to intervene in existing requests.

## [Q-4] NFT-Based Authentication Limited To Direct Ownership Check
**Affected files:**
- `src/ArtGobblers.sol` [L437](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L437),  [L733](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L)

**Description:**
The `gobble` and `mintLegendaryGobbler` methods require callers to be the direct owners of the Gobblers they wish to engage in the given method and no not consider token or operator approvals. Checking approvals as fallback to direct ownership is recommended as it allows integrating smart contracts to more easily and cheaply perform operations on behalf of users because contracts do not have to transfer the Gobbler tokens to themselves first and subsequently return them.

**Recommendation:**
Use a check similar to L890, first checking direct ownership and falling back to approval in the `gobble`  and `mintLegendaryGobbler` methods. Consider renaming or adding fields to the `ArtGobbled` events so that the operator / owner at the time can be discerned. Furthermore, for this change in the `mintLegendaryGobbler` ensure that the owners' (not the operators') `lastBalance`, `lastTimestamp`, `emissionMultiple` and `gobblersOwned` are changed in the loop as the caller i.e. operator may only have approval and subsequently be using Gobblers from multiple different owners.
 
## [Q-5] Non-Standard Naming 
**Affected files:**
- [`src/utils/rand/RandProvider.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/RandProvider.sol)
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L813)

**Description:**
- `src/utils/rand/RandProvider.sol`: Interface name and file `RandProvider` not prefixed with 'I'
- `src/ArtGobblers.sol`: Internal method `updateUserGooBalance` not prefixed with underscore to indicate it's internal.

**Recommendation:**
It is recommended to follow typical naming conventions for the sake of improved readability unless there's some express purpose for the unconventional style in the two instances.

## [Q-6] Lack Of `multicall`
**Affected files:**
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol)
- [`src/Pages.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol)
- [`src/Goo.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol)

**Description:**
The listed files do not implement a `multicall` method. This is highly recommended for contracts that do not interact directly with ETH. While the only downside is an increase in code size, it does have potential to massively improve UX by allowing EOAs to do multiple actions on the contracts in a single transaction.

**Recommendation:**
Implement a `multicall` method for the mentioned contracts. The OpenZeppelin contracts or solady libraries have good implementations in this regard.


## [Q-7] Docs / Code Discrepancy
**Affected files:**
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol )
- [`README.md`](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/README.md?plain=1#L213)

**Description:**
On L213 the readme claims that "Art Gobblers themselves are fully animated ERC1155 NFTs." The `ArtGobblers` contract however only implements the ERC721 standard and not ERC1155.

**Recommendation:**
Correct this discrepancy and remove any unused files such as `src/utils/token/GobblersERC1155B.sol` .
 
## [Q-8] `Owned` Mixin Implements ERC173 Functionality But Does Not Adhere To The Standard
**Affected files:**
- [`lib/solmate/src/auth/Owned.sol`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol)
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol )
- [`src/utils/GobblerReserve.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol)

**Description:**
The `Owned` contract and subsequently the contracts that inherit from it implement [ERC173](https://eips.ethereum.org/EIPS/eip-173) functionality (single owner address, transferability, emitted event upon transfer) but do not adhere to its signatures. 

**Recommendation:**
It is recommended to comply with the ERC173 standard as it comes at no added cost but does improve potential interoperability with other applications. Rename the `setOwner` method to `transferOwnership` and the `OwnerUpdated` event to  `OwnershipTransferred` in order to be ERC173 compliant.

## [Q-9] `GobblerReserve` Cannot Accept Gobblers Sent As A Safe Transfer
**Affected files:**
- [`src/utils/GobblerReserve.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol)

**Description:**
Considering `GobblerReserve` is meant to hold Gobblers on behalf of others it may be wise to allow it to receive Gobblers via safe transfers depending on how it is intended for users to potentially use this contract.

**Recommendation:**
Implement the ERC721 `onERC721Received` hook and check that the contract is the Art Gobblers to ensure that users can deposit Gobblers into reserve contracts.

## [Q-10] Inconsistent Error Style
**Affected files:**
Overall repo including but not limited to:
- [`src/ArtGobblers.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol )
- [`src/utils/token/GobblersERC721.sol`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol)

**Description:**
Throughout the repo both custom errors and the default string errors (`Error(string)`) are used. 

**Recommendation:**
For the sake of consistency and readability it is recommended one style of errors is chosen and kept consistent throughout the repo.