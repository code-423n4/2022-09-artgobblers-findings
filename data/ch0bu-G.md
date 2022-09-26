## 1. `++i` costs less gas compared to `i++` or `i += 1`, same for `--i/i--`. Especially in for loops

`++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

`i++` increments i and returns the initial value of `i`.

```
uint i = 1;  
i++; // == 1 but i == 2
```
But ++i returns the actual incremented value:
```
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```
In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

I suggest using ++i instead of i++ to increment the value of an uint variable.

If done inside for loop, saves 6 gas per loop.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol
```
37	for (uint256 i = 0; i < ids.length; i++) {
```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol
```
251	for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```

## 2. Use a more recent version of solidity

- Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath  
- Use a solidity version of at least 0.8.2 to get compiler automatic inlining  
- Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads  
- Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings  
- Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

```
All contracts in scope
```

## 3. No need to explicitly initialize variables with default values

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol
```
37	for (uint256 i = 0; i < ids.length; i++) {
```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol
```
186	for (uint256 i = 0; i < amount; ++i) {
```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol
```
251	for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol
```
432	for (uint256 i = 0; i < cost; ++i) {
592	for (uint256 i = 0; i < numGobblers; ++i) {
```

## 4. Use custom errors instead of revert strings to save Gas


Custom errors from Solidity 0.8.4 are cheaper than revert string (cheaper deployment cost and runtime cost when the revert condition is met)

Source: https://blog.soliditylang.org/2021/04/21/custom-errors/:

> Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

https://github.com/transmissions11/VRGDAs/blob/f4dec0611641e344339b2c78e5f733bba3b532e0/src/VRGDA.sol
```
32	require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
```
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/auth/Owned.sol
```
20	require(msg.sender == owner, "UNAUTHORIZED");
```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol
```
62	require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
66	require(owner != address(0), "ZERO_ADDRESS");
95	require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
...
121	require(
122         to.code.length == 0 ||
123             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
124             ERC721TokenReceiver.onERC721Received.selector,
125         "UNSAFE_RECIPIENT"
126     );
...
137	require(
138         to.code.length == 0 ||
139             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
140             ERC721TokenReceiver.onERC721Received.selector,
141         "UNSAFE_RECIPIENT"
142     );
```
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol
```
55	require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
59	require(owner != address(0), "ZERO_ADDRESS");
85	require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
103	require(from == _ownerOf[id], "WRONG_FROM");
105	require(to != address(0), "INVALID_RECIPIENT");
...
107	require(
108         msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
109         "NOT_AUTHORIZED"
110     );
...
135	require(
136             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
137                 ERC721TokenReceiver.onERC721Received.selector,
138             "UNSAFE_RECIPIENT"
139         );
...
151	require(
152             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
153                 ERC721TokenReceiver.onERC721Received.selector,
154             "UNSAFE_RECIPIENT"
155         );
```
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol
```
142	require(x > 0, "UNDEFINED");
```
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/tokens/ERC721.sol
```
36	require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
40	require(owner != address(0), "ZERO_ADDRESS");
69	require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
87	require(from == _ownerOf[id], "WRONG_FROM");
89	require(to != address(0), "INVALID_RECIPIENT");
...
91	require(
92          msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
93          "NOT_AUTHORIZED"
94      );
...
118	require(
119         to.code.length == 0 ||
120             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
121             ERC721TokenReceiver.onERC721Received.selector,
122         "UNSAFE_RECIPIENT"
123     );
...
134	require(
135         to.code.length == 0 ||
136             ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
137             ERC721TokenReceiver.onERC721Received.selector,
138         "UNSAFE_RECIPIENT"
139     );
...
158	require(to != address(0), "INVALID_RECIPIENT");
160	require(_ownerOf[id] == address(0), "ALREADY_MINTED");
175	require(owner != address(0), "NOT_MINTED");
...
196	require(
197         to.code.length == 0 ||
198             ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, "") ==
199             ERC721TokenReceiver.onERC721Received.selector,
200         "UNSAFE_RECIPIENT"
201     );	
...
211	require(
212         to.code.length == 0 ||
213             ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, data) ==
214             ERC721TokenReceiver.onERC721Received.selector,
215         "UNSAFE_RECIPIENT"
216     );		
```

## 5. Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.


## 6. Multiplication/division by two should use bit shifting


\<x> * 2 is equivalent to \<x> << 1 and \<x> / 2 is the same as \<x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol
```
449	getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
462	cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```
