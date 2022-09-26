# Gas Optimization Report

1. `Public functions` not called by the contract should be declared `external` instead
Calls to external functions are cheaper than public functions. Thus, if a function is not used internally in any contract, it should be set to external to save gas and improve code readability.

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L693
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L265


2. It costs more gas to initialize variables with their default value than letting the default value be applied

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L592
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L251
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37


3. `x = x + y` is cheaper than `x += y` (same for `x = x - y` is cheaper than `x -= y`)

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L439
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L456
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L464
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L662
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L905
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L906
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L912
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L913


4. `array.length` should not be looked up in every loop of a for-loop

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37


5. `++i` costs less gas than `i++`, especially when it’s used in for-loops

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37

