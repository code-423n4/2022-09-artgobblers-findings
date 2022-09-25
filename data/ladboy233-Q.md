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

## GobblerReserve.sol does not implement onERC721Received Hook

GobblerReserver.sol is minted to receive Art Gobbler ERC721 token. 

It is a good practice for a smart contract to implement onERC721Received hook

in safeTransfer is used. 

https://docs.openzeppelin.com/contracts/3.x/api/token/erc721

## Use multiply symbol instead of left shift operator in function mintReservedGobblers in Art Gobbler

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L844

we recommand change the line

```
 uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
```

to 

```
 uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach * 2);
```

## updated the state variable before event emission

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L353

instead of 

```
       unchecked {
            // Overflow should be impossible due to supply cap of 10,000.
            emit GobblerClaimed(msg.sender, gobblerId = ++currentNonLegendaryId);
        }
```

we recommand move ++currentNonLegenaryId before the event mission for better readability and conciseness.

```
       unchecked {
            // Overflow should be impossible due to supply cap of 10,000.
            gobblerId = ++currentNonLegendaryId
            emit GobblerClaimed(msg.sender,   gobblerId);
        }
```

same for the event mission in mintLegendary 

```
function mintLegendaryGobbler
```

we can change 

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L441

```
    emit Transfer(msg.sender, getGobblerData[id].owner = address(0), id);
```

to 

```
    delete getGobblerData[id];
    emit Transfer(msg.sender, address(0), id);
```
