dont use greater then operator in L415..

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L415

 if (gobblerId > MAX_SUPPLY) revert NoRemainingLegendaryGobblers();

.......if user somehow always make 'gobblerId' greater then MAX_SUPPLY....i.e.
as you have defined in function mintLegendaryGobbler on L411 (https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L411 ) that gobblerId = FIRST_LEGENDARY_GOBBLER_ID + legendaryGobblerAuctionData.numSold;
where FIRST_LEGENDARY_GOBBLER_ID  as calculated from above is equal to 9991 and user wants 9 more points to not get working this if condition correctly as it suppose to..
therefore ...below cost will not executed on L418...(https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol#L418)

Recommendation: try to use != iff possible... ....