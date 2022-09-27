L-1 Missing checks for address(0x0) when assigning values to address state or immutable variables
Zero address should be checked for state variables, immutable variables. A zero address can lead into problems.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L294-L295

L-2 Missing indexed event parameters
Each event should use three indexed fields if there are three or more fields

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L240

L-3 Use of Block.timestamp
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L399
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L763

L-4 abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()
Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L347

N-1 Large multiples of ten should use scientific notation
https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L112
