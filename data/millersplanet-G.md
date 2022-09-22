- Line 437 on `ArtGobblers.sol` `require` statement can be replaced with an `if` statement and a custom error: ```require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");```

	Declare error: 
	
	`error WrongFrom();`
	
	Replace mentioned line with:
	```
	if(getGobblerData[id].owner != msg.sender) revert WrongFrom();
	```

- Line 885 on `ArtGobblers.sol` `require` statement can be replaced with an `if` statement and a custom error: ``` require(from == getGobblerData[id].owner, "WRONG_FROM");```

	Use declared error for line 437 and replace mentioned line with:
	```
	if(getGobblerData[id].owner != from) revert WrongFrom();
	```

- Line 887 on `ArtGobblers.sol` `require` statement can be replaced with an `if` statement and a custom error: ``` require(to != address(0), "INVALID_RECIPIENT");```

	Declare error: 
	
	`error InvalidRecipient();`
	
	Replace mentioned line with:
	```
	if(to == address(0)) revert InvalidRecipient();
	```

- Line 889 on `ArtGobblers.sol` `require` statement can be replaced with an `if` statement and an already declared custom error: 

	```
	require(
		msg.sender == from || 
		isApprovedForAll[from][msg.sender] || 
		msg.sender == getApproved[id],
		"NOT_AUTHORIZED"
	);
	```

	Use the same previously declared error and replace the mentioned line with:
	```
	if(msg.sender != from || !isApprovedForAll[from][msg.sender] || msg.sender != getApproved[id]) revert InvalidRecipient();
	```