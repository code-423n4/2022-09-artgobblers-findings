## FINDINGS
NB: *Some functions have been truncated where neccessary to just show affected parts of the code*
The gas estimates are the exact results from running the tests included with an exception of internal functions
Throught the report some places might be denoted with audit tags to show the actual place affected.

### Cache storage values in memory to minimize SLOADs
The code can be optimized by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L493
### ArtGobblers.sol.legendaryGobblerPrice(): numMintedFromGoo should use the  cached value(Saves 109 gas )
```
Average Gas before: 1646
Average Gas After:  1537
```

```solidity
File: /src/ArtGobblers.sol
482:        uint256 mintedFromGoo = numMintedFromGoo;

492:            // Compute how many gobblers were minted since the auction began.
493:            uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart; //@audit: numMintedFromGoo should use the cached value 
```

[Line 493](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L493) should use the cached value from [Line 482](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L482)

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..27f45e6 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -490,7 +490,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             if (numMintedAtStart > mintedFromGoo) revert LegendaryAuctionNotStarted(numMintedAtStart - mintedFromGoo);

             // Compute how many gobblers were minted since the auction began.
-            uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
+            uint256 numMintedSinceStart = mintedFromGoo- numMintedAtStart;

             // prettier-ignore
             // If we've minted the full interval or beyond it, the price has decayed to 0.
```
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L411-L471
### ArtGobblers.sol.mintLegendaryGobbler(): legendaryGobblerAuctionData.numSold should be cached(saves 8 gas)
```
Average gas before: 72774
Average gas after:  72656
```

```solidity
File: /src/ArtGobblers.sol
411:   function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) {
412:        gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold; // Assign id.

461:            legendaryGobblerAuctionData.startPrice = uint120(
462:                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
463:            );
464:            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.

470:        }
471:    }
```

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..4b6b040 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -409,7 +409,8 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
     /// @param gobblerIds The ids of the standard gobblers to burn.
     /// @return gobblerId The id of the legendary gobbler that was minted.
     function mintLegendaryGobbler(uint256[] calldata gobblerIds) external returns (uint256 gobblerId) {
-        gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold; // Assign id.
+        uint128 temp = legendaryGobblerAuctionData.numSold;
+        gobblerId = FIRST_LEGENDARY_GOBBLER_ID + temp; // Assign id.

         // If the gobbler id would be greater than the max supply, there are no remaining legendaries.
         if (gobblerId > MAX_SUPPLY) revert NoRemainingLegendaryGobblers();
@@ -461,7 +462,7 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
             legendaryGobblerAuctionData.startPrice = uint120(
                 cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
             );
-            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
+            legendaryGobblerAuctionData.numSold = temp + 1; // Increment the # of legendaries sold.

             // If gobblerIds has 1,000 elements this should cost around ~270,000 gas.
             emit LegendaryGobblerMinted(msg.sender, gobblerId, gobblerIds[:cost]);
```

### Reorder the checks to save on gas if we fail early
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L880-L917
```solidity
File: /src/ArtGobblers.sol
880:    function transferFrom(
881:        address from,
882:        address to,
883:        uint256 id
884:    ) public override {
885:        require(from == getGobblerData[id].owner, "WRONG_FROM");

887:        require(to != address(0), "INVALID_RECIPIENT");

889:        require(
890:            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
891:            "NOT_AUTHORIZED"
892:        );

917:    }
```
in the above function, the check `require(to != address(0), "INVALID_RECIPIENT");` consumes less gas compared to `require(from == getGobblerData[id].owner, "WRONG_FROM");`. As such with the current way the code is setup, if the `to address` happens to be address `0` , the transaction would revert as the check on Line 887 would fail. We would still have incurred the cost for the evaluation of `require(from == getGobblerData[id].owner, "WRONG_FROM");`  which is more expensive. Now we could save this gas by ordering the less costly check on top  as shown below.

