 --- 

 ## `++i` cost less gas compared to `i++`. Consider using `++{variable}` instead of `{variable}++` 

 ### Files Found: 

 There are 2 instances of this issue. 

- File: src/Pages.sol - line 251 

 ### Mitigation: 

 Consider using `++i` instead of `i++` 

 --- 

 --- 

 ## Constants should be defined rather than using magic numbers 

 ### Files Found: 

 There are 15 instances of this issue. 

- File: src/ArtGobblers.sol - line 849 

- File: src/Pages.sol - line 248 

 ### Mitigation: 

 It is best practice to use constant variables rather than literal values to make the code easier to understand and maintain. 

 --- 

 

 --- 

 ## Using private rather than public for constants, saves gas 

 ### Files Found: 

 There are 8 instances of this issue. 

- File: src/ArtGobblers.sol - line 112 

- File: src/ArtGobblers.sol - line 115 

- File: src/ArtGobblers.sol - line 118 

- File: src/ArtGobblers.sol - line 122 

- File: src/ArtGobblers.sol - line 126 

- File: src/ArtGobblers.sol - line 177 

- File: src/ArtGobblers.sol - line 180 

- File: src/ArtGobblers.sol - line 184 



 ### Mitigation: 

 If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata. 

 --- 

 --- 

 ## `x += y` costs more gas than `x = x + y` for state variables 

 ### Files Found: 

 There are 8 instances of this issue. 

- File: src/ArtGobblers.sol - line 439 

- File: src/ArtGobblers.sol - line 456 

- File: src/ArtGobblers.sol - line 464 

- File: src/ArtGobblers.sol - line 662 

- File: src/ArtGobblers.sol - line 845 

- File: src/ArtGobblers.sol - line 913 

- File: src/ArtGobblers.sol - line 914 

- File: src/Pages.sol - line 244 



 ### Mitigation: 

 Use `x = x + y` instead of `x += y` 

 --- 

 --- 

 ## It costs more gas to initialise non-constant/non-immutable variables to zero than to let the default of zero be applied 

 ### Files Found: 

 There are 3 instances of this issue. 

- File: src/ArtGobblers.sol - line 432 

- File: src/ArtGobblers.sol - line 592 

- File: src/Pages.sol - line 251 

 ### Mitigation: 

 uint is initialised at 0. It cost more gas to initialise variable at 0 

 --- 

