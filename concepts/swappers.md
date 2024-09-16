# Swaps

## Introduction

We've already explained how positions work, and how they create an intention to swap tokens. Now, all positions need to have their swaps executed. Since doing them separately would be very expensive in terms of gas, Mean Finance aggregates all positions in the same pair, so that can be swapped together.

For instance, going back to our DAI/WETH pair example, all daily positions will be executed together. Regardless of whether they are from DAI to WETH or vice versa, they will all be executed in one transaction.&#x20;

Now, let's assume this is the pair's situation:

* Need to swap 100 DAI in total for WETH
* Need to swap 300 WETH in total for DAI
* Oracle says 1 DAI = 1 WETH

We can see that the we need 200 DAI, and we have 200 extra WETH. The idea is that whoever executes the swap provides the missing DAI and, in return, we will reward them with the extra WETH.

## Swappers

As we said before, positions are not executed by their owners. Instead, they are executed by market makers or "swappers".&#x20;

These swappers have two roles:

* Execute swaps
* Provide the liquidity needed for those swaps

### Incentives

In order to encourage swappers to provide the needed liquidity and spend gas in executing swaps, we've come up with the following incentives:

1. Since the Mean Finance pairs use price oracles, there is a potential arbitrage opportunity between our oracles and other DEXes
2. When executing the swap, we will send the reward (in the previous example it was 200 WETH) optimistically, similarly to flash swaps
3. We will also allow swappers to borrow the protocol's entire liquidity during the swap, for free, similarly to flash loans
4. We will share a portion of the swap's protocol fees with the swapper. Right now, they will get 3/4 of the 0.6%, so they would be getting 0.45% of the tokens being swapped

## Example

Let's see how all incentives would work in action. Let's assume that the swap to execute looks like this:

* Need to swap 100 DAI in total for WETH
* Need to swap 300 WETH in total for DAI
* Oracle says 1 DAI = 1 WETH

### Summary

So what can we tell about this swap?

|                       |       **DAI**       |       **WETH**      |
| :-------------------: | :-----------------: | :-----------------: |
|    Total available    |         100         |         300         |
|      Total needed     |         300         |         100         |
|   Total fees (0.6%)   |  300 \* 0.6% = 1.8  |  100 \* 0.6% = 0.6  |
|  Swapper fees (0.45%) | 300 \* 0.45% = 1.35 | 100 \* 0.45% = 0.45 |
| Treasury fees (0.15%) | 300 \* 0.15% = 0.45 | 100 \* 0.15% = 0.15 |

### Execution

And how will the execution look like?

1. The protocol will send 200.45 WETH to the swapper
   * 200.45 WETH = 300 WETH (available) - 100 WETH (needed) + 0.45 WETH (swapper fee)
2. The protocol will call whatever contract the swapper specifies
3. The protocol will expect to have 198.55 DAI returned
   * 198.65 DAI = 300 DAI (needed) - 100 DAI (available) - 1.35 DAI (swapper fee)
4. The protocol will send to the treasury:
   * 0.45 DAI (treasury fee)
   * 0.15 WETH (treasury fee)

### **Changes in balances**

|      | Protocol | Swapper  | Treasury |
| ---- | -------- | -------- | -------- |
| DAI  | + 198.2  | - 198.65 | + 0.45   |
| WETH | - 200.6  | + 200.45 | + 0.15   |
