## Remove the ASCII cartoon character to reduce the code size and save gas

In ArtGobblers.sol, Goo.sol, Pages.sol

the ASCII cartoon character 

```
                                                                                 **,/*,
                                                                     *%@&%#/*,,..........,/(%&@@#*
                                                                 %@%,..............................#@@%
                                                              &&,.....,,...............................,/&@*
                                                            (@*.....**............,/,.......................(@%
                                                           &&......*,............./,.............**............&@
                                                          @#......**.............**..............,*........,*,..,@/
                                                         /@......,/............../,..............,*........../,..*@.
                                                        #@,......................*.............../,..........**...#/
                                                      ,@&,.......................................*..........,/....(@
                                                  *@&(*...................................................../*....(@
                                                 @(..*%@@&%#(#@@@%%%%%&&@@@@@@@@@&&#(///..........................#@
                                                 @%/@@@&%&&&@@&%%%%%%%#(/(((/(/(/(/(/(/(/(/(%%&@@@%(/,............#&
                                                  @@@#/**./@%%%&%#/*************./(%@@@@&(*********(@&&@@@%(.....,&@
                                                 ,@/.//(&@@/.     .#@%/******./&&*,      ./@&********%@/**(@#@@#,..(@
                                                 #%****%@.           %@/****./&@      ,.    %&********%@(**&@...(@#.#@
                                                 &#**./@/  %@&&      .@#****./@*    &@@@@&  .@/******./@@((((@&....(@
                                                 ##**./&@ ,&@@@,     #@/****./@@      @@.  .@&*******./@%****%@@@(,
                                                 ,@/**./%@(.      .*@@/********(&@#*,,,,/&@%/*******./@@&&&@@@#
                                                   @&/**@&/%&&&&&%/**.//////*********./************./@&******@*
                                                     /@@@@&(////#%&@@&(**./#&@@&(//*************./&@(********#@
                                                       .@#**.///*****************(#@@@&&&&&@@@@&%(**********./@,
                                                       @(*****%@#*********************&@#*********************(@
                                                       @****./@#*./@@#//***.///(%@%*****%@*********************#@
                                                      #&****./@%************************&@**********************@%
                                                     .@/******.//*******************./@@(************************@/
                                                     /@**********************************************************(@,
                                                     @#*****************************************************%@@@@@@@.
                                                    *@/*************************************************************#@(
                                                    @%***************************************************************./@(
                     /@@&&&@@                     .@/*******************************************************************&@
                    @%######%@.                   @#***************************./%&&&%(**************#%******************&#
                    @%######&@%&@@.             ,@(***./&#********************#@&#####%@&*************&%****************./@,
               &&*,/@%######&@@@*.*@&,         @@****./@&*******************./%@#######%@#***********./@&*****************(@
              ((...*%@&##%@@,..........,,,,%@&@%/*****&%****************./&@#*%@#######&@*#@%*********./@&*****************(@,
              (@#....(@%#&&,...,/...........@(*******(@(****************(@/...*%@@@@@@%*....&@@@@&@@@@@@%/%@@##(************(@.
              ((./(((%@%#&@/,/&@/...........%&*******%@****************./@%,.................#,............/@%***************#@
              *@@####@@%###%&@(@(...........%&*******%@****************%@,,#%/..............................#@/***************&/
              (#.....,&&####&@..%%..........%%*****(@@#****************#@,...................................@(***************(@
              .@@&%%&@@&####&&.............,@(***%@(**********./#%%%%%##&@&#(,...............................#@****************&.
               &#.....(@%###&@*............%@**%@(*******(&@&%#/////////@%...................................#@***************&@
                 #@@@@&%####&@&&&,........%@./@%*****(@@%////////////////@@@%,...............................#@**************#@
                     @@&&&&@@(    /&@@&%%@&@@@%**./&@(///////////////////@%.................................,@(*********./%@&.
                      (@//@%                @%***&&(//////////////////////(&@(**,,,,./(%&@@@%/*,,****,,***./@@&&&&&&&&#//%@
                      (@//%@               (@(*#@#////////////////////////////%@@%%%&@@#////%@/***************************&&
                      (@//%@  .,,,,/#&&&&&&@&*#@#///////////////////////////////@%//&&///////#@(***************************@&(#@@@@@&(*.
               ,@@@@@&&@//%@,,.,,,,,.,..,,#@./@%////////////////////////////////%@**&&////////(@(**************************&#,,,,,,,,,,,,/(#&@&
          &@%*,,,,,,,,#@//%@,,,,,,,,,,,,,,&%*#@(////////////////////////////////%@**&&/////////&@**************************#@.,,,.,,.,,&#.,,...,%@
       (@/,,,,,,,,,,,,(@(/%@,,,,,,,,,,,,,,&%*#@(////////////////////////////////%@./%@/////////#@(*************************&%,,,(%@@@@#*,.     .,/@.
      &%..    *&@%/,.,#@(*#@*,,.,,,,,,,,,,%@/#@(////////////////////////////////%@**#@/////////#@(*****************.//#%@@@@%%(/,...        ...,,,%&
     ,@*.,.       ../((%&&@@@&%#((///,,,,,/@&(@(////////////////////////////////@&**#@/////////%@%###%&&&&@@@@@@%%#(**,,,,,,.         ..,,,,,,,,,,%#
      @(,,,,,..,            ,..   ..,,,**(%%%&@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@%%(((,,.,.,,,,,,.,..,,,,.,.,,,,.,..,.,,.,,,,,.,,,,,,,,,.*@%
       @%,,,,,,,,,,,,,,,.,.,,,      .,.,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,.,,,.,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,.,.,,,,,,#@@,
        ,@@(,.,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,.,,.,,.,.,./#%&@@@@@#
         .@#&@@@@@%*,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,.,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,/&@@@@@%&@%((((#@@.
          .@%((((#@@@/#&@@@@&%#/*,.,..,,,,.,,,,,.,.,,,,,,,,,,,,,,,,..,.,..,,...,,,...,,,,,,.,,,,,,,,,,,../#%&@@@@@@@&%((///*********./(((/&&
             %@&%%#/***********./////(((((((####%%&&@@@@@@@@@@@@@@&@@@@@@@@@@@@@@@@&&%%%%%%%%#((((((((%@&#(((((#%@%/*******************./*/
```

