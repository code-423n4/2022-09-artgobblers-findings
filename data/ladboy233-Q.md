## The compiler version may not support customized error between version 0.8.4

All contracts use the pragma version

```
// SPDX-License-Identifier: AGPL-3.0-only
pragma solidity >=0.8.0;
```
the customized error are extensively used in the code

```
    error InvalidProof();
    error AlreadyClaimed();
    error MintStartPending();
```

(customized error in ArbGobbler.sol)

however, customized errors are not supported until solidity version 0.8.4

https://github.com/ethereum/solidity/releases/tag/v0.8.4

So using the solidity version between 0.8.4 and 0.8.0 can result in compiler error.

We recommand the project use the up-to-date solidity version.

```
// SPDX-License-Identifier: AGPL-3.0-only
pragma solidity >=0.8.4;
```

## Use SafeMint instead of mint when minting NFT.

https://ethereum.stackexchange.com/questions/115280/mint-vs-safemint-which-is-best-for-erc721

If YOU are paying for the minting of tokens, use _mint. The _safeMint might cost you an arbitrary amount of money because of choices made by the recipient of the tokens. This is enough to deter you from considering it.

If THEY are paying for the minting of tokens and you expect buyers to be composing functionality with smart contracts, use _safeMint. There is some marginal benefit of allowing the extra features with smart contracts this allows.

## Page NFT do not have owner

the Page NFT do not have owner, which means it cannot be listed on opensea or looksRare,

We recommand the project add owner for Page NFT.

## function Gobble miss the check that if the msg.sender is approved by NFT owner

in the function Gobble

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L723

```
    function gobble(
        uint256 gobblerId,
        address nft,
        uint256 id,
        bool isERC1155
    ) external {
        // Get the owner of the gobbler to feed.
        address owner = getGobblerData[gobblerId].owner;

        // The caller must own the gobbler they're feeding.
        if (owner != msg.sender) revert OwnerMismatch(owner);
```

note the check 

```
        if (owner != msg.sender) revert OwnerMismatch(owner);
```

it only check if the owner is the msg.sender, but does not check if msg.sender is approved by NFT owner like other transfer function is handled,

we recommend the project add the check below

```
if(owner != msg.sender || !isApprovedForAll(owner, msg.sender) || getApproved(tokenId) != msg.sender) revert OwnerMismatch(owner);
```


