# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1      | Immutable state variables lack zero value (address or uint) checks | Low | 7 |
| 2      | Risk of reentrancy in the `transferFrom` function from the `PagesERC721` contract | Low | 1 |

## Findings

### 1- Immutable state variables lack zero value (address or uint) checks  :

Constructors should check that the values written in an immutable state variables variable are not zero (for uint) or the zero address (for addresses).

#### Impact - Low Risk

#### Proof of Concept

Instances include:

File: src/ArtGobblers.sol

[mintStart = _mintStart;](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L311)

[team = _team;](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L316)

[community = _community;](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L317)

File: src/Goo.sol

[artGobblers = _artGobblers;](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L83)

[pages = _pages;](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L84)

File: src/Pages.sol

[mintStart = _mintStart;](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L177)

[community = _community;](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L181)

#### Mitigation
Add non-zero address checks in the constructors for the instances aforementioned.

### 2- Risk of reentrancy in the `transferFrom` function from the `PagesERC721` contract  :

When the owner calls the pages ERC721 transferFrom function the old approver address can reenter the function to modify the transfer receiver address, this is possible because the `transferFrom` function doesn't respect the CEI (check-effect-interact) best practice as the approver is deleted after assigning the new owner.

#### Impact - Low Risk

#### Proof of Concept

Instances occurs in the code below :

File: src/utils/token/PagesERC721.sol [Line 98-125](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L98-L125)

```
function transferFrom(
        address from,
        address to,
        uint256 id
    ) public {
        require(from == _ownerOf[id], "WRONG_FROM");

        require(to != address(0), "INVALID_RECIPIENT");

        require(
            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
        );

        // Underflow of the sender's balance is impossible because we check for
        // ownership above and the recipient's balance can't realistically overflow.
        unchecked {
            _balanceOf[from]--;

            _balanceOf[to]++;
        }

        _ownerOf[id] = to;

        delete getApproved[id];

        emit Transfer(from, to, id);
    }
```

The old approver is deleted after the ERC721 ownership has been transferred and thus the old approver can reenter the function to transfer the token from the new owner to another address.

#### Mitigation

The `transferFrom` function should be modified as follow :

```
function transferFrom(
        address from,
        address to,
        uint256 id
    ) public {
        require(from == _ownerOf[id], "WRONG_FROM");

        require(to != address(0), "INVALID_RECIPIENT");

        require(
            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
        );

        // Underflow of the sender's balance is impossible because we check for
        // ownership above and the recipient's balance can't realistically overflow.
        unchecked {
            _balanceOf[from]--;

            _balanceOf[to]++;
        }

        // @audit delete the old approver first
        delete getApproved[id];

        _ownerOf[id] = to;

        emit Transfer(from, to, id);
    }
```