occupies around 60 lines of code per file, and around 180 LOC in total,

removing the ASCII cartoon character can reduce the code size and save gas.

## IT COSTS MORE GAS TO INITIALIZE NON-CONSTANT/NON-IMMUTABLE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L432


```
            for (uint256 i = 0; i < cost; ++i) {
```
            

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/ArtGobblers.sol#L592


```
            for (uint256 i = 0; i < numGobblers; ++i) {
```
            

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251


```
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
            

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37


```
            for (uint256 i = 0; i < ids.length; i++) {
```
           

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L186


```
            for (uint256 i = 0; i < amount; ++i) {
```
            

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114


```
            for (uint256 i = 0; i < owners.length; ++i) {
```
           

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L173


```
            for (uint256 i = 0; i < amount; ++i) {
```
            
## ++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT�S USED IN FOR-LOOPS (--I/I-- TOO)
    
Saves 6 gas per loop


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/Pages.sol#L251


```
            for (uint256 i = 0; i < numPages; i++) _mint(community, ++lastMintedPageId);
```
            

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37


```
            for (uint256 i = 0; i < ids.length; i++) {
```
            

## <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

The overheads outlined below are PER LOOP, excluding the first loop

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset


https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/GobblerReserve.sol#L37


```
            for (uint256 i = 0; i < ids.length; i++) {
```
            

https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC1155B.sol#L114


```
            for (uint256 i = 0; i < owners.length; ++i) {
```

## Use customized error and revert instead of require to save gas

https://blog.soliditylang.org/2021/04/21/custom-errors/

custom errors decrease both deploy and runtime gas costs. Note that runtime gas cost is only relevant when the revert condition is met.