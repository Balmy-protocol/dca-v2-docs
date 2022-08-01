# Swapper

### Introduction

Having improved a lot of the UX with the [Companion](companion.md) and having a solid [Core](../core/) we set upon ourselves to make it as easy as possible to allow anyone execute the swaps in our protocol, lowering the barrier of entry into becoming a market maker.

### Solution

We know that executing swaps with the Hub can be hard if you are not a solidity developer, or have a custom team developing market making algorithms. That's why we created a specialized smart contract for it with two simple ways of executing swaps, the ["Swap for Caller"](../../guides/swaps/executing-a-swap/swap-for-caller.md) and the ["Swap with DEX"](../../guides/swaps/executing-a-swap/swap-with-dex.md).

