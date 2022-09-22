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

	Use the previously declared error and replace the mentioned line with:
	```
	if(
		msg.sender == from || 
		isApprovedForAll[from][msg.sender] || 
		msg.sender == getApproved[id],
		) {
			delete getApproved[id];

			getGobblerData[id].owner = to;

			unchecked {
					uint32 emissionMultiple = getGobblerData[id].emissionMultiple; // Caching saves gas.

					// We update their last balance before updating their emission multiple to avoid
					// penalizing them by retroactively applying their new (lower) emission multiple.
					getUserData[from].lastBalance = uint128(gooBalance(from));
					getUserData[from].lastTimestamp = uint64(block.timestamp);
					getUserData[from].emissionMultiple -= emissionMultiple;
					getUserData[from].gobblersOwned -= 1;

					// We update their last balance before updating their emission multiple to avoid
					// overpaying them by retroactively applying their new (higher) emission multiple.
					getUserData[to].lastBalance = uint128(gooBalance(to));
					getUserData[to].lastTimestamp = uint64(block.timestamp);
					getUserData[to].emissionMultiple += emissionMultiple;
					getUserData[to].gobblersOwned += 1;
			}

			emit Transfer(from, to, id);
	} else revert InvalidRecipient();
	```
