Contract:  GobblerReserve.sol

Changing i++ to ++i in the for loop of:

	line 37

	will save around 5 gas per iteration.
	
Recommendation:

	for (uint256 i = 0; i < ids.length; ++i) {

This will also bring more code consistency given that all other for loops in contracts
under review follow this logic.