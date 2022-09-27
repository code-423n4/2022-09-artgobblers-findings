# Gas Optimizations

|No.|Issue|Instances|
|:-|:-|:-:|
|1|For-loops: Index initialized with default value|7|
|2|For-Loops: Cache array length outside of loops|2|
|3|Arithmetics: `++i` costs less gas compared to `i++` or `i += 1`|5|
|4|Arithmetics: Use `!= 0` instead of `> 0` for unsigned integers in `require` statements|1|
|5|Arithmetics: Use Shift Right/Left instead of Division/Multiplication if possible|2|
|6|Visibility: Consider declaring constants as non-public to save gas|4|
|7|Duplicated `require()` checks should be refactored to a modifier or function|1|
|8|Errors: Use custom errors instead of revert strings|36|
|9|State variables should be cached in stack variables rather than re-reading them from storage|1|
|10|`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables|2|
|11|Using `bool` for storage incurs overhead|5|
|12|Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead|17|
|13|Not using the named return variables when a function returns, wastes deployment gas|4|
|14|Multiple accesses of a mapping/array should use a local variable cache|4|

Total: **97** instances over **14** issues

## For-loops: Index initialized with default value
Uninitialized `uint` variables are assigned with a default value of `0`. 

Thus, in for-loops, explicitly initializing an index with `0` costs unnecesary gas. For example, the following code:
```js
for (uint256 i = 0; i < length; ++i) {
```
can be written without explicitly setting the index to `0`:  
```js
for (uint256 i; i < length; ++i) {
```

_There are **7** instances of this issue:_  
```solidity
src/Pages.sol:
 251:        for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/ArtGobblers.sol:
 432:        for (uint256 i = 0; i < cost; ++i) {
 592:        for (uint256 i = 0; i < numGobblers; ++i) {

src/utils/GobblerReserve.sol:
  37:        for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC721.sol:
 186:        for (uint256 i = 0; i < amount; ++i) {

src/utils/token/GobblersERC1155B.sol:
 114:        for (uint256 i = 0; i < owners.length; ++i) {
 173:        for (uint256 i = 0; i < amount; ++i) {
```

## For-Loops: Cache array length outside of loops
Reading an array's length at each iteration has the following gas overheads:
* storage arrays incur a Gwarmaccess (**100 gas**)
* memory arrays use `mload` (**3 gas**)
* calldata arrays use `calldataload` (**3 gas**)

Caching the length changes each of these to a `DUP<N>` (**3 gas**), and gets rid of the extra `DUP<N>` needed to store the stack offset. This would save around **3 gas** per iteration.

For example:
```js
for (uint256 i; i < arr.length; ++i) {}
```
can be changed to:
```js
uint256 len = arr.length;
for (uint256 i; i < len; ++i) {}
```

_There are **2** instances of this issue:_  
```solidity
src/utils/GobblerReserve.sol:
  37:        for (uint256 i = 0; i < ids.length; i++) {

src/utils/token/GobblersERC1155B.sol:
 114:        for (uint256 i = 0; i < owners.length; ++i) {
```

## Arithmetics: `++i` costs less gas compared to `i++` or `i += 1`
`++i` costs less gas compared to `i++` or `i += 1` for unsigned integers, as pre-increment is cheaper (about **5 gas** per iteration). This statement is true even with the optimizer enabled.

`i++` increments `i` and returns the initial value of `i`. Which means:
```js
uint i = 1;  
i++; // == 1 but i == 2  
```
But `++i` returns the actual incremented value:
```js
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```

In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`, thus it costs more gas.

The same logic applies for `--i` and `i--`.

_There are **5** instances of this issue:_  
```solidity
src/Pages.sol:
 251:        for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

src/ArtGobblers.sol:
 464:        legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.
 906:        getUserData[from].gobblersOwned -= 1;
 913:        getUserData[to].gobblersOwned += 1;

src/utils/GobblerReserve.sol:
  37:        for (uint256 i = 0; i < ids.length; i++) {
```

## Arithmetics: Use `!= 0` instead of `> 0` for unsigned integers in `require` statements
A variable of type `uint` will never go below 0. Thus, checking `!= 0` rather than `> 0` is sufficient, which would save **[6 gas](https://aws1.discourse-cdn.com/business6/uploads/zeppelin/original/2X/3/363a367d6d68851f27d2679d10706cd16d788b96.png)** per instance.

_There is **1** instance of this issue:_  
```solidity
lib/solmate/src/utils/SignedWadMath.sol:
 142:        require(x > 0, "UNDEFINED");
```

## Arithmetics: Use Shift Right/Left instead of Division/Multiplication if possible
A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left.

While the `MUL` and `DIV` opcodes use 5 gas, the `SHL` and `SHR` opcodes only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

For example, the following code:
```js
uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
```
can be changed to:
```js
uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;
```

_There are **2** instances of this issue:_  
```solidity
src/ArtGobblers.sol:
 449:        getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
 462:        cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

## Visibility: Consider declaring constants as non-public to save gas
If a constant is not used outside of its contract, declaring its visibility as `private` or `internal` instead of `public` can save gas. If needed, the value can be read from the verified contract source code.

Gas savings occur as the compiler does not have to create non-payable getter functions for deployment calldata and add another entry to the method ID table.

_There are **4** instances of this issue:_  
```solidity
src/ArtGobblers.sol:
 112:        uint256 public constant MAX_SUPPLY = 10000;
 115:        uint256 public constant MINTLIST_SUPPLY = 2000;
 118:        uint256 public constant LEGENDARY_SUPPLY = 10;
 177:        uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;
```

## Duplicated `require()` checks should be refactored to a modifier or function
If a `require()` statement that is used to validate function parameters or global variables is present across multiple functions in a contract, it should be rewritten into modifier if possible. This would help to reduce code deployment size, which saves gas, and improve code readability.

_There is **1** instance of this issue:_  
Instance #3:
```solidity
lib/solmate/src/tokens/ERC721.sol:
  89:        require(to != address(0), "INVALID_RECIPIENT");
 158:        require(to != address(0), "INVALID_RECIPIENT");
```

## Errors: Use custom errors instead of revert strings
Since Solidity v0.8.4, custom errors can be used rather than `require` statements.

Taken from [Custom Errors in Solidity](https://blog.soliditylang.org/2021/04/21/custom-errors/):
> Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors reduce runtime gas costs as they save about **50 gas** each time they are hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Furthermore, not definiting error strings also help to reduce deployment gas costs. 

_There are **36** instances of this issue:_  
```solidity
src/ArtGobblers.sol:
 437:        require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
 885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
 887:        require(to != address(0), "INVALID_RECIPIENT");

 889:        require(
 890:            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
 891:            "NOT_AUTHORIZED"
 892:        );

src/utils/token/PagesERC721.sol:
  55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
  59:        require(owner != address(0), "ZERO_ADDRESS");
  85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
 103:        require(from == _ownerOf[id], "WRONG_FROM");
 105:        require(to != address(0), "INVALID_RECIPIENT");

 107:        require(
 108:            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
 109:            "NOT_AUTHORIZED"
 110:        );


 135:        require(
 136:            ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
 137:                ERC721TokenReceiver.onERC721Received.selector,
 138:            "UNSAFE_RECIPIENT"
 139:        );


 151:        require(
 152:            ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
 153:                ERC721TokenReceiver.onERC721Received.selector,
 154:            "UNSAFE_RECIPIENT"
 155:        );

src/utils/token/GobblersERC721.sol:
  62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
  66:        require(owner != address(0), "ZERO_ADDRESS");
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

src/utils/token/GobblersERC1155B.sol:
 107:        require(owners.length == ids.length, "LENGTH_MISMATCH");

 149:        require(
 150:            ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
 151:                ERC1155TokenReceiver.onERC1155Received.selector,
 152:            "UNSAFE_RECIPIENT"
 153:        );


 185:        require(
 186:            ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
 187:                ERC1155TokenReceiver.onERC1155BatchReceived.selector,
 188:            "UNSAFE_RECIPIENT"
 189:        );

lib/solmate/src/auth/Owned.sol:
  20:        require(msg.sender == owner, "UNAUTHORIZED");

lib/solmate/src/tokens/ERC721.sol:
  36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
  40:        require(owner != address(0), "ZERO_ADDRESS");
  69:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
  87:        require(from == _ownerOf[id], "WRONG_FROM");
  89:        require(to != address(0), "INVALID_RECIPIENT");

  91:        require(
  92:            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
  93:            "NOT_AUTHORIZED"
  94:        );


 118:        require(
 119:            to.code.length == 0 ||
 120:                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
 121:                ERC721TokenReceiver.onERC721Received.selector,
 122:            "UNSAFE_RECIPIENT"
 123:        );


 134:        require(
 135:            to.code.length == 0 ||
 136:                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
 137:                ERC721TokenReceiver.onERC721Received.selector,
 138:            "UNSAFE_RECIPIENT"
 139:        );

 158:        require(to != address(0), "INVALID_RECIPIENT");
 160:        require(_ownerOf[id] == address(0), "ALREADY_MINTED");
 175:        require(owner != address(0), "NOT_MINTED");

 196:        require(
 197:            to.code.length == 0 ||
 198:                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, "") ==
 199:                ERC721TokenReceiver.onERC721Received.selector,
 200:            "UNSAFE_RECIPIENT"
 201:        );


 211:        require(
 212:            to.code.length == 0 ||
 213:                ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, data) ==
 214:                ERC721TokenReceiver.onERC721Received.selector,
 215:            "UNSAFE_RECIPIENT"
 216:        );

lib/solmate/src/utils/SignedWadMath.sol:
 142:        require(x > 0, "UNDEFINED");

lib/VRGDAs/src/VRGDA.sol:
  32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
```

## State variables should be cached in stack variables rather than re-reading them from storage
If a state variable is read from storage multiple times in a function, it should be cached in a stack variable.

Caching of a state variable replaces each Gwarmaccess (**100 gas**) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There is **1** instance of this issue:_  
`numMintedFromGoo` in `legendaryGobblerPrice()`:
```solidity
src/ArtGobblers.sol:
 482:        uint256 mintedFromGoo = numMintedFromGoo;
 493:        uint256 numMintedSinceStart = numMintedFromGoo - numMintedAtStart;
```

## `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
The above also applies to state variables that use `-=`.

_There are **2** instances of this issue:_  
```solidity
src/Pages.sol:
 244:        uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);

src/ArtGobblers.sol:
 844:        uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);
```

## Using `bool` for storage incurs overhead
Declaring storage variables as `bool` is more expensive compared to `uint256`, as explained [here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol#L23-L27):

> Booleans are more expensive than `uint256` or any type that takes up a full word because each write operation emits an extra `SLOAD` to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

Use `uint256(1)` and `uint256(2)` rather than true/false to avoid a Gwarmaccess ([**100 gas**](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)) for the extra `SLOAD`, and to avoid Gsset (**20000 gas**) when changing from 'false' to 'true', after having been 'true' in the past.

_There are **5** instances of this issue:_  
```solidity
src/ArtGobblers.sol:
 149:        mapping(address => bool) public hasClaimedMintlistGobbler;

src/utils/token/PagesERC721.sol:
  70:        mapping(address => mapping(address => bool)) internal _isApprovedForAll;

src/utils/token/GobblersERC721.sol:
  77:        mapping(address => mapping(address => bool)) public isApprovedForAll;

src/utils/token/GobblersERC1155B.sol:
  37:        mapping(address => mapping(address => bool)) public isApprovedForAll;

lib/solmate/src/tokens/ERC721.sol:
  51:        mapping(address => mapping(address => bool)) public isApprovedForAll;
```

## Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead
As seen from [here](https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html):
> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

However, this does not apply to storage values as using reduced-size types might be beneficial to pack multiple elements into a single storage slot. Thus, where appropriate, use `uint256`/`int256` and downcast when needed.

_There are **17** instances of this issue:_  
```solidity
src/ArtGobblers.sol:
 189:        uint128 startPrice;
 191:        uint128 numSold;
 204:        uint64 randomSeed;
 206:        uint64 nextRevealTimestamp;
 208:        uint56 lastRevealedId;
 210:        uint56 toBeRevealed;
 615:        uint64 swapIndex = getGobblerData[swapId].idx == 0
 623:        uint64 currentIndex = getGobblerData[currentId].idx == 0
 899:        uint32 emissionMultiple = getGobblerData[id].emissionMultiple; // Caching saves gas.

src/utils/token/GobblersERC721.sol:
  38:        uint64 idx;
  40:        uint32 emissionMultiple;
  49:        uint32 gobblersOwned;
  51:        uint32 emissionMultiple;
  53:        uint128 lastBalance;
  55:        uint64 lastTimestamp;

src/utils/token/GobblersERC1155B.sol:
  48:        uint64 idx;
  50:        uint32 emissionMultiple;
```

## Not using the named return variables when a function returns, wastes deployment gas  

_There are **4** instances of this issue:_  
```solidity
src/utils/token/PagesERC721.sol:
  73:        if (operator == address(artGobblers)) return true; // Skip approvals for the ArtGobblers contract.
  75:        return _isApprovedForAll[owner][operator];

src/utils/token/GobblersERC1155B.sol:
  56:        return getGobblerData[id].owner;

src/utils/rand/ChainlinkV1RandProvider.sol:
  69:        return requestRandomness(chainlinkKeyHash, chainlinkFee);
```

## Multiple accesses of a mapping/array should use a local variable cache
If a mapping/array is read with the same key/index multiple times in a function, it should be cached in a stack variable.

 Caching a mapping's value in a local `storage` variable when the value is accessed [multiple times](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0), saves **~42 gas per access** due to not having to recalculate the key's keccak256 hash (Gkeccak256 - **30 gas**) and that calculation's associated stack operations. Caching an array's struct avoids recalculating the array offsets into memory.

_There are **4** instances of this issue:_  
`getGobblerData[id]` in `mintLegendaryGobbler()`:
```solidity
src/ArtGobblers.sol:
 437:        require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
 439:        burnedMultipleTotal += getGobblerData[id].emissionMultiple;
```
`getGobblerData[currentId]` in `revealGobblers()`:
```solidity
src/ArtGobblers.sol:
 620:        address currentIdOwner = getGobblerData[currentId].owner;
 623:        uint64 currentIndex = getGobblerData[currentId].idx == 0
```
`getUserData[user]` in `gooBalance()`:
```solidity
src/ArtGobblers.sol:
 761:        getUserData[user].emissionMultiple,
 762:        getUserData[user].lastBalance,
 763:        uint(toDaysWadUnsafe(block.timestamp - getUserData[user].lastTimestamp))
```
`getGobblerData[id]` in `transferFrom()`:
```solidity
src/ArtGobblers.sol:
 885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
 899:        uint32 emissionMultiple = getGobblerData[id].emissionMultiple; // Caching saves gas.
```
