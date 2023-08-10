# Price/Oracle

We've explained how positions works, and that swaps are executed by external market makers. Now, at the moment of the swap, the tokens' prices are determined by an oracle.

## Oracle

In order to determine the price between tokens, we currently use one of three different sources:  Chainlink, API3 and Uniswap v3. When a pair is initialized for the first time, we check if the pair is available through (in this order): Chainlink, API3 or Uniswap. If the pair is not available in any of them, then a position can't be created for that pair.

### Chainlink

Chainlink Data Feeds provide smart contract developers with access to a secure and reliable source of real-world data to power their unique use cases for DeFi and beyond. You can read more about them on their [website](https://chain.link/data-feeds).

### API3 dAPIs

dAPIs are continuously updated streams of off-chain data, such as the latest cryptocurrency, stock, and commodity prices. They are secure, transparent, and cost-efficient data feeds that connect smart contracts directly to first-party data sources. They are powered by Airnode, the serverless first-party oracle solution developed by the API3 DAO. You can read more about them on their [website](https://old-docs.api3.org/dapis/).

### Uniswap V3

Our oracle currently uses Uniswap's V3 pool oracles to determine the correct price. These pool oracles return a time-weighted average price (or TWAP). Our oracle uses a time period that can be between 1 and 20 min.

As a consequence of this approach, only token pairs that have a pool (with at least some liquidity) in Uniswap V3 can be created on Mean Finance.

{% hint style="warning" %}
It is important to mention that this approach works as long as the price in Uniswap V3 pools is reliable.&#x20;

We recommend that users create positions in token pairs that have a decent amount of liquidity on a Uniswap V3 pool.
{% endhint %}







