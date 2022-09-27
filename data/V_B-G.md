### 1. Redundant memory slot in LibString library

There is function `toString` in `LibString` library. There 5 storage slots are used for the next purposes:

```solidity
// The maximum value of a uint256 contains 78 digits (1 byte per digit), but we allocate 160 bytes
// to keep the free memory pointer word aligned. We'll need 1 word for the length, 1 word for the
// trailing zeros padding, and 3 other words for a max of 78 digits. In total: 5 * 32 = 160 bytes.
```

Actually, the last (fifth) storage slot is redundant, it is filled with zero value and never used later:

```solidity
// Clean the last word of memory it may not be overwritten.
mstore(str, 0)
```

It is reasonable to not allocate such a slot at all (and to not store there zero value) to reduce gas consumption and make the code more clear.

### 2. Using preincrement instead of postcrement in loops

There are many places where the index in a loop is incremented by the postincrement `i++`. The more efficient way to increment the variable is an `++i` preincrement. Also, the incrementation of variables can be put in `unchecked` brackets.

### 3. Using `immutables` for all unchangeable storage variables

Although the `immutable` keyword is used for many of the variables it is not used in all variables where it can be so. As an example, `BASE_URI` variable from `ArtGobblers` should be declared with `immutable` keyword.