# Table of contents

- **[[0x0] Disclaimer](#0x0)**
- **[[G-01] Try ++i instead of i++](G-01)**
- **[[G-02] Try `unchecked{++i}` instead of `i++` in loops](G-02)**
- **[[G-03] Consider `a = a + b` instead of `a += b`](G-03)**
- **[[G-04] Consider marking onlyOwner functions as payable](G-04)**
- **[[G-05] Use binary shifting instead of `a / 2^x, x > 0`](G-05)**
- **[[G-07] Add `require()` before some computations](G-07)**
- **[[G-11] Cache the array.length into a variable](G-11)**
- **[[G-12] Unused the named return variables](G-12)**
- **[[G-15] Use `private/internal` for `constants/immutable/state` variables instead of `public`](G-15)**
- **[[G-20] Mark functions as `external` instead of `public`, if there are no internal calls](G-20)**
- **[[G-21] Use `calldataload` instead of `mload`](G-21)**
- **[[G-23] Try `unchecked{++x}` over `x += 1`](G-23)**


## Disclaimer<a name="0x0"></a>

- Please, consider everything described below as a general recommendation. These notes will represent potential possibilities to optimize gas consumption. It's okay, if something is not suitable in your case. Just let me know the reason in the comments section. Enjoy!

## **[G-01] Try ++i instead of i++**<a name="G-01"></a>

### ***Description:***

  - In case of `i++`, the compiler has to to create a temp variable to return `i` (if there's a need) and then `i` gets incremented.  
  - In case of `++i`, the compiler just simply returns already incremented value.

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: lib/solmate/src/tokens/ERC721.sol 
      .......................................

        // Lines: [99-101]
            _balanceOf[from]--;
            _balanceOf[to]++;
            
        // Lines: [178-180]
            unchecked {
                _balanceOf[owner]--;
            }


      file: src/utils/token/PagesERC721.sol 
      .......................................
        // Lines: [115-117]
            _balanceOf[from]--;
            _balanceOf[to]++;


      file: src/Pages.sol
      ...............................
      
        // Lines: [251-251]
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

      file: src/utils/GobblerReserve.sol 
      ..................................

        // Lines: [37-39]
            for (uint256 i = 0; i < ids.length; i++) {
                artGobblers.transferFrom(address(this), to, ids[i]);
            }

    ```

## **[G-02] Try `unchecked{++i};` instead of `i++;`/`++i;` in loops**<a name="G-02"></a>

### ***Description:***

  - Since [Solidity 0.8.0](https://github.com/ethereum/solidity/releases/tag/v0.8.0), all arithmetic operations revert on over- and underflow by default Therefore, `unchecked` box can be used to prevent all unnecessary checks, if it's no a way to get a reversion.
  
  - There are ~1e80 atoms in the universe, so 2^256 is closed to that number, therefore it's no a way to be overflowed, because of the gas limit as well. 

### ***All occurances:***

- Contracts:

    ```Solidity
      file: src/Pages.sol
      ...............................
      
        // Lines: [251-251]
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

      file: src/utils/GobblerReserve.sol 
      ..................................

        // Lines: [37-39]
            for (uint256 i = 0; i < ids.length; i++) {
                artGobblers.transferFrom(address(this), to, ids[i]);
            }

    ```

## **[G-03] Consider `a = a + b` instead of `a += b`**<a name="G-03"></a>

### ***Description:***

- It has an impact on the deployment cost and the cost for distinct transaction as well.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/ArtGobblers.sol 
      ...............................
      
        // Lines: [439-439]
              burnedMultipleTotal += getGobblerData[id].emissionMultiple;

        // Lines: [456-456]
              getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple. 

        // Lines: [464-446]
              legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.

        // Lines: [662-662]
              getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);

        // Lines: [844-844]
            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);

        // Lines: [912-913]
            getUserData[to].emissionMultiple += emissionMultiple;
            getUserData[to].gobblersOwned += 1;

        // Lines: [905-906]
            getUserData[from].emissionMultiple -= emissionMultiple;
            getUserData[from].gobblersOwned -= 1;

      file: lib/solmate/src/utils/SignedWadMath.sol 
      .............................................

        // Lines: [201-207]
            r *= 1677202110996718588342820967067443963516166;
            // add ln(2) * k * 5e18 * 2**192
            r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
            // add ln(2**96 / 10**18) * 5e18 * 2**192
            r += 600920179829731861736702779321621459595472258049074101567377883020018308;
            // base conversion: mul 2**18 / 2**192
            r >>= 174;

      file: src/Pages.sol
      ...............................

        // Lines: [244-244]
            uint256 newNumMintedForCommunity = numMintedForCommunity += uint128(numPages);

      file: src/utils/token/GobblersERC721.sol 
      ........................................

        // Lines: [184-184]
            getUserData[to].gobblersOwned += uint32(amount);

    ```

## **[G-04] Consider marking onlyOwner functions as payable**<a name="G-04"></a>

### ***Description:***

- This one is a bit questionable, but you can try that out. So, the compiler adds some extra conditions in case of non-payable, but we know that `onlyOwner` modifier will be reverted, if the user invoke following methods.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/Goo.sol 
      ...............................
      
        // Lines: [101-103]
          function mintForGobblers(address to, uint256 amount) external only(artGobblers) {
            _mint(to, amount);
          }

        // Lines: [108-110]
          function burnForGobblers(address from, uint256 amount) external only(artGobblers) {
              _burn(from, amount);
          }
          
        // Lines: [115-117]
          function burnForPages(address from, uint256 amount) external only(pages) {
              _burn(from, amount);
          }

        ```

## **[G-05] Use binary shifting instead of `a / 2^x, x > 0` or `a * 2^x, x > 0`**<a name="G-05"></a>

### ***Description:***

- It's also pretty impactful one, especially in loops.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: lib/VRGDAs/src/LogisticVRGDA.sol 
      ...............................
      
        // Lines: [47-47]
          // Comment:  (2 * 1e18) == shift and then multiply by 1e18;
          logisticLimitDoubled = logisticLimit * 2e18;

      file: src/ArtGobblers.sol 
      ...............................

        // Lines: [449-449]
            getGobblerData[gobblerId].emissionMultiple = uint32(burnedMultipleTotal * 2);

        // Lines: [461-461]
            legendaryGobblerAuctionData.startPrice = uint120(
                cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2
            );
      ```


## **[G-07] Add `require() before some computations`**<a name="G-07"></a>


### ***Description:***

- Everyting above `require()` takes some gas for execution, therefore if the statement reverts gas will not be retrieved.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/utils/token/GobblersERC1155B.sol 
      ..........................................
      
        // Lines: [184-190]
          if (to.code.length != 0) {
              require(
                  ERC1155TokenReceiver(to).onERC1155BatchReceived(msg.sender, address(0), ids, amounts, data) ==
                      ERC1155TokenReceiver.onERC1155BatchReceived.selector,
                  "UNSAFE_RECIPIENT"
              );
          }
    ```

## **[G-11] Cache the array.length into a variable**<a name="G-11"></a>

### ***Description:***

- Reduces the extra `DUP<X>` for every iteration.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/utils/GobblerReserve.sol 
      ...............................
      
        // Lines: [37-39]
            for (uint256 i = 0; i < ids.length; i++) {
                artGobblers.transferFrom(address(this), to, ids[i]);
            }

      file: src/utils/token/GobblersERC1155B.sol 
      ..........................................

        // Lines: [113-117]
          unchecked {
              for (uint256 i = 0; i < owners.length; ++i) {
                  balances[i] = balanceOf(owners[i], ids[i]);
              }
          }

    ```

## **[G-12] Unused the named return variables**<a name="G-12"></a>

### ***Description:***

- Unnecessary gas usage on a deployment side.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/utils/token/GobblersERC1155B.sol 
      ..........................................
      
        // Lines: [55-57]
          function ownerOf(uint256 id) public view virtual returns (address owner) {
              return getGobblerData[id].owner;
          }


      file: src/utils/token/PagesERC721.sol 
      .......................................

        // Lines: [72-76]
            function isApprovedForAll(address owner, address operator) public view returns (bool isApproved) {
                if (operator == address(artGobblers)) return true; // Skip approvals for the ArtGobblers contract.

                return _isApprovedForAll[owner][operator];
            }

    ```

## **[G-15] Use `private/internal` for `constants/immutable/state` variables instead of `public`**<a name="G-15"></a>

### ***Description:***

- Optimization comes from not creating a getter function for each `public` instance.
  
### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/Pages.sol 
      ...........................
      
        // Lines: [96-96]
          string public BASE_URI;

        // Lines: [103-103]
            uint256 public immutable mintStart;

    ```

## **[G-20] Mark functions as `external` instead of `public`, if there are no internal calls**<a name="G-20"></a>

### ***Description:***

- Functions marked by `external` use call data to read arguments, where `public` will first allocate in local memory and then load them.
  
### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: lib/solmate/src/tokens/ERC721.sol 
      .......................................

        // Lines: [66-74]
          function approve(address spender, uint256 id) public virtual {
              address owner = _ownerOf[id];

              require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

              getApproved[id] = spender;

              emit Approval(owner, spender, id);
          }
      ```
## **[G-21] Use `calldataload` instead of `mload`**<a name="G-21"></a>

### ***Description:***

- After Berlin hard fork, to load a non-zero byte from calldata dropped from 64 units of gas to 16 units, therefore if you do not modify args, use a calldata instead of memory. Here you need to explicitly specify `calldataload`, or replace `memory` with `calldata`. If the args are pretty huge, allocating args in memory will cost a lot. 
  
### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: lib/solmate/src/tokens/ERC721.sol 
      .......................................
      
        // Lines: [57-60]
          constructor(string memory _name, string memory _symbol) {
              name = _name;
              symbol = _symbol;
          }

        // Lines: [204-208]
            function _safeMint(
                address to,
                uint256 id,
                bytes memory data
            ) internal virtual {}

      file: src/ArtGobblers.sol 
      ...............................

        // Lines: [287-310]
            constructor(
                // Mint config:
                bytes32 _merkleRoot,
                uint256 _mintStart,
                // Addresses:
                Goo _goo,
                Pages _pages,
                address _team,
                address _community,
                RandProvider _randProvider,
                // URIs:
                string memory _baseUri,
                string memory _unrevealedUri
            )
                GobblersERC721("Art Gobblers", "GOBBLER")
                Owned(msg.sender)
                LogisticVRGDA(
                    69.42e18, // Target price.
                    0.31e18, // Price decay percent.
                    // Max gobblers mintable via VRGDA.
                    toWadUnsafe(MAX_MINTABLE),
                    0.0023e18 // Time scale.
                )
            {

            // etc..

      ```
## **[G-23] Try `unchecked{++x}` over `x += 1`**<a name="G-23"></a>
### ***Description:***
- Optimization comes from reducing the amount of operations on a stack level. 

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/ArtGobblers.sol 
      ...............................
      
        // Lines: [464-464]
          legendaryGobblerAuctionData.numSold += 1; // Increment the # of legendaries sold.

        // Lines: [906-906]
          getUserData[from].gobblersOwned -= 1;

        // Lines: [913-913]
          getUserData[to].gobblersOwned += 1;

      ```

## Kudos for the quality of the code! It's pretty easy to explore
