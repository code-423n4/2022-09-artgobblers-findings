# Gas Optimizations

## Findings

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | State variables only set in the constructor should be declared `immutable`     | 4 |
| 2      | Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead  |  4 |
| 3      | `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables  |  7 |
| 4      | Use custom errors rather than `require()`/`revert()` strings to save deployments gas  |  24 |
| 5      | It costs more gas to initialize variables to zero than to let the default of zero be applied    |  6  |
| 6      | Use of `++i` cost less gas than `i++` in for loops    |  2  |
| 7      | Using private rather than public for constants, saves gas |  8 |
| 8      | Functions guaranteed to revert when called by normal users can be marked payable |  4 |

## Findings

### 1- State variables only set in the constructor should be declared `immutable` :

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

There are 4 instances of this issue:

File: src/utils/token/PagesERC721.sol [Line 24](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L24)
```
string public name;
```

File: src/utils/token/PagesERC721.sol [Line 26](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L26)
```
string public symbol;
```

File: src/utils/token/GobblersERC721.sol [Line 23](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L23)
```
string public name;
```

File: src/utils/token/GobblersERC721.sol [Line 25](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L25)
```
string public symbol;
```

### 2- Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead :

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size as you can check [here](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html).

So use `uint256`/`int256` for state variables and then downcast to lower sizes where needed.

There are 4 instances of this issue:

File: src/Pages.sol [Line 107](https://github.com/code-423n4/2022-09-vtvl/blob/main/src/VTVLVesting.sol#L27)
```
uint128 public currentId;
```

File: src/Pages.sol [Line 114](https://github.com/code-423n4/2022-09-vtvl/blob/main/src/VTVLVesting.sol#L27)
```
uint128 public numMintedForCommunity;
```

File: src/ArtGobblers.sol [Line 159](https://github.com/code-423n4/2022-09-vtvl/blob/main/src/ArtGobblers.sol#L27)
```
uint128 public numMintedFromGoo;
```

File: src/ArtGobblers.sol [Line 167](https://github.com/code-423n4/2022-09-vtvl/blob/main/src/ArtGobblers.sol#L27)
```
uint128 public currentNonLegendaryId;
```

### 3- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

There are 7 instances of this issue:

File: src/ArtGobblers.sol [Line 456](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L456)
```
getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal);
```

File: src/ArtGobblers.sol [Line 464](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L464)
```
legendaryGobblerAuctionData.numSold += 1; 
```

File: src/ArtGobblers.sol [Line 662](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L662)
```
getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
```

File: src/ArtGobblers.sol [Line 905](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L905)
```
getUserData[from].emissionMultiple -= emissionMultiple;
```

File: src/ArtGobblers.sol [Line 906](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L906)
```
getUserData[from].gobblersOwned -= 1;
```

File: src/ArtGobblers.sol [Line 912](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L912)
```
getUserData[to].emissionMultiple += emissionMultiple;
```

File: src/ArtGobblers.sol [Line 912](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L913)
```
getUserData[to].gobblersOwned += 1;
```    

### 4- Use custom errors rather than `require()`/`revert()` strings to save deployments gas :

Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information.

There are 24 instances of this issue :

```
File: src/ArtGobblers.sol

437      require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
696      if (gobblerId == 0) revert("NOT_MINTED");
705      if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");
711      revert("NOT_MINTED");
885      require(from == getGobblerData[id].owner, "WRONG_FROM");
887      require(to != address(0), "INVALID_RECIPIENT");
889      require(
            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
         );

src/Pages.sol

266      if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

File: src/utils/token/GobblersERC1155B.sol

107      require(owners.length == ids.length, "LENGTH_MISMATCH");
149      require(
                ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
                    ERC1155TokenReceiver.onERC1155Received.selector,
                "UNSAFE_RECIPIENT"
         );
185      require(
                ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
                        ERC1155TokenReceiver.onERC1155BatchReceived.selector,
                "UNSAFE_RECIPIENT"
         );

File: src/utils/token/GobblersERC721.sol

62       require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
66       require(owner != address(0), "ZERO_ADDRESS");
95       require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
121      require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
         );
137      require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
         );

File: src/utils/token/PagesERC721.sol

55       require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
59       require(owner != address(0), "ZERO_ADDRESS");
85       require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
103      require(from == _ownerOf[id], "WRONG_FROM");
105      require(to != address(0), "INVALID_RECIPIENT");
107      require(
            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
         );
135      require(
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
         );
151      require(
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
         );
```

### 5- It costs more gas to initialize variables to zero than to let the default of zero be applied (saves ~3 gas per instance) :

If a variable is not set/initialized, it is assumed to have the default value (0 for uint or int, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

There are 6 instances of this issue:

File: src/utils/token/GobblersERC1155B.sol [Line 173](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173)
```
for (uint256 i = 0; i < amount; ++i) 
```

File: src/utils/token/GobblersERC721.sol [Line 186](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L186)
```
for (uint256 i = 0; i < amount; ++i)
```

File: src/utils/GobblerReserve.sol [Line 37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
```
for (uint256 i = 0; i < ids.length; i++)
```

File: src/Pages.sol [Line 251](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)
```
for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

File: src/ArtGobblers.sol [Line 432](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432)
```
for (uint256 i = 0; i < cost; ++i)
```

File: src/ArtGobblers.sol [Line 592](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592)
```
for (uint256 i = 0; i < numGobblers; ++i)
```

### 6- Use of ++i cost less gas than i++/i=i+1 in for loops :

There are 2 instances of this issue:

File: src/utils/GobblerReserve.sol [Line 37](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37)
```
for (uint256 i = 0; i < ids.length; i++)
```

File: src/Pages.sol [Line 251](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251)
```
for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

### 7- Using private rather than public for constants, saves gas :

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

There are 8 instances of this issue:

File: src/ArtGobblers.sol [Line 112](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112)

```
uint256 public constant MAX_SUPPLY = 10000;
```

File: src/ArtGobblers.sol [Line 115](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115)

```
uint256 public constant MINTLIST_SUPPLY = 2000;
```

File: src/ArtGobblers.sol [Line 118](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118)

```
uint256 public constant LEGENDARY_SUPPLY = 10;
```

File: src/ArtGobblers.sol [Line 122](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L122)

```
uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;
```

File: src/ArtGobblers.sol [Line 126](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L126)

```
uint256 public constant MAX_MINTABLE = MAX_SUPPLY
        - MINTLIST_SUPPLY
        - LEGENDARY_SUPPLY
        - RESERVED_SUPPLY;
```

File: src/ArtGobblers.sol [Line 177](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L177)

```
uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
```

File: src/ArtGobblers.sol [Line 180](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L180)

```
uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
```

File: src/ArtGobblers.sol [Line 184](https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L184)

```
uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```

### 8- Functions guaranteed to revert when called by normal users can be marked `payable` :

If a function modifier such as `onlyAdmin` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for the owner because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are : 

CALLVALUE(gas=2), DUP1(gas=3), ISZERO(gas=3), PUSH2(gas=3), JUMPI(gas=10), PUSH1(gas=3), DUP1(gas=3), REVERT(gas=0), JUMPDEST(gas=1), POP(gas=2). 
Which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

There are 4 instances of this issue:

```
File: src/ArtGobblers.sol

560      function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner

File: src/Goo.sol

101      function mintForGobblers(address to, uint256 amount) external only(artGobblers)
108      function burnForGobblers(address from, uint256 amount) external only(artGobblers)
115      function burnForPages(address from, uint256 amount) external only(pages)
```