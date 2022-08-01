# Libraries

We've built quite a few libraries on the Periphery that should make your life as a dev easier.

## InputBuilding

Like we explain in the [swap section](../../guides/swaps/executing-a-swap/regular-swaps.md), the parameters needed to execute a swap are quite particular. In order to help with integrations, we've built this library that takes a more human-friendly parameters format, and converts them into the format that the Hub expects.

## ModifyPositionWithRate

In v1, when you modified a position, you would specify a new rate and amount of swaps. In v2 we changed the approach, but we've built this library for users that still prefer the old one.

You can check the examples in the [position management section](../../guides/position-management/modifying-a-position.md#helper-library).

## SecondsUntilNextSwap

Executing a DCA position is all about timing. If you'd like to understand how much longer you'll have to wait until the next swap is available, you can use this library that will calculate it for you.
