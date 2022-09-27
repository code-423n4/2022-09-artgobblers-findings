# Table of contents

- **[[L-01] Lack of two-step ownership transfer](L-01)**


## **[L-01] Lack of two-step ownership transfer**<a name="L-01"></a>

### ***Description:***
  - There is a little risk of putting the wrong address as a `newOwner`, therefore it's good to implement two-step ownership transfer which is based on proposing the ownership by previous owner and then accepting that by `pendingAdmin`.
### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: lib/solmate/src/auth/Owned.sol 
      .....................................
      
        // Lines: [39-43]
          function setOwner(address newOwner) public virtual onlyOwner {
              owner = newOwner;
              emit OwnerUpdated(msg.sender, newOwner);
          }

### ***Recommendations:***
  - Previous owner only is able to propose an ownership.
  - See the implementation here: https://bttcscan.com/address/0xDC8f714d9a5c67266d6da2E5d3267d1587Ea5710#code 
