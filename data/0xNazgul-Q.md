## [NAZ-L1] Gobblers Can Be Force Feed Other Gobblers
**Severity** Low
**Context**: [`ArtGobblers.sol#L260`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L260), [`ArtGobblers.sol#L736`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L736)

**Description**:
On #L260 there is `error Cannibalism();` leading to that Gobblers should not eat other Gobblers. However, Gobblers can be wrapped into a new token and be force feed to other Gobblers.

**Proof of Concept**:
1. Jobbler 'The Cannibal' Metheny has two Gobblers.
2. Jobbler wraps one of his Gobblers into a Gobburger (Gobbler Burger a work of art in his sick mind) and invites his other Gobbler to a BBQ.
3. Jobbler feeds his wrapped Gobburger to the unsuspecting Gobbler.

**Recommendation**:
No real way to stop a user from doing this. Should however be outlined in the documentation so that users/developers can know that this can still happen.


## [NAZ-L2] Lack of Two-Step Process for Critical Operations
**Severity** Low
**Context**: [`Owned.sol#L39`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L39)

**Description**:
This function transfers the ownership of the contract in a single step. There is no way to reverse a one-step transfer of ownership to an address without an owner. This would not be the case if ownership were transferred through a two-step process in which an owner proposed a transfer and the prospective recipient accepted it.

**Recommendation**:
Consider using a two-step process for ownership transfers. 


## [NAZ-L3] Missing Equivalence Checks in Setters
**Severity**: Low
**Context**: [`Owned.sol#L39`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L39), [`ArtGobblers.sol#L560`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L560)

**Description**:
Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

**Recommendation**:
This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.


## [NAZ-L4] Missing Time locks
**Severity**: Low 
**Context**: [`Owned.sol#L39`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L39) 

**Description**:
When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert: (1) users and give them a chance to engage/exit protocol if they are not agreeable to the changes (2) team in case of compromised owner(s) and give them a chance to perform incident response.

**Recommendation**:
Users may be surprised when critical parameters are changed. Furthermore, it can erode users' trust since they can’t be sure the protocol rules won’t be changed later on. Compromised owner keys may be used to change protocol addresses/parameters to benefit attackers. Without a time-delay, authorized owners have no time for any planned incident response.


## [NAZ-L5] Missing Zero-address Validation
**Severity**: Low
**Context**: [`Owned.sol#L39`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L39), [`ArtGobblers.sol#L292-L296`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L292-L296), [`ArtGobblers.sol#L560`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L560), [`PagesERC721.sol#L37`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L37), [`Pages.sol#L160-L162`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L160-L162), [`ChainlinkV1RandProvider.sol#L49-L51`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L49-L51), [`Goo.sol#L82`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L82), [`GobblerReserve.sol#L23`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L23)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-N1] Code Contains Empty Blocks
**Severity**: Informational
**Context**: [`DeployRinkeby.s.sol#L13-L15`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L13-L15), [`MerkleProofLib.sol#L23`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/MerkleProofLib.sol#L23), [`LibString.sol#L30`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/LibString.sol#L30)

**Description**:
It's best practice that when there is an empty block, to add a comment in the block explaining why it's empty.

**Recommendation**:
Consider adding `/* Comment on why */` to the empty blocks.


## [NAZ-N2] Line Length
**Severity**: Informational
**Context**: [`ArtGobblers.sol#L475-L476`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L475-L476), [`ArtGobblers.sol#L499`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L499)

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N3] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`ArtGobblers.sol#L136-L139`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L136-L139), [`ArtGobblers.sol#L813`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L813), [`Pages.sol#L96`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L96), [`Pages.sol#L122`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L122), [`Pages.sol#L128`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L128), [`ChainlinkV1RandProvider.sol#L27-L30`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L27-L30), [`ChainlinkV1RandProvider.sol#L73`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/ChainlinkV1RandProvider.sol#L73), [`DeployBase.s.sol#L18-L27`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol#L18-L27), [`DeployRinkeby.s.sol#L13-L15`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployRinkeby.s.sol#L13-L15), [`LogisticToLinearVRGDA.sol#L20-L28`](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticToLinearVRGDA.sol#L20-L28), [`LogisticVRGDA.sol#L30`](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L30), [`VRGDA.sol#L21`](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#l21), [`LinearVRGDA.sol#L19`](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol#L19), [`FixedPointMathLib.sol#L12`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L12)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N4] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`ArtGobblers.sol#L187`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L187), [`ArtGobblers.sol#L804`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L804), [`GobblersERC1155B.sol#L37`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L37), [`GobblersERC721.sol#L23-L25`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L23-L25), [`PagesERC721.sol#L24-L26`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L24-L26), [`Owned.sol#L17`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L17), [`ERC721.sol#L21-L23`](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L21-L23)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N5] Use Underscores for Number Literals
**Severity**: Informational
**Context**: [`ArtGobblers.sol#L112`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112), [`ArtGobblers.sol#L115`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115)

**Description**:
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

**Recommendation**:
Consider using underscores for number literals to improve its readability.


## [NAZ-N6] Left Shift Could Be Replaced By `mul`
**Severity**: Informational
**Context**: [`ArtGobblers.sol#L844`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L844)

**Description**:
It will increase readability for future developers and users.

**Recommendation**:
Consider using `mul` instead of the left shift


## [NAZ-N7] Inconsistent Use of Inline Delete
**Severity**: Informational
**Context**: [`ArtGobblers.sol#L441`](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L441)

**Description**:
There is an inconsistent use of inline delete in the event. It will increase readability for future developers and users if done outside the event.

**Recommendation**:
Consider Moving the inline delete outside of the event.


## [NAZ-N8] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-artgobblers)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.