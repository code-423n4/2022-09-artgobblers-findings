## No Need to Initialize Variables with Default Values
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). If you explicitly initialize it with its default value, you will be incurring more gas. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592

## += Costs More Gas
`+=` generally costs more gas than writing out the assigned equation explicitly. As an example, the following line of code could be rewritten as:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L439

```
                burnedMultipleTotal = burnedMultipleTotal + getGobblerData[id].emissionMultiple;
```

Similarly, as an example, the following line of code could be rewritten as:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L905

```
            getUserData[from].emissionMultiple  =  getUserData[from].emissionMultiple - emissionMultiple;
```
## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =). As an example, consider replacing >= with the strict counterpart > in the following line of code:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L435

```
                if (id > FIRST_LEGENDARY_GOBBLER_ID - 1) revert CannotBurnLegendary(id);
```
Similarly, as an example, consider replacing <= with the strict counterpart < in the following line of code:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L702

```
        if (gobblerId < currentNonLegendaryId + 1) return UNREVEALED_URI;
```
## Use Custom Errors Instead of Require to Save Gas
Consider replacing all require statements with custom errors which are cheaper both in deployment and runtime cost starting from Solidity 0.8.4. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L437
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L69

## ++i costs less gas compared to i++
++i costs less gas compared to i++ or i += 1 for unsigned integers considering the pre-increment operation is cheaper (about 5 GAS per iteration).

i++ increments i and makes the compiler create a temporary variable for returning the initial value of i. In contrast, ++i returns the actual incremented value without making the compiler do extra job. Here are some of the instances entailed:

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L164
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114

## calldata and memory
When running a function we could pass the function parameters as calldata or memory for variables such as strings, bytes, structs, arrays etc. If we are not modifying the passed parameter we should pass it as calldata because calldata is more gas efficient than memory. Here are some of the instances entailed:

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L207
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L161

