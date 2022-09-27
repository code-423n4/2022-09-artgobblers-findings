### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [N-01] | Include `return parameters` in _NatSpec comments_ | 14 |
| [N-02] | ```0``` address check | 9 |
| [N-03] | Long lines are not suitable for the ``Solidity Style Guide`` | 1 |
| [N-04] | _Inconsistent_ NatSpec desciption  |  |

Total 4 issues


### Low Risk Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [L-01] | Add to _blacklist function_ | |
| [L-02] | _Missing supportsInterface_ | 3 |
| [L-03] | _Lock pragmas_ to specific compiler version |  |
| [L-04] | Avoid _shadowing_ `inherited state variables` | 1 |

Total 4 issues

## [N-01] Include ``return parameters`` in _NatSpec comments_

**Context:**
[Pages.sol#L265](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L265)
[Pages.sol#L239](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L239)
[Pages.sol#L219](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L219)
[ArtGobblers.sol#L872](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L872)
[ArtGobblers.sol#L866](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L866)
[ArtGobblers.sol#L839](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L839)
[ArtGobblers.sol#L757](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L757)
[ArtGobblers.sol#L693](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L693)
[ArtGobblers.sol#L509](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L509)
[Functions with all return in GobblersERC1155B.sol ](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol)
[Functions with all return in GobblersERC721.sol ](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol)
[Functions with all return in PagesERC721.sol ](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol)
[ChainlinkV1RandProvider.sol#L62](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L62)
[Functions with all return in SignedWadMath.sol](https://github.com/code-423n4/2022-09-artgobblers/tree/main/lib)

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

If Return parameters are declared, you must prefix them with "/// @return".

Some code analysis programs do analysis by reading NatSpec details, if they can't see the "@return" tag, they do incomplete analysis.

**Recommendation:**
Include return parameters in NatSpec comments

Current Code:
 ```js
src/Pages.sol#L265

    /// @notice Returns a page's URI if it has been minted.
    /// @param pageId The id of the page to get the URI for.
    function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

        return string.concat(BASE_URI, pageId.toString());
    }
```
Recommendation Code:
 ```js
    /// @notice information about what a function does
    /// @param pageId The id of the page to get the URI for.
    /// @return Returns a page's URI if it has been minted 
    function tokenURI(uint256 pageId) public view virtual override returns (string memory) {
        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

        return string.concat(BASE_URI, pageId.toString());
    }
```

## [N-02] ```0``` address check

**Context:**
[ArtGobblers.sol#L294](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L294)
[ArtGobblers.sol#L295](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L295)
[ArtGobblers.sol#L292](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L292)
[ArtGobblers.sol#L293](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L293)
[ArtGobblers.sol#L296](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L296)
[Goo.sol#L82](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L82)
[Pages.sol#L160](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L160)
[Pages.sol#L161](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L161)
[Pages.sol#L162](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L162)

**Description:**
Also check of the address to protect the code from 0x0 address  problem just in case. This is best practice or instead of suggesting that they verify address != 0x0, you could add some good natspec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address.

**Recommendation:**
like this ;
```if (_team == address(0)) revert ADDRESS_ZERO();```

## [N-03] Long lines are not suitable for the ``Solidity Style Guide``

**Context:**
[ArtGobblers.sol#L499](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L499)

**Description:**
It is generally recommended that lines in the source code should not exceed 80-120 characters. Today's screens are much larger, so in some cases it makes sense to expand that. The lines above should be split when they reach that length, as the files will most likely be on GitHub and GitHub always uses a scrollbar when the length is more than 164 characters.

(why-is-80-characters-the-standard-limit-for-code-width)[https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width]

**Recommendation:**
Multiline output parameters and return statements should follow the same style recommended for wrapping long lines found in the Maximum Line Length section.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html#introduction

```js
thisFunctionCallIsReallyLong(
    longArgument1,
    longArgument2,
    longArgument3
);
```

## [N-04] _Inconsistent_ Natspec description

**Description:**
One of the ```Pages.sol``` contract constructor arguments is ```address _community``` and NatSpec explanation : 
"/// @param _community Address of the community reserve"

One of the ```ArtGobblers.sol``` contract constructor arguments is ```address _community``` and NatSpec explanation : 
"/// @param _community Address of the community reserve"

However, the addresses defined during these __two constructor a deploy are different.__

**Recommendation:**
Update comments or addresses.


## [L-01] Add to _blacklist function_

**Description:**
Cryptocurrency mixing service, Tornado Cash, has been blacklisted in the OFAC.
A lot of blockchain companies, token projects, NFT Projects have ```blacklisted``` all Ethereum addresses owned by Tornado Cash listed in the US Treasury Department's sanction against the protocol.
https://home.treasury.gov/policy-issues/financial-sanctions/recent-actions/20220808
In addition, these platforms even ban accounts that have received ETH on their account with Tornadocash.

Some of these Projects;
 USDC (https://www.circle.com/en/usdc)
 Flashbots (https://www.paradigm.xyz/portfolio/flashbots )
 Aave (https://aave.com/)
 Uniswap
 Balancer
 Infura
 Alchemy 
 Opensea
 dYdX 

Details on the subject;
https://twitter.com/bantg/status/1556712790894706688?s=20&t=HUTDTeLikUr6Dv9JdMF7AA

https://cryptopotato.com/defi-protocol-aave-bans-justin-sun-after-he-randomly-received-0-1-eth-from-tornado-cash/

For this reason, every project in the Ethereum network must have a blacklist function, this is a good method to avoid legal problems in the future, apart from the current need.

Transactions from the project by an account funded by Tonadocash or banned by OFAC can lead to legal problems.Especially American citizens may want to get addresses to the blacklist legally, this is not an obligation

If you think that such legal prohibitions should be made directly by validators, this may not be possible:
https://www.paradigm.xyz/2022/09/base-layer-neutrality

```The ban on Tornado Cash makes little sense, because in the end, no one can prevent people from using other mixer smart contracts, or forking the existing ones. It neither hinders cybercrime, nor privacy.```

Here is the most beautiful and close to the project example; Manifold

Manifold Contract
https://etherscan.io/address/0xe4e4003afe3765aca8149a82fc064c0b125b9e5a#code

```js
     modifier nonBlacklistRequired(address extension) {
         require(!_blacklistedExtensions.contains(extension), "Extension blacklisted");
         _;
     }
```
Recommended Mitigation Steps add to Blacklist function and modifier.


## [L-02] _Missing supportsInterface_

**Context:**
[GobblersERC1155B.sol#L124](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L124)
[GobblersERC721.sol#L149](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L149)
[PagesERC721.sol#L162](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L162)

**Description:**
ERC721TokenReceiver interface ID is missing in supportInterface

```js
 function supportsInterface(bytes4 _interfaceId) external pure returns (bool) {
        return
            _interfaceId == 0x01ffc9a7 || // ERC165 Interface ID
            _interfaceId == 0x80ac58cd || // ERC721 Interface ID
            _interfaceId == 0x5b5e139f; // ERC721Metadata Interface ID
    }
```

**Recommendation:**
```js
 function supportsInterface(bytes4 _interfaceId) external pure returns (bool) {
        return
            _interfaceId == 0x01ffc9a7 || // ERC165 Interface ID
            _interfaceId == 0x80ac58cd || // ERC721 Interface ID
            _interfaceId == 0x150b7a02 || // ERC721TokenReceiver Interface ID
            _interfaceId == 0x5b5e139f; // ERC721Metadata Interface ID
    }
```

## [L-03] _Lock pragmas_ to specific compiler version

**Description:**
Pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the developer would need to manually update the pragma in order to compile locally. 
https://swcregistry.io/docs/SWC-103

**Recommendation:**
Ethereum Smart Contract Best Practices - Lock pragmas to specific compiler version.
[solidity-specific/locking-pragmas](https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/)

## [L-04] Avoid _shadowing_ `inherited state variables`

**Context:**
[ArtGobblers.sol#L730](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L730)

**Description:**
In ```ArtGobblers.sol#L730``` , there is a local variable named ```owner```, but there is a state varible named ```owner``` in the inherited ```Owned.sol``` with the same name. This use causes compilers to issue warnings, negatively affecting checking and code readability. 

```js
function gobble(
        uint256 gobblerId,
        address nft,
        uint256 id,
        bool isERC1155
    ) external {
        address owner = getGobblerData[gobblerId].owner;
        // Codes...
```

**Recommendation:**
Avoid using variables with the same name, including inherited in the same contract, if used, it must be specified in the NatSpec comments.

## Suggestions

[S-01] 
Gobblers NFTs are "Art Gobblers themselves are fully animated ERC1155 NFTs." it is mentioned as ERC721 using GobblersERC721.sol file in the project, documents should be corrected

[S-02] 
The getTargetSaleTime function in the VRGDA.sol file does not have a body, the reason for this preference is not clear, at least it should broadcast br emit
```js
function getTargetSaleTime(int256 sold) public view virtual returns (int256);
```

[S-03] 
For code readability, add real numbers as comments with "//" next to scientific notation.
Recommendation;
```js
 GobblersERC721("Art Gobblers", "GOBBLER")
        Owned(msg.sender)
        LogisticVRGDA(
            69.42e18, // Target price.                // 69420000000000000000
            0.31e18, // Price decay percent.     // 310000000000000000
            // Max gobblers mintable via VRGDA. 
            toWadUnsafe(MAX_MINTABLE),
            0.0023e18 // Time scale.                 // 2300000000000000
```


[S-04] 
Natspec comments are missing the return tag, add it as in all other functions. [ArtGobblers.sol#L509](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L509)

[S-05] 
For code readability, make sure what the numbers look like below, at least specify "//" in comments
Recommendation;
```js
MAX_SUPPLY       = 10000;
MINTLIST_SUPPLY  = 2000;
LEGENDARY_SUPPLY = 10;
RESERVED_SUPPLY  = 1598 //(MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
MAX_MINTABLE     = 6392 // MAX_SUPPLY- MINTLIST_SUPPLY- LEGENDARY_SUPPLY- RESERVED_SUPPLY;
```

[S-06] 
ChainlinkV1RandProvider.sol file tests could not be seen, because it is an external call, rinkeby tests must be done and added to the project. The Virtual Test environment is insufficient for such tests.
