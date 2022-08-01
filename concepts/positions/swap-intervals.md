# Swap Intervals

As we explained earlier, users don't have to pay for their swaps to be executed. This is because they won't be executing them at all. Mean Finance tries to incentivize the involvement of a new actor, the market maker or "swapper". Mean Finance provides different [incentives](../swappers.md#incentives) to swappers, so that they execute all swaps in all positions.&#x20;

## Swap window

Since swaps are executed by these swappers, they can decide when to execute them. Let's try to explain this with John's position.&#x20;

John had selected "daily" as his swap interval. This means that from 00 AM to 11.59 PM, his position can only be executed once. Once it is executed, it can't be executed again until 00 AM of the following day, but there is no guarantee about swaps being executed at a specific time of the day.

The same thing happens with other intervals, such as weekly. Once a weekly position is executed, it can't be executed again until 00 AM the next Monday.

## Stale positions

As we said before, there are different incentives in place that should encourage swappers to execute all swaps. Now, these incentives can be affected by different factors:

* The general sentiment of the crypto market
* The popularity/demand of the tokens involved in the swap
* The volume of the tokens involved in the swap

So it could happen that certain positions are not swapped for extended periods of time. We call these "stale" positions. If this were to happen to your position, remember that fees are only charged on swaps (more on [how fees work](../fees.md#protocol-fees)), so your balance remains unaffected.
