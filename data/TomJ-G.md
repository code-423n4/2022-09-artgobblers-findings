## Table of Contents
Total of 6 issues found.
- Defined Variables Used Only Once
- Duplicate require() Checks Should be a Modifier or a Function
- Use Bit Shifting Instead of Multiplication/Division of 2
- Using Elements Smaller than 32 bytes (256 bits) Might Use More Gas
- ++i Costs Less Gas than i++
- Use Custom Errors to Save Gas

&ensp;
### Defined Variables Used Only Once

#### Issue
Certain variables is defined even though they are used only once.
Remove these unnecessary variables to save gas.
For cases where it will reduce the readability, one can use comments to help describe what the code is doing.

#### PoC
Total of 4 instances found.

1. Remove 'startPrice' variable of `legendaryGobblerPrice()` of `ArtGobblers.sol`
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L480
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L499
Mitigation:
Delete line 480 and replace line 499 with code below
```
else return FixedPointMathLib.unsafeDivUp(legendaryGobblerAuctionData.startPrice * (LEGENDARY_AUCTION_INTERVAL - numMintedSinceStart), LEGENDARY_AUCTION_INTERVAL);
```

2. Remove 'numSold' variable of `legendaryGobblerPrice()` of `ArtGobblers.sol`
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L481
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L487
Mitigation:
Delete line 481 and replace line 487 with code below
```
uint256 numMintedAtStart = (legendaryGobblerAuctionData.numSold + 1) * LEGENDARY_AUCTION_INTERVAL;
```

3. Remove 'remainingIds' and 'distance' variable of `revealGobblers()` of `ArtGobblers.sol`
https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L597-L608
Mitigation:
Delete line 599 and 602 and replace line 608 with code below
```
uint256 swapId = currentId + (randomSeed % (FIRST_LEGENDARY_GOBBLER_ID - lastRevealedId - 1));
```

#### Mitigation
Don't define variable that is used only once.
Details are listed on above PoC.

&ensp;
### Duplicate require() Checks Should be a Modifier or a Function

#### Issue
Since below require checks are used more than once,
I recommend making these to a modifier or a function.

#### PoC
Total of 1 instance found.
```
./ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
./ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
```

#### Mitigation
I recommend making duplicate require statement into modifier or a function.

&ensp;
### Use Bit Shifting Instead of Multiplication/Division of 2

#### Issue
The MUL and DIV opcodes cost 5 gas but SHL and SHR only costs 3 gas.
Since MUL/DIV and SHL/SHR result the same, use cheaper bit shifting.

#### PoC
Total of 5 instances found.
```solidity
./ArtGobblers.sol:462:                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
./ArtGobblers.sol:449:            getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);
./ArtGobblers.sol:462:                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
```

#### Mitigation
Use bit shifting instead of multiplication/division.
Example:
```solidity
uint256 center = upper - (upper - lower) / 2; 
Change to:
uint256 center = upper - (upper - lower) >> 2; 
```

&ensp;
### Using Elements Smaller than 32 bytes (256 bits) Might Use More Gas

#### Issue
Since EVM operates on 32 bytes at a time, if the element is smaller than that, the EVM must use more operations
in order to reduce the elements from 32 bytes to specified size. Therefore it is more gas saving to use 32 bytes 
unless it is used in a struct or state variable in order to reduce storage slot. 

Reference: https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html

#### PoC
Total of 3 instances found.
```solidity
./ArtGobblers.sol:615:                uint64 swapIndex = getGobblerData[swapId].idx == 0
./ArtGobblers.sol:623:                uint64 currentIndex = getGobblerData[currentId].idx == 0
./ArtGobblers.sol:899:            uint32 emissionMultiple = getGobblerData[id].emissionMultiple; // Caching saves gas.
```

#### Mitigation
I suggeot using uint256 instead of anything smaller and downcast where needed.

&ensp;
### ++i Costs Less Gas than i++

#### Issue
Prefix increments/decrements (++i or --i) costs cheaper gas than 
postfix increment/decrements (i++ or i--).

#### PoC
Total of 9 instances found.
```solidity
./ERC721.sol:101:            _balanceOf[to]++;
./ERC721.sol:164:            _balanceOf[to]++;
./PagesERC721.sol:117:            _balanceOf[to]++;
./PagesERC721.sol:181:            _balanceOf[to]++;
./Pages.sol:251:            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
./GobblerReserve.sol:37:            for (uint256 i = 0; i < ids.length; i++) {
./ERC721.sol:99:            _balanceOf[from]--;
./ERC721.sol:179:            _balanceOf[owner]--;
./PagesERC721.sol:115:            _balanceOf[from]--;
```

#### Mitigation
Change it to postfix increments/decrements.
It saves 6 gas per instances (for for-loop it saves 6 gas per loop).

