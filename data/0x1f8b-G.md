- [Gas](#gas)
    - [**1. Don't use the length of an array for loops condition**](#1-dont-use-the-length-of-an-array-for-loops-condition)
    - [**2. Avoid compound assignment operator in state variables**](#2-avoid-compound-assignment-operator-in-state-variables)
        - [Total gas saved: **13 * 10 = 130**](#total-gas-saved-13--10--130)
    - [**3. Shift right or left instead of dividing or multiply by 2**](#3-shift-right-or-left-instead-of-dividing-or-multiply-by-2)
        - [Total gas saved: **172 * 1 + 201 * 1 = 373**](#total-gas-saved-172--1--201--1--373)
    - [**4. ++i costs less gas compared to i++ or i += 1**](#4-i-costs-less-gas-compared-to-i-or-i--1)
    - [**5. Use Custom Errors instead of Revert Strings to save Gas**](#5-use-custom-errors-instead-of-revert-strings-to-save-gas)
        - [Total gas saved: **9 * 36 = 324**](#total-gas-saved-9--36--324)
    - [**6. There's no need to set default values for variables**](#6-theres-no-need-to-set-default-values-for-variables)
        - [Total gas saved: **8 * 3 = 24**](#total-gas-saved-8--3--24)
    - [**7. Gas saving using immutable**](#7-gas-saving-using-immutable)
    - [**8. Change bool to uint256 can save gas**](#8-change-bool-to-uint256-can-save-gas)

# Gas

## **1. Don't use the length of an array for loops condition**

It's cheaper to store the length of the array inside a local variable and iterate over it.

**Affected source code:**

- [GobblerReserve.sol:37](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37)
- [GobblersERC1155B.sol:114](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L114)

## **2. Avoid compound assignment operator in state variables**

Using compound assignment operators for state variables (like `State += X` or `State -= X` ...) it's more expensive than using operator assignment (like `State = State + X` or `State = State - X` ...).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
uint private _a;
function testShort() public {
_a += 1;
}
}

contract TesterB {
uint private _a;
function testLong() public {
_a = _a + 1;
}
}
```

Gas saving executing: **13 per entry**

```
TesterA.testShort: 43507
TesterB.testLong:  43494
```

**Affected source code:**

- [ArtGobblers.sol:456](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L456)
- [ArtGobblers.sol:464](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L464)
- [ArtGobblers.sol:662](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L662)
- [ArtGobblers.sol:844](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L844)
- [ArtGobblers.sol:905](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L905)
- [ArtGobblers.sol:906](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L906)
- [ArtGobblers.sol:912](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L912)
- [ArtGobblers.sol:913](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L913)
- [Pages.sol:244](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L244)
- [GobblersERC721.sol:184](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L184)

### Total gas saved: **13 * 10 = 130**

## **3. Shift right or left instead of dividing or multiply by 2**

Shifting one to the right will calculate a division by two.

he `SHR` opcode only requires 3 gas, compared to the `DIV` opcode's consumption of 5. Additionally, shifting is used to get around Solidity's division operation's division-by-0 prohibition.

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.16;

contract TesterA {
function testDiv(uint a) public returns (uint) { return a / 2; }
}

contract TesterB {
function testShift(uint a) public returns (uint) { return a >> 1; }
}
```

Gas saving executing: **172 per entry**

```
TesterA.testDiv:    21965 
TesterB.testShift:  21793   
```

The same optimization can be used to multiply by 2, using the left shift.

```javascript
pragma solidity 0.8.16;

contract TesterA {
function testMul(uint a) public returns (uint) { return a * 2; }
}

contract TesterB {
function testShift(uint a) public returns (uint) { return a << 1; }
}
```

Gas saving executing: **201 per entry**

```
TesterA.testMul:    21994
TesterB.testShift:  21793    
```

**Affected source code:**

`*`:
- [ArtGobblers.sol:449](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L449)
`/`:
- [ArtGobblers.sol:462](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L462)

### Total gas saved: **(172 * 1) + (201 * 1) = 373**

## **4. `++i` costs less gas compared to `i++` or `i += 1`**

`++i` costs less gas compared to `i++` or `i += 1` for unsigned integers, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

`i++` increments `i` and returns the initial value of `i`. Which means:

```solidity
uint i = 1;
i++; // == 1 but i == 2
```

But `++i` returns the actual incremented value:

```solidity
uint i = 1;
++i; // == 2 and i == 2 too, so no need for a temporary variable
```

In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`
I suggest using `++i` instead of `i++` to increment the value of an uint variable. Same thing for `--i` and `i--`

*Keep in mind that this change can only be made when we are not interested in the value returned by the operation, since the result is different, you only have to apply it when you only want to increase a counter.*

**Affected source code:**

- [ERC721.sol:99](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L99)
- [ERC721.sol:101](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L101)
- [ERC721.sol:164](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L164)
- [ERC721.sol:179](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L179)
- [Pages.sol:251](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L251)
- [GobblerReserve.sol:37](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37)
- [PagesERC721.sol:115](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L115)
- [PagesERC721.sol:117](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L117)
- [PagesERC721.sol:181](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L181)

## **5. Use Custom Errors instead of Revert Strings to save Gas**

Custom errors from Solidity `0.8.4` are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

**Source Custom Errors in Solidity:**

Starting from Solidity `v0.8.4`, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testRevert(bool path) public view {
 require(path, "test error");
}
}

contract TesterB {
error MyError(string msg);
function testError(bool path) public view {
 if(path) revert MyError("test error");
}
}
```

Gas saving executing: **9 per entry**

```
TesterA.testRevert: 21611
TesterB.testError:  21602     
```

**Affected source code:**

- [VRGDA.sol:32](https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol#L32)
- [Owned.sol:20](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol#L20)
- [ERC721.sol:36](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L36)
- [ERC721.sol:40](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L40)
- [ERC721.sol:69](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L69)
- [ERC721.sol:87](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L87)
- [ERC721.sol:89](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L89)
- [ERC721.sol:91-94](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L91-L94)
- [ERC721.sol:118-123](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L118-L123)
- [ERC721.sol:134-139](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L134-L139)
- [ERC721.sol:158](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L158)
- [ERC721.sol:160](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L160)
- [ERC721.sol:175](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L175)
- [ERC721.sol:196-201](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L196-L201)
- [ERC721.sol:211-216](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L211-L216)
- [SignedWadMath.sol:142](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L142)
- [ArtGobblers.sol:437](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L437)
- [ArtGobblers.sol:885](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L885)
- [ArtGobblers.sol:887](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L887)
- [ArtGobblers.sol:889-892](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L889-L892)
- [GobblersERC1155B.sol:107](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L107)
- [GobblersERC1155B.sol:149-153](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L149-L153)
- [GobblersERC1155B.sol:185-189](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L185-L189)
- [GobblersERC721.sol:62](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L62)
- [GobblersERC721.sol:66](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L66)
- [GobblersERC721.sol:95](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L95)
- [GobblersERC721.sol:121-126](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L121-L126)
- [GobblersERC721.sol:137-142](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L137-L142)
- [PagesERC721.sol:55](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L55)
- [PagesERC721.sol:59](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L59)
- [PagesERC721.sol:85](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L85)
- [PagesERC721.sol:103](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L103)
- [PagesERC721.sol:105](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L105)
- [PagesERC721.sol:107-110](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L107-L110)
- [PagesERC721.sol:135-139](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L135-L139)
- [PagesERC721.sol:151-155](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L151-L155)

### Total gas saved: **9 * 36 = 324**

## **6. There's no need to set default values for variables**

If a variable is not set/initialized, the default value is assumed (0, `false`, 0x0 ... depending on the data type). You are simply wasting gas if you directly initialize it with its default value.

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testInit() public view returns (uint) { uint a = 0; return a; }
}

contract TesterB {
function testNoInit() public view returns (uint) { uint a; return a; }
}
```

Gas saving executing: **8 per entry**

```
TesterA.testInit:   21392
TesterB.testNoInit: 21384   
```

**Affected source code:**

- [Pages.sol:251](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L251)
- [GobblerReserve.sol:37](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37)
- [GobblersERC1155B.sol:114](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC1155B.sol#L114)
- [GobblersERC721.sol:186](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L186)
- [GobblerReserve.sol:37](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37)
- [ArtGobblers.sol:592](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L592)

### Total gas saved: **8 * 3 = 24**

## **7. Gas saving using `immutable`**

It's possible to avoid storage access a save gas using `immutable` keyword for the following variables:

It's also better to remove the initial values, because they will be set during the constructor.

**Affected source code:**

- [GobblersERC721.sol:23](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L23)
- [GobblersERC721.sol:25](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L25)
- [PagesERC721.sol:24](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L24)
- [PagesERC721.sol:26](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L26)
- [Pages.sol:96](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L96)
- [ERC721.sol:21](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L21)
- [ERC721.sol:23](https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L23)
- [ArtGobblers.sol:136](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L136)
- [ArtGobblers.sol:139](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L139)

## **8. Change `bool` to `uint256` can save gas**

Because each write operation requires an additional `SLOAD` to read the slot's contents, replace the bits occupied by the boolean, and then write back, `booleans` are more expensive than `uint256` or any other type that uses a complete word. This cannot be turned off because it is the compiler's defense against pointer aliasing and contract upgrades.

Reference:

- https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol#L23-L27

Also, this is applicable to integer types different from `uint256` or `int256`.

**Affected source code for `booleans`:**

- [ArtGobblers.sol:149](https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L149)