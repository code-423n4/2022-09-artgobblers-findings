1. User can't got their Remaining Legendary Gobblers

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L415

the only way to set them to got their max number of remaining legendary gobblers by set `gobblerId >= MAX_SUPPLY`
to ensure that NoRemainingLegendaryGobblers() so if the Max_Supply was reach it it would revert immediately.

2. Innacurate Imported file 

Files :

1.) https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L62-L71

Checked if some of file directory has changed/innacurate/it can't found so it should better to change and u must update them and adding/import some contract to avoid running/error :

```
import {Owned} from "solmate/auth/Owned.sol"; //@audit
import {ERC721} from "solmate/tokens/ERC721.sol"; //@audit
import {LibString} from "solmate/utils/LibString.sol"; //@audit
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol"; //@audit
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol"; //@audit
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol"; //@audit 
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol"; //@audit lib/solmate/src/utils/SignedWadMath.sol;


import {LibGOO} from "goo-issuance/LibGOO.sol"; //@audit import {LibGOO} from "lib/goo-issuance/src/LibGOO.sol";
import {LogisticVRGDA} from "VRGDAs/LogisticVRGDA.sol"; //@audit import {LogisticVRGDA} from "lib/VRGDAs/src/LogisticVRGDA.sol";
```

Make sure that directory of each one of contract was correct 

2.)  https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Goo.sol#L4

```
import {ERC20} from "lib/solmate/src/tokens/ERC20.sol";
```

3. The contract name was innacurate

https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol

It should be `DeployBase.sol` rather than `DeployBase.s.sol` (since it was quote on contract)

Same Case : 

https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol

`DeployRinkeby.sol`

4. Locked Pragma Compiler

File : 

1.) https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol
2.) https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol

Since it was used >=0.8.0. As the compiler can be use for example 0.8.14 and consider locking at this version the same as another. It can be consider using  locking the pragma version whenever possible and avoid using a floating pragma in the final deployment. Since it can be problematic, if there are publicly disclosed bugs and issues that affect the current compiler version used.