&ensp;
### Use Custom Errors to Save Gas

#### Issue
Custom errors from Solidity 0.8.4 are cheaper than revert strings.
Reference: https://blog.soliditylang.org/2021/04/21/custom-errors/

#### PoC
Total of 36 instances found.
```solidity
./SignedWadMath.sol:142:        require(x > 0, "UNDEFINED");
```
```solidity
./Owned.sol:20:        require(msg.sender == owner, "UNAUTHORIZED");
```
```solidity
./ArtGobblers.sol:437:                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
./ArtGobblers.sol:885:        require(from == getGobblerData[id].owner, "WRONG_FROM");
./ArtGobblers.sol:887:        require(to != address(0), "INVALID_RECIPIENT");
./ArtGobblers.sol:889:        require(
                                  msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
                                  "NOT_AUTHORIZED"
                              );
```
```solidity
./GobblersERC1155B.sol:107:        require(owners.length == ids.length, "LENGTH_MISMATCH");
./GobblersERC1155B.sol:149:            require(
                                           ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==
                                               ERC1155TokenReceiver.onERC1155Received.selector,
                                           "UNSAFE_RECIPIENT"
                                       );
./GobblersERC1155B.sol:185:            require(
                                           ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
                                               ERC1155TokenReceiver.onERC1155BatchReceived.selector,
                                           "UNSAFE_RECIPIENT"
                                       );
```
```solidity
./ERC721.sol:36:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
./ERC721.sol:40:        require(owner != address(0), "ZERO_ADDRESS");
./ERC721.sol:69:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
./ERC721.sol:87:        require(from == _ownerOf[id], "WRONG_FROM");
./ERC721.sol:89:        require(to != address(0), "INVALID_RECIPIENT");
./ERC721.sol:91:        require(
                            msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],
                            "NOT_AUTHORIZED"
                        );
./ERC721.sol:118:        require(
                             to.code.length == 0 ||
                                 ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                                 ERC721TokenReceiver.onERC721Received.selector,
                             "UNSAFE_RECIPIENT"
                         );
./ERC721.sol:134:        require(
                             to.code.length == 0 ||
                                 ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                                 ERC721TokenReceiver.onERC721Received.selector,
                             "UNSAFE_RECIPIENT"
                         );
./ERC721.sol:158:        require(to != address(0), "INVALID_RECIPIENT");
./ERC721.sol:160:        require(_ownerOf[id] == address(0), "ALREADY_MINTED");
./ERC721.sol:175:        require(owner != address(0), "NOT_MINTED");
./ERC721.sol:196:        require(
                             to.code.length == 0 ||
                                 ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, "") ==
                                 ERC721TokenReceiver.onERC721Received.selector,
                             "UNSAFE_RECIPIENT"
                         );
./ERC721.sol:211:        require(
                             to.code.length == 0 ||
                                 ERC721TokenReceiver(to).onERC721Received(msg.sender, address(0), id, data) ==
                                 ERC721TokenReceiver.onERC721Received.selector,
                             "UNSAFE_RECIPIENT"
                         );
```
```solidity
./PagesERC721.sol:55:        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
./PagesERC721.sol:59:        require(owner != address(0), "ZERO_ADDRESS");
./PagesERC721.sol:85:        require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
./PagesERC721.sol:103:        require(from == _ownerOf[id], "WRONG_FROM");
./PagesERC721.sol:105:        require(to != address(0), "INVALID_RECIPIENT");
./PagesERC721.sol:107:        require(
                                  msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],
                                  "NOT_AUTHORIZED"
                              );
./PagesERC721.sol:135:            require(
                                      ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                                          ERC721TokenReceiver.onERC721Received.selector,
                                      "UNSAFE_RECIPIENT"
                                  );
./PagesERC721.sol:151:            require(
                                      ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                                          ERC721TokenReceiver.onERC721Received.selector,
                                      "UNSAFE_RECIPIENT"
                                  );
```
```solidity
./VRGDA.sol:32:        require(decayConstant < 0, "NON_NEGATIVE_DECAY_CONSTANT");
```
```solidity
./GobblersERC721.sol:62:        require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
./GobblersERC721.sol:66:        require(owner != address(0), "ZERO_ADDRESS");
./GobblersERC721.sol:95:        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
./GobblersERC721.sol:121:        require(
                                     to.code.length == 0 ||
                                         ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==
                                         ERC721TokenReceiver.onERC721Received.selector,
                                     "UNSAFE_RECIPIENT"
                                 );
./GobblersERC721.sol:137:        require(
                                     to.code.length == 0 ||
                                         ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==
                                         ERC721TokenReceiver.onERC721Received.selector,
                                     "UNSAFE_RECIPIENT"
                                 );
```

#### Mitigation
I suggest implementing custom errors to save gas.

&ensp;