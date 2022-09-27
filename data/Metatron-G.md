### [G-01] ++i costs less gas compared to i++

++i costs **about 5 gas less per iteration** compared to i++ for unsigned integer.
This statement is true even with the optimizer enabled.
Summarized my results where i is used in a loop, is unsigned integer, and you safely can be changed to ++i without changing any behavior,

I've found 4 locations in 4 files:

```
src/Pages.sol:
  250              // Mint the pages to the community reserve while updating lastMintedPageId.
  251:             for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
  252  

src/utils/GobblerReserve.sol:
  36          unchecked {
  37:             for (uint256 i = 0; i < ids.length; i++) {
  38                  artGobblers.transferFrom(address(this), to, ids[i]);

src/utils/token/PagesERC721.sol:
  114          unchecked {
  115:             _balanceOf[from]--;
  116
  117:             _balanceOf[to]++;
  118          }

  180          unchecked {
  181:             _balanceOf[to]++;
  182          }
```

---------------------------------------------------------------------------

### [G-02] An arrays length should be cached to save gas in for-loops

An array’s length should be cached to save gas in for-loops
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset).
Caching the array length in the stack saves around **3 gas per iteration**.

I've found 2 locations in 2 files:

```
src/utils/GobblerReserve.sol:
  36          unchecked {
  37:             for (uint256 i = 0; i < ids.length; i++) {
  38                  artGobblers.transferFrom(address(this), to, ids[i]);

src/utils/token/GobblersERC1155B.sol:
  113          unchecked {
  114:             for (uint256 i = 0; i < owners.length; ++i) {
  115                  balances[i] = balanceOf(owners[i], ids[i]);

```

---------------------------------------------------------------------------


### [G-03] Using default values is cheaper than assignment

If a variable is not set/initialized, it is assumed to have the default value 0 for uint, and false for boolean.
Explicitly initializing it with its default value is an anti-pattern and wastes gas.
For example: ```uint8 i = 0;``` should be replaced with ```uint8 i;```

I've found 7 locations in 5 files:

```
src/ArtGobblers.sol:
  431  
  432:             for (uint256 i = 0; i < cost; ++i) {
  433                  id = gobblerIds[i];

  591          unchecked {
  592:             for (uint256 i = 0; i < numGobblers; ++i) {
  593                  /*//////////////////////////////////////////////////////////////

src/Pages.sol:
  250              // Mint the pages to the community reserve while updating lastMintedPageId.
  251:             for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
  252  

src/utils/GobblerReserve.sol:
  36          unchecked {
  37:             for (uint256 i = 0; i < ids.length; i++) {
  38                  artGobblers.transferFrom(address(this), to, ids[i]);

src/utils/token/GobblersERC721.sol:
  185  
  186:             for (uint256 i = 0; i < amount; ++i) {
  187                  getGobblerData[++lastMintedId].owner = to;  //@audit cannot mint id 0 with batch because of the ++c (can change to start from)

src/utils/token/GobblersERC1155B.sol:
  113          unchecked {
  114:             for (uint256 i = 0; i < owners.length; ++i) {
  115                  balances[i] = balanceOf(owners[i], ids[i]);

  172          unchecked {
  173:             for (uint256 i = 0; i < amount; ++i) {
  174                  ids[i] = ++lastMintedId; // Increment id while setting.
```

---------------------------------------------------------------------------

### [G-04] Custom errors save gas

From solidity 0.8.4 and above,
Custom errors from are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)
Source: https://blog.soliditylang.org/2021/04/21/custom-errors/:
```Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.```

I've found 20 locations in 4 files:

```
src/ArtGobblers.sol:
  436  
  437:                 require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");
  438  

  884      ) public override {
  885:         require(from == getGobblerData[id].owner, "WRONG_FROM");
  886  
  887:         require(to != address(0), "INVALID_RECIPIENT");
  888  
  889:         require(
  890              msg.sender == from || isApprovedForAll[from][msg.sender] || msg.sender == getApproved[id],

src/utils/token/GobblersERC721.sol:
   61      function ownerOf(uint256 id) external view returns (address owner) {
   62:         require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");
   63      }

   65      function balanceOf(address owner) external view returns (uint256) {
   66:         require(owner != address(0), "ZERO_ADDRESS");
   67  

   94  
   95:         require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");
   96  

  120  
  121:         require(
  122              to.code.length == 0 ||

  136  
  137:         require(
  138              to.code.length == 0 ||

src/utils/token/GobblersERC1155B.sol:
  106      {
  107:         require(owners.length == ids.length, "LENGTH_MISMATCH");
  108  

  148          if (to.code.length != 0) {
  149:             require(
  150                  ERC1155TokenReceiver(to).onERC1155Received(msg.sender, address(0), id, 1, data) ==

  184          if (to.code.length != 0) {
  185:             require(
  186                  ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==

src/utils/token/PagesERC721.sol:
   54      function ownerOf(uint256 id) external view returns (address owner) {
   55:         require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");
   56      }

   58      function balanceOf(address owner) external view returns (uint256) {
   59:         require(owner != address(0), "ZERO_ADDRESS");
   60  

   84  
   85:         require(msg.sender == owner || isApprovedForAll(owner, msg.sender), "NOT_AUTHORIZED");
   86  

  102      ) public {
  103:         require(from == _ownerOf[id], "WRONG_FROM");
  104  
  105:         require(to != address(0), "INVALID_RECIPIENT");
  106  
  107:         require(
  108              msg.sender == from || isApprovedForAll(from, msg.sender) || msg.sender == getApproved[id],

  134          if (to.code.length != 0)
  135:             require(
  136                  ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "") ==

  150          if (to.code.length != 0)
  151:             require(
  152                  ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, data) ==

```

---------------------------------------------------------------------------

### [G-05] Upgrade pragma to 0.8.15 to save gas

Across the whole solution, the declared pragma is 0.8.0
Upgrading to 0.8.15 has many benefits, cheaper on gas is one of them.
The following information is regarding 0.8.15 over 0.8.14. Assume that over 0.8.0 the save is higher!
```
According to the release note of 0.8.15: https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/
The benchmark shows saving of 2.5-10% Bytecode size,
Saving 2-8% Deployment gas,
And saving up to 6.2% Runtime gas.
```

---------------------------------------------------------------------------

### [G-06] Use immutable for state variables that are only set in the constructor - to save gas

Save around 20,000 gas per uint256 variable (use 100+3 gas for immutable variable instead of 20000 for regular public)

I've found 4 locations in 2 files:

```
src/utils/token/GobblersERC721.sol:
  22  
  23:     string public name;
  24  
  25:     string public symbol;
  26  

src/utils/token/PagesERC721.sol:
  23  
  24:     string public name;
  25  
  26:     string public symbol;
  27  
```
