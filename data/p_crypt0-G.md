## GobblersERC721.sol
* * *


[excessive use of require statements](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L62) :

require owner!=0 as not authorized.

use 

````
error ZeroAddres();

if owner == address(0) revert ZeroAddress()
````

***
[owner zero](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L66)

require owner != address(0)

use:
````
if owner == address(0) revert ZeroAddress;
````
***
[require owner == owner || isApprovedForAll[owner][msg.sender] not_authorized](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L121-L126) .

use

````
error NotAuthorized();

if msg.sender != owner && !isApprovedForAll[owner][msg.sender] revert NotAuthorized;
````
***
[usafe_recipient](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L121-L126)
````
error UnsafeRecipient();

if to.code.length !=  0 && ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "")  == ERC721TokenReceiver.onERC721Received.selector
unsafe recipient
````
***
[unsafe_recipient2](https://github.com/code-423n4/2022-09-artgobblers/blob/d2087c5a8a6a4f1b9784520e7fe75afa3a9cbdbe/src/utils/token/GobblersERC721.sol#L137-L142):
````
error UnsafeRecipient();

if to.code.length !=  0 && ERC721TokenReceiver(to).onERC721Received(msg.sender, from, id, "")  == ERC721TokenReceiver.onERC721Received.selector
unsafe recipient
````