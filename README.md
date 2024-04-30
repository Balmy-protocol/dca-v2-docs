# Introduction to the protocol

Balmy is the state-of-the-art DCA open protocol that enables users (or dapps) to Dollar Cost Average (DCA) any ERC20 into any ERC20 with their preferred period frequency, without sacrificing decentralization or giving up personal information to any centralized parties. We do all of this by leveraging both [Chainlink and Uniswap V3 TWAP oracles](concepts/price-oracle.md).

> [Dollar-cost averaging](https://www.investopedia.com/terms/d/dollarcostaveraging.asp) is a tool an investor can use to build savings and wealth over a long period while neutralizing the short-term volatility in the market.&#x20;
>
> \
> The purchases occur regardless of the asset's price and at regular intervals. In effect, this strategy removes much of the detailed work of attempting to time the market in order to make purchases of assets at the best prices.
>
> \- Investopedia

{% hint style="danger" %}
**Important disclosures**

* **We can change the oracle provider smart-contract via a timelocked function.**\
  In order to support more pairs, we have made the oracle provider upgradable. However, for security reasons, it is behind a timelocked function.
* **We can pause** [**swaps**](guides/swaps/)\
  This will allow us to react swiftly in the case our smart contracts are vulnerable against an exploit. Users will still be able to withdraw funds from their positions during the paused period.
{% endhint %}

## **Protocol Design**

The protocol is designed in such way that users get protection against volatility, while market makers are well-incentivized to execute the trades:

### **Users**

* Get the protection against market volatility
* Get MEV-protected trades
* Gas-less DCA

### **Market Makers**

* Can leverage the whole protocol balance to execute the trade (see [flash-swaps](https://docs.mean.finance/guides/swaps/executing-a-swap/flash-swaps))
* Can earn a percentage of the fees
* Can arbitrage our pool

Our design minimizes any type [MEV](https://github.com/Dogetoshi/MEV) related activity for our users. As [Coincidence of Wants](https://en.wikipedia.org/wiki/Coincidence\_of\_wants) happen within our pools, the user will get matched by the same pool. If not, a market maker will take care of executing the trade at the desired price.

![](<.gitbook/assets/Diagrama de Secuencias.png>)

## Integrations

🤑Market Making (MM) on our protocol🤑

{% content-ref url="guides/swaps/" %}
[swaps](guides/swaps/)
{% endcontent-ref %}

🤝Integrate our DCA on your protocol🤝

{% content-ref url="guides/position-management/" %}
[position-management](guides/position-management/)
{% endcontent-ref %}





### &#x20;