```diff
diff --git a/src/ArtGobblers.sol b/src/ArtGobblers.sol
index 0d413c0..f12358d 100644
--- a/src/ArtGobblers.sol
+++ b/src/ArtGobblers.sol
@@ -882,9 +882,10 @@ contract ArtGobblers is GobblersERC721, LogisticVRGDA, Owned, ERC1155TokenReceiv
         address to,
         uint256 id
     ) public override {
+        require(to != address(0), "INVALID_RECIPIENT");
         require(from == getGobblerData[id].owner, "WRONG_FROM");

-        require(to != address(0), "INVALID_RECIPIENT");


         require(
             msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
```

**Other instances to modify**

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L98-L125
```solidity
File: /src/utils/token/PagesERC721.sol
98:    function transferFrom(

102:    ) public {
103:        require(from == _ownerOf[id], "WRONG_FROM");

105:        require(to != address(0), "INVALID_RECIPIENT");

125:    }
```


https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol#L82-L109
```solidity
File: /src/tokens/ERC721.sol
82:    function transferFrom(
83:        address from,
84:        address to,
85:        uint256 id
86:    ) public virtual {
87:        require(from == _ownerOf[id], "WRONG_FROM");

89:        require(to != address(0), "INVALID_RECIPIENT");


109:    }
```

### Use Shift Right/Left instead of Division/Multiplication
A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

[relevant source](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g008---use-shift-rightleft-instead-of-divisionmultiplication-if-possible)

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L462
```solidity
File: /src/ArtGobblers.sol
462:                 cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

```diff
-                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
+                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE >> 1 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost << 1
```


### x += y costs more gas than x = x + y for state variables

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L456
```solidity
File: /src/ArtGobblers.sol
456:            getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.


464:            legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L844
```solidity
File: /src/ArtGobblers.sol
844:            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
```

### No need to initialize variables with their default values
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). If you explicitly initialize it with its default value, you are just wasting gas.
It costs more gas to initialize variables to zero than to let the default of zero be applied
Declaring uint256 i = 0; means doing an MSTORE of the value 0 Instead you could just declare uint256 i to declare the variable without assigning it’s default value, saving 3 gas per declaration

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251
```solidity
File: /src/Pages.sol
//@audit: uint256 i = 0;
251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37
```solidity
File: /src/utils/GobblerReserve.sol

//@audit: uint256 i = 0
37:            for (uint256 i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186
```solidity
File: /src/utils/token/GobblersERC721.sol

//@audit: uint256 i = 0
186:            for (uint256 i = 0; i < amount; ++i) {
```


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114
```solidity
File: /src/utils/token/GobblersERC1155B.sol
114:            for (uint256 i = 0; i < owners.length; ++i) {
```

### Cache the length of arrays in loops (saves ~6 gas per iteration)
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.

The solidity compiler will always read the length of the array during each iteration. That is,

 1.if it is a storage array, this is an extra sload operation (100 additional extra gas (EIP-2929 2) for each iteration except for the first),
 2.if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first),
 3.if it is a calldata array, this is an extra calldataload operation (3 additional gas for each iteration except for the first)

This extra costs can be avoided by caching the array length (in stack):
When reading the length of an array,  **sload** or **mload** or **calldataload** operation is only called once and subsequently replaced by a cheap **dupN** instruction. Even though mload , calldataload and dupN have the same gas cost, mload and calldataload needs an additional dupN to put the offset in the stack, i.e., an extra 3 gas. which brings this to 6 gas
 
Here, I suggest storing the array’s length in a variable before the for-loop, and use it instead:

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37
```solidity
File: /src/utils/GobblerReserve.sol
37:            for (uint256 i = 0; i < ids.length; i++) {
```


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114
```solidity
File: /src/utils/token/GobblersERC1155B.sol
114:            for (uint256 i = 0; i < owners.length; ++i) {
```

### ++i costs less gas compared to i++ or i += 1  (~5 gas per iteration)

++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

i++ increments i and returns the initial value of i. Which means:

