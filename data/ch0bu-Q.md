# NON-CRITICAL

## 1. Floading Pragma version


In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Proof of Concept
https://swcregistry.io/docs/SWC-103

```
All contracts
```

## 2. Use a more recent version of Solidity


- Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
- Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>) Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)

```
All contracts
```

## 3. Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom


It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Reference: This similar medium-severity finding from Consensys Diligence Audit of Fei Protocol: https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call


https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol
```
38	artGobblers.transferFrom(address(this), to, ids[i]);
```

## 4. File is missing NatSpec

```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol
```


## 5. NatSpec is incomplete

Some files are missing @title, @author, @dev or other tags

```
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/LibString.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol
```
