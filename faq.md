---
description: Frequently Asked Questions about Mean Finance
---

# FAQ

## Protocol

### **What is Mean Finance?**

Mean Finance is the state-of-the-art DCA protocol. It enables you to set up actions like “Swap 10 USDC for WBTC every day, for 30 days”. You can create these actions between almost all ERC20 tokens, in the frequency of your choosing. These token swaps will then occur regardless of the asset's price and at regular intervals, reducing the impact of volatility on your investment.&#x20;

### **How does Mean Finance work?**

When you set up a position, you are creating an intention to swap one token for the other. Then, some external user can come and execute the swap for you, honoring the desired frequency of course. When they execute your swap, you are charged a 0.6% fee on the amount that was swapped. This fee is then split between Mean Finance and the swapper. Since you don’t have to execute the swap by yourself, you don’t need to pay any gas.

### **Why should I use Mean Finance?**

Timing the market can be extremely difficult. The goal of performing DCA is to reduce the overall impact of volatility on the price of the target asset; as the price will likely vary each time one of the periodic swaps is executed, the investment is not subject to high volatility. DCA aims to avoid the mistake of making one lump-sum investment that is poorly timed with regard to asset pricing.

Mean Finance will allow you to perform DCA, in a gas-less and decentralized fashion. This means:

* No account required
* No trading limits
* No deposit or withdrawal fees

### **How is the price calculated for each swap?**

Mean Finance relies on on-chain oracles to determine the price at the moment of the swap. Right now Mean Finance uses [Chainlink price feeds](https://chain.link/data-feeds) and [Uniswap V3’s TWAP oracles](https://docs.uniswap.org/protocol/concepts/V3-overview/oracle), but in the future we will support more on-chain oracles.

### **Is Mean Finance audited?**

Mean Finance contracts have been audited by Pessimistic and PeckShield. You can read the reports [here](https://github.com/Mean-Finance/dca-v2-core/tree/main/audits).

## **End User**

### **When do I need to pay gas fees?**

&#x20;**** Users need to pay gas only when they interact with the positions. This includes:

* Creating their position
* Modifying their position
* Withdrawing balance from their position
* Terminating their position
* Setting or revoking permissions

&#x20;End users don’t have to pay gas fees when the swaps are executed.

&#x20;For more information, feel free to check our [fees section](concepts/fees.md).

### **Why hasn't my new position been swapped yet?**

Since swaps are executed by external users, they can decide when to execute them. There is no predefined time for execution, so that's why you might be in this situation.&#x20;

Let's assume that you created a position in the USDC/ETH pair at 9 AM UTC. It could happen that the daily swap for that pair was executed a few minutes before that, at 8.30 AM UTC. So there can't be any more daily swaps until the next day.

### **Do I need to pay any fees?**

Users need to pay gas fees when interacting with their positions. At the same time, there is a protocol fee that is charged in each swap. That fee is currently 0.6%.&#x20;

For example, let’s assume that you’ve created a position that swaps 2000 USDC for ETH each day. Let’s also assume that, when the swap is executed, 2000 USDC = 1 ETH. Instead of getting 1 ETH, you would be getting 1 ETH - 0.6% = 0.994 ETH. That 0.6% will be split between Mean Finance and the user who actually executed the swap.

&#x20;For more information, feel free to check our [fees section](concepts/fees.md).

### **What does it mean for a pair to be stale?**

Mean Finance [incentivizes](concepts/swappers.md#incentives) other users to execute swaps, for a profit. Now, these incentives can be affected by different factors:

* The general sentiment of the crypto market
* The popularity/demand of the tokens involved in the swap
* The volume of the tokens involved in the swap

When a specific pair has some swaps that haven’t been executed in quite some time, the pair is signaled as stale.

### **Why is my position stale?**

You don’t need to execute your swaps by yourself since Mean Finance [incentivizes](concepts/swappers.md#incentives) other users to execute swaps, for a profit. Now, these incentives can be affected by different factors:

* The general sentiment of the crypto market
* The popularity/demand of the tokens involved in the swap
* The volume of the tokens involved in the swap

So it could happen that your swap is not executed within your specified frequency. If this were to happen to your position, remember that fees are only charged on swaps, so your balance will remain unaffected.

### **When will my position be swapped?**

Since swaps are executed by external users, they can decide when to execute them. For example, let’s assume that you had created a position with daily swaps. This means that from 00 AM UTC to 11.59 PM UTC, your position can only be executed once. Once it is executed, it can't be executed again until 00 AM of the following day. The same happens with other frequencies, such as weekly or monthly.\
