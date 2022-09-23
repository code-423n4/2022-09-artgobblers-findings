## Fisher Yates Shuffle Modulo Bias
In the Fisher Yates shuffle, there can be a very small modulo bias when choosing the distance:
```uint256 distance = randomSeed % remainingIds;```
For instance, let's say `remainingIds` is 9000. Because 2^64 mod 9000 = 3616, the distances {0, ..., 3615} have a likelihood that is increased by 4.9 * 10^(-16). In theory, the algorithm therefore does not perform a true random permutation (in the mathematical sense), but in practice, this should not be a problem. There might be some mint positions that are preferrable / where some ids are more likely by a very small amount.

## Gobbler reveals can happen more often than every 24 hours
According to a comment in `requestRandomSeed`, reveals should only happen every 24 hours: `// We want at most one batch of reveals every 24 hours.` This is currently not always enforced. `nextRevealTimestamp` is always increased by 24 hours, not set to `block.timestamp + 1 days`. Because of that, multiple successive reveals are possible when no reveals happened for a long time. For instance, when no reveals happened for 30 days, `requestRandomSeed` can be called 30 times in a row (if enough Gobblers are minted such that always one can be revealed for every call).

## Gobbling-as-a-Service
In my opinion, approved addresses should also be able to call `gobble`. This would allow other projects to integrate into Art Gobblers better. For instance, a protocol could offer GaaS, "Gobbling-as-a-service", where you let them manage the gobbling for your gobbler and they have portfolio managers or algorithms that carefully choose the art to feed to the gobbler in order to maximize his value. In 5 years, GaaS may be a huge industry and people want to become Gobbling quants and Gobbling portfolio managers ;)

## LibGoo rounding causing checkpointing to affect result
Because of the rounding in `LibGoo`, it currently makes a (small) difference if you regularly checkpoint or not.

Consider the following diff, simulating a transfer to itself every 12 seconds:
```diff
--- a/test/ArtGobblers.t.sol
+++ b/test/ArtGobblers.t.sol
@@ -173,10 +173,18 @@ contract ArtGobblersTest is DSTestPlus {
         gobblers.mintFromGoo(type(uint256).max, true);
         //asert owner is correct
         assertEq(gobblers.ownerOf(2), users[0]);
+        uint startTime = block.timestamp;
         //asert balance went down by expected amount
-        uint256 finalBalance = gobblers.gooBalance(users[0]);
-        uint256 paidGoo = initialBalance - finalBalance;
-        assertEq(paidGoo, gobblerPrice);
+        // for (uint256 i = 0; i <= 86400 * 30 * 12; i += 12) {
+        //     vm.prank(users[0]);
+        //     gobblers.transferFrom(users[0], users[0], 2);
+        //     vm.warp(startTime + 3 days + i);
+        // }
+        vm.warp(startTime + 3 days + 86400 * 30 * 12);
+        console.log(gobblers.gooBalance(users[0]));
+        // uint256 finalBalance = gobblers.gooBalance(users[0]);
+        // uint256 paidGoo = initialBalance - finalBalance;
+        // assertEq(paidGoo, gobblerPrice);
     }
```

In theory, the logged number should be equal, no matter if we previously run the commented out lines (which transfer the token to oneself every 12 seconds for 12 months) or do not run it. In practice, the results are:
- With the token transfer every 12 seconds: 298319329386043864916293
- Without it: 298319329386047666906459
(a difference of 3801990166 tokens)

This will probably be very hard to avoid, but it is something to be aware of.