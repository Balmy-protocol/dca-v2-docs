# Swaps

## Introduction

{% hint style="info" %}
Before diving into the technical aspect of swaps, we highly recommend that you first read about the [concepts involved](../../concepts/swappers.md).
{% endhint %}

Swaps are an integral part of the Mean Finance ecosystem. There are many [incentives](../../concepts/swappers.md#incentives) in place so that devs would want to execute swaps. Please take into account that in order to fully take advantage of swaps, you **must** have a good understanding of Ethereum, programming and smart contracts.

## Multi Swaps

As we explained [before](../../architecture/core/hub.md#introduction), in v1 we followed a Factory/Pairs approach. Now, with the [Hub](../../architecture/core/hub.md), all pairs live on the same contract. This change allows us to execute what we call _Multi Swaps_.&#x20;

Swaps are normally executed between two different tokens. You send one token, and receive another in exchange. In our case, we can support swaps that involve many different tokens at the same time. Essentially, we can "merge" many pairs together in only one swap.&#x20;

This novel approach saves not only gas, but also reduces the amount of liquidity needed to execute swaps. Let's try to understand this with an example. Let's assume that we have three different pairs with the following amounts to swap:

* WETH/DAI
  * Needs 100 DAI
  * Will reward 200 WETH in return
* WETH/USD
  * Needs 100 WETH
  * Will reward 50 USDC in return
* USDC/DAI
  * Needs 200 USDC
  * Will reward 200 DAI in return

Now, if we were to execute these swaps individually, swappers would need to provide different tokens each time. Even if the swappers were to re-use the award from one pair for the others, there would be a lot of token transfers, making the gas costs pretty high.

But, with Multi Swaps, all these pairs would be merged into one:

* Tokens to provide:
  * 150 USDC
* Reward tokens:
  * 100 DAI
  * 100 WETH

As simple as that. No need for unnecessary token transfers, no need for custom code, it will all be handled by the Hub, so any swapper can use it.
