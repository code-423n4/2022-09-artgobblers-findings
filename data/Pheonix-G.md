If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {

Affected code : (ArtGlobbers.sol )

        unchecked {
            for (uint256 i = 0; i < numGobblers; ++i) 
and 

                for (uint256 i = 0; i < cost; ++i) {
                id = gobblerIds[i];

                if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);

                require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

                burnedMultipleTotal += getGobblerData[id].emissionMultiple;

                emit Transfer(msg.sender, getGobblerData[id].owner = address(0), id)