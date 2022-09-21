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

