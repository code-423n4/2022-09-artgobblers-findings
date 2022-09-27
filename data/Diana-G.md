## G-01 Use an unchecked block for calculations that cannot overflow

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block:
https://docs.soliditylang.org/en/v0.8.7/control-structures.html#checked-or-unchecked-arithmetic

There are 10 instances of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/script/deploy/DeployBase.s.sol

```
File: script/deploy/DeployBase.s.sol

66: address gobblerAddress = LibRLP.computeAddress(tx.origin, vm.getNonce(tx.origin) + 4);
67: address pageAddress = LibRLP.computeAddress(tx.origin, vm.getNonce(tx.origin) + 5);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
File: src/ArtGobblers.sol

180: uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;
184: uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
327: gobblerRevealsData.nextRevealTimestamp = uint64(_mintStart + 1 days);
412: gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold;
708: if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold)
821: ? gooBalance(user) + gooAmount
822: : gooBalance(user) - gooAmount;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
File: src/Pages.sol

222: uint256 timeSinceStart = block.timestamp - mintStart;
```

------------

## G-02 It costs more gas to initialize variables to zero than to let the default of zero be applied

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

There are 5 instances of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
File: src/ArtGobblers.sol

432: for (uint256 i = 0; i < cost; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
File: src/Pages.sol

251: for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

```
File: src/utils/token/GobblersERC721.sol

186: for (uint256 i = 0; i < amount; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

```
File: src/utils/token/GobblersERC1155B.sol

114: for (uint256 i = 0; i < owners.length; ++i) {
173: for (uint256 i = 0; i < amount; ++i) {
```

--------------------

## G-03 x += y costs more gas than x = x+y for state variables (same for x -= y)

There are 14 instances of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
File: src/ArtGobblers.sol

439: burnedMultipleTotal += getGobblerData[id].emissionMultiple;
456: getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal);
464: legendaryGobblerAuctionData.numSold += 1;
662: getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);
844: uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1); 
905: getUserData[from].emissionMultiple -= emissionMultiple;
906: getUserData[from].gobblersOwned -= 1;
912: getUserData[to].emissionMultiple += emissionMultiple;
913: getUserData[to].gobblersOwned += 1;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
File: src/Pages.sol

244: uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

```
File: src/utils/token/GobblersERC721.sol

184: getUserData[to].gobblersOwned += uint32(amount);
```

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol

```
File: lib/solmate/src/utils/SignedWadMath.sol

201: r *= 1677202110996718588342820967067443963516166;
203: r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
205: r += 600920179829731861736702779321621459595472258049074101567377883020018308;
```

-----------

## G-04 Use custom errors rather than revert() or require() strings to save deployment gas

Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

Source: [https://blog.soliditylang.org/2021/04/21/custom-errors/](https://blog.soliditylang.org/2021/04/21/custom-errors/):

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the `error` statement, which can be used inside and outside of contracts (including interfaces and libraries).

There are 21 instances of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
File: src/ArtGobblers.sol

885:     require(from == getGobblerData[id].owner, "WRONG_FROM");
887:     require(to != address(0), "INVALID_RECIPIENT");
889-891:    require(
            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
            "NOT_AUTHORIZED"
        );
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
File: src/Pages.sol

266: if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

```
File: src/utils/token/GobblersERC721.sol

62:      require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
66:      require(owner != address(0), "ZERO_ADDRESS");
95:      require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
121-125: require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
137-141: require(
            to.code.length == 0 ||
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                ERC721TokenReceiver.onERC721Received.selector,
            "UNSAFE_RECIPIENT"
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

```
File: src/utils/token/GobblersERC1155B.sol

107:     require(owners.length == ids.length, "LENGTH_MISMATCH");
149-152: require(
                ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
                    ERC1155TokenReceiver.onERC1155Received.selector,
                "UNSAFE_RECIPIENT"
185-188: require(
                ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
                    ERC1155TokenReceiver.onERC1155BatchReceived.selector,
                "UNSAFE_RECIPIENT"
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol

```
File: src/utils/token/PagesERC721.sol

55:      require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
59:      require(owner != address(0), "ZERO_ADDRESS");
85:      require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
103:     require(from == _ownerOf[id], "WRONG_FROM");
105:     require(to != address(0), "INVALID_RECIPIENT");
107-109: require(
            msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
            "NOT_AUTHORIZED" 
135-138: require(
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
151-154: require(
                ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                    ERC721TokenReceiver.onERC721Received.selector,
                "UNSAFE_RECIPIENT"
```

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol

```
File: lib/solmate/src/utils/SignedWadMath.sol

142: require(x > 0, "UNDEFINED");
```

-------------------------

## G-05 Usage of uints or ints smaller than 32 bytes (26 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

There are 51 instances of this issue

-------------

## G-06 ++i costs less gas than i++ especially when it's used in for loops (Same applies for --i , i-- too)

Prefix increments are cheaper than postfix increments.  

Further more, using unchecked {++i} is even more gas efficient, and the gas saving accumulates every iteration and can make a real change  
There is no risk of overflow caused by increamenting the iteration index in for loops (the `++i` in `for (uint256 i; i < numPages; ++i) {`).  
But increments perform overflow checks that are not necessary in this case.  
These functions use not using prefix increments (`++i`) or not using the unchecked keyword

There are 4 instances of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol

```
File: src/Pages.sol

251: for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol

```
File: src/utils/token/PagesERC721.sol

115: _balanceOf[from]--;
117: _balanceOf[to]++;
181: _balanceOf[to]++;
```

-----------------

## G-07 An array's length should be cached to save gas

There are 8 instance of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol

```
File: src/utils/token/GobblersERC721.sol

122: to.code.length == 0 ||
138: to.code.length == 0 ||
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol

```
File: src/utils/token/GobblersERC1155B.sol

107: require(owners.length == ids.length, "LENGTH_MISMATCH");
114: for (uint256 i = 0; i < owners.length; ++i) {
148: if (to.code.length != 0)
184: if (to.code.length != 0) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol

```
File: src/utils/token/PagesERC721.sol

134: if (to.code.length != 0)
150: if (to.code.length != 0)
```

----------

## G-08 Multiplication or division by 2 should use bit shifting

`<x> * 2` is equivalent to `<x> << 1` and `<x> / 2` is the same as `<x> >> 1`. The `MUL` and `DIV` opcodes cost 5 gas, whereas `SHL` and `SHR` only cost 3 gas

There are 2 instances of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
File: src/ArtGobblers.sol

449: getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
462: cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

---------

## G-09 Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

There are 4 instances of this issue

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

```
File: src/ArtGobblers.sol

560: function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol

```
File: src/Goo.sol

101: function mintForGobblers(address to, uint256 amount) external only(artGobblers) {
108: function burnForGobblers(address from, uint256 amount) external only(artGobblers) {
115: function burnForPages(address from, uint256 amount) external only(pages) {
```

----------

## G-10 Using greater than 0 costs more gas than != 0 when used on a uint in a require() statement

!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas). This change saves 6 gas per instance

https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol

```
File: lib/solmate/src/utils/SignedWadMath.sol

142: require(x > 0, "UNDEFINED");
```

----------