```solidity
uint i = 1;  
i++; // == 1 but i == 2  
```

But ++i returns the actual incremented value:

```solidity
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

Instances include:

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/Pages.sol#L251
```solidity
FIle: /src/Pages.sol
251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

We can modify the above as follows, just like in other parts of the codebase
```solidity
            for (uint256 i = 0; i < numPages; ++i) _mint(community, ++lastMintedPageId);
```

**Other instances**
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/GobblerReserve.sol#L37
```solidity
File: /src/utils/GobblerReserve.sol
37:            for (uint256 i = 0; i < ids.length; i++) {
```


### Using bools for storage incurs overhead
```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```

See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) 
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

**Instances affected include**
https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L149
```solidity
File: /src/ArtGobblers.sol
149:    mapping(address => bool) public hasClaimedMintlistGobbler;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L77
```solidity
File: /src/utils/token/GobblersERC721.sol
77:    mapping(address => mapping(address => bool)) public isApprovedForAll;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/PagesERC721.sol#L70
```solidity
File: /src/utils/token/PagesERC721.sol
70:    mapping(address => mapping(address => bool)) internal _isApprovedForAll;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L37
```solidity
File: /src/utils/token/GobblersERC1155B.sol
37:    mapping(address => mapping(address => bool)) public isApprovedForAll;
```

### Using private rather than public for constants, saves gas
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L112-L129
```solidity
File: /src/ArtGobblers.sol
112:    uint256 public constant MAX_SUPPLY = 10000;

115:    uint256 public constant MINTLIST_SUPPLY = 2000;

118:    uint256 public constant LEGENDARY_SUPPLY = 10;

122:    uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;

126:    uint256 public constant MAX_MINTABLE = MAX_SUPPLY
127:        - MINTLIST_SUPPLY
128:        - LEGENDARY_SUPPLY
129:        - RESERVED_SUPPLY;


177:    uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;


180:    uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;


184:    uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```
### Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

Below I show sample places where custom errors could be applied

### Use Custom Errors instead of Revert Strings to save Gas(~50 gas)
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)
Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).
see [Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L437
```solidity
File: /src/ArtGobblers.sol
437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

```

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/ArtGobblers.sol#L885-L892
```solidity
FIle: /src/ArtGobblers.sol
885:        require(from == getGobblerData[id].owner, "WRONG_FROM");

887:        require(to != address(0), "INVALID_RECIPIENT");

889:        require(
890:            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
891:            "NOT_AUTHORIZED"
892:        );
```

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/PagesERC721.sol#L55
```solidity
File: /src/utils/token/PagesERC721.sol
55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

59:        require(owner != address(0), "ZERO_ADDRESS");

85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");

103:        require(from == _ownerOf[id], "WRONG_FROM");

105:        require(to != address(0), "INVALID_RECIPIENT");

107:        require(
108:            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
109:            "NOT_AUTHORIZED"
110:        );


135:            require(
136:                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
137:                    ERC721TokenReceiver.onERC721Received.selector,
138:                "UNSAFE_RECIPIENT"
139:            );


151:            require(
152:                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
153:                    ERC721TokenReceiver.onERC721Received.selector,
154:                "UNSAFE_RECIPIENT"
155:            );
```

https://github.com/code-423n4/2022-09-artgobblers/blob/2ae644ace932271fb34399c1c0771aabae2e423c/src/utils/token/GobblersERC721.sol#L62
```solidity
File: /src/utils/token/GobblersERC721.sol
62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

66:        require(owner != address(0), "ZERO_ADDRESS");

68:        return getUserData[owner].gobblersOwned;

95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

121:        require(
122:            to.code.length == 0 ||
123:                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
124:                ERC721TokenReceiver.onERC721Received.selector,
125:            "UNSAFE_RECIPIENT"
126:        );


137:        require(
138:            to.code.length == 0 ||
139:                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
140:                ERC721TokenReceiver.onERC721Received.selector,
141:            "UNSAFE_RECIPIENT"
142:        );
```