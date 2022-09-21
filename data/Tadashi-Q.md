### **Left shift could be replaced by multiplication without losing gas**

**Summary**: `<< 1` can be replaced by `*2` without losing gas and increasing readability.

**Details**: [L844](https://github.com/artgobblers/art-gobblers/blob/c821b6d48f836ccf5d152c75584bb9792bba774f/src/ArtGobblers.sol#L844) of ArtGobblers.sol contains a left shift which is equivalent to multiplication by 2. While representing a multiplication by a power of 2 by left shifts can save gas, this do not occur here. Hence, consider changing the left shift by an explicit multiplication by 2.

**Impact**: Code QA

**Suggestion**: Consider changing the left shift by a multiplication by 2.