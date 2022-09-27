## AN ARRAY’S LENGTH SHOULD BE CACHED TO SAVE GAS IN FOR-LOOPS

    File:   main/src/utils/GobblerReserve.sol   #1
    37            for (uint256 i = 0; i < ids.length; i++) {

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

      File:   main/src/utils/token/GobblersERC1155B.sol   #2
      114            for (uint256 i = 0; i < owners.length; ++i) {

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114

## State Variables declared only set in the constructor should be  declared immutable

    File:   main/src/utils/token/GobblersERC721.sol   #1
    23    string public name;

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L23

      File:   main/src/utils/token/GobblersERC721.sol   #2
      25   string public symbol;

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L25

## ++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)
Saves 6 gas PER LOOP

    File:   main/src/Pages.sol   #1
    251            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251

      File:   main/src/utils/GobblerReserve.sol   #2
      37            for (uint256 i = 0; i < ids.length; i++) {

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

## Using private rather than public for constants, saves gas
If needed, the value can be read from the verified contract source code

    File:   main/src/ArtGobblers.sol   #1
    112    uint256 public constant MAX_SUPPLY = 10000;

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112

      File:   main/src/ArtGobblers.sol   #2
      115    uint256 public constant MINTLIST_SUPPLY = 2000;

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L115

      File:   main/src/ArtGobblers.sol   #3
      118    uint256 public constant LEGENDARY_SUPPLY = 10;

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L118

      File:   main/src/ArtGobblers.sol   #4
      177    uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L177

## It costs more gas to initialize variables to zero than to let the default of zero be applied

    File:   main/src/ArtGobblers.sol   #1
    432            for (uint256 i = 0; i < cost; ++i) {

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L432

      File:   main/src/ArtGobblers.sol   #2
      592            for (uint256 i = 0; i < numGobblers; ++i) {

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L592

      File:   main/src/Pages.sol   #3
      251            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L251

      File:   main/src/utils/GobblerReserve.sol   #4
      37            for (uint256 i = 0; i < ids.length; i++) {

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L37

      File:   main/src/utils/token/GobblersERC1155B.sol   #5
      114             for (uint256 i = 0; i < owners.length; ++i) {

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L114

      File:   main/src/utils/token/GobblersERC1155B.sol   #6
      173            for (uint256 i = 0; i < amount; ++i) {
      
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC1155B.sol#L173

## Use custom errors rather than revert()/require() strings to save deployment gas
Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version

     File:   main/src/ArtGobblers.sol   #1
     696            if (gobblerId == 0) revert("NOT_MINTED"); // 0 is not a valid id for Art Gobblers.

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L696

       File:   main/src/ArtGobblers.sol   #2
       705        if (gobblerId < FIRST_LEGENDARY_GOBBLER_ID) revert("NOT_MINTED");
       
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L705

      File:   main/src/ArtGobblers.sol   #3
      711        revert("NOT_MINTED"); // Unminted legendaries and invalid token ids.

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L711

      File:   main/src/Pages.sol   #4
      266        if (pageId == 0 || pageId > currentId) revert("NOT_MINTED");

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L266

       File:   main/src/utils/token/GobblersERC721.sol   #5
       62                require((owner = getGobblerData[id].owner) != address(0), "NOT_MINTED");

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L62

      File:   main/src/utils/token/GobblersERC721.sol   #6
      66        require(owner != address(0), "ZERO_ADDRESS");

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L66

## Use Solidity version of atleast 0.8.15

* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L2
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Pages.sol#L2
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/Goo.sol#L2
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/GobblerReserve.sol#L2
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/GobblersERC721.sol#L2
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/token/PagesERC721.sol#L2
* https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/utils/rand/RandProvider.sol#L2