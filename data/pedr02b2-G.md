### Lock Down pragma versions

Lock down pragma versions for all contracts under scope to versions used for testing etc to mitigate the potential for vulnerabilities to be introduced if versions happen to change before deploying live, a reaudit of the code would be required in such instances.

### Replace bitwise left shift with multiplication

Replace <<1 with *2 for better readability, too follow the syntax already used within the project on other equations, and using mul instead of bitwise shift can be a faster operation on mulitiplications.

line844
            uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);

Method	        Time
Div: i / 4	        358
Div: i >> 2	224
Mul: i * 4	        206
Mul: i << 2	255
Mod: i % 4	918
Mod: i & 3	254