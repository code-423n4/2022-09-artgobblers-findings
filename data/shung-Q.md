# QA Report

## L1: Pages.sol constructor lacks input sanitization

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L177

It is possible to set the `mintStart` to zero. Such a mistake would result in much lower price for Pages NFTs. Consider at least ensuring that the `mintStart` is greater or equal to block time.

## L2: Owned.sol lacks two-step ownership transfer

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L39-L43

The current one-step ownership transfer method is prone to irreversible user errors. The owner can by mistake send the ownership of the contract to a wrong address, losing all the funds in the contract (in this case, the gobblers in the reserve contract). Consider having a pull method for a second step in ownership transfer where the new owner has to confirm to acquire the ownership.

## L3: VRGDA contracts accept negative values for some arguments

https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LinearVRGDA.sol#L30
https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/LogisticVRGDA.sol#L44-L49

`LinearVRGDA.perTimeUnit`, `LogisticVRGDA.timeScale`, and `LogisticVRGDA.logisticlimit` can all be initialized with negative values. Allthough likelyhood of such a mistake is low, if a deployer makes such a mistake, the VRGDA will work in reverse (i.e.: decreasing price as more sold, allowing all the supply to be gobbled up (pun intended) by quick users), which would be catastrophic. Consider checking these values to be positive in the constructors.

## L4: Unchecked math can overflow in VRGDA

https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L47-L50

Consider using checked math in `getVRGDAPrice()` function. Although you have noted that in practical use overflow would not happen, there could be issues due to dev or user mistakes originating from inheriting contracts. As this is an abstract contract, it should have as few assumptions as possible.

## I1: Convenience function to compound goo can be added

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L767-L787

Compounding goo rewards frequently would provide users a higher APY. However, the code does not have a function for updating goo balance just for the purpose of compounding. It instead has `addGoo` and `removeGoo` functions which can be used for the same effect, but they are inefficient because they make external calls. Having a dedicated compounding function can improve user experience and reduce fees. Example:

```solidity
function compoundGoo() external {
    updateUserGooBalance(msg.sender, 0, GooBalanceUpdateType.INCREASE);
}
```

## I2: Floating pragma used in GobblerReserve

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L2

`GobblerReserve.sol` is not an abstract contract. Therefore to have consistent bytecode among deployments it would be better to have a fixed pragma. Consider using `pragma solidity 0.8.13;` like the rest of the contract.

## I3: Inconsistent Pages per day

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L174

The documents state that 10 pages per day is targeted to be minted after the logistic to linear VRGDA switch. However, the value is hardcoded as 9 pages per day in the contract. Consider updating the documentation or fixing the contract.

## I4: Goo balance is updated without event emission

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L454-L455
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L660-L661
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L903-L904
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L910-L911

In `mintLegendaryGobbler()` and `transferFrom()` functions of `ArtGobblers.sol`, the goo balance (i.e.: `lastBalance` and `lastTimestamp` properties of user data) is updated without the emission of associated `GooBalanceUpdated()` event. For consistency, consider emitting this event any time goo balance is updated, not just when adding or removing goo.
