# Oracles

## Introduction

Oracles are a crucial part of the protocol, since they provide information about token prices at the moment of swaps. This means that Mean Finance can only support a pair if we have an on-chain oracle that supports it. So, in order to support as many pairs as possible, we've integrated [two different sources](../../concepts/price-oracle.md), Chainlink and Uniswap v3.

## Oracles

### Aggregator

In order to use both, we've created an "Oracle Aggregator". This is a contract that knows our other two oracles, and acts as a proxy when asked for quotes. The proxy logic is quite simple. We will use Chainlink if the Chainlink oracle supports the given pair. If it doesn't, we will fallback to Uniswap v3.&#x20;

The reason behind this is that the reliability of Uniswap v3 oracles depends on the amount of liquidity and volume of each pair. Since we can't control any of this, we believe if would be safer to use Chainlink when available and try to avoid inaccurate quotes.

![The Oracle Aggregator](<../../.gitbook/assets/Oracle Aggregator.png>)

### Chainlink Oracle

Our Chainlink oracle is quite simple. It simply asks [Chainlink's feed registry](https://docs.chain.link/docs/feed-registry/) for quotes, which redirects the query to the appropriate price feed.

![The Chainlink Oracle](<../../.gitbook/assets/Chainlink Oracle.png>)

### Uniswap v3 Oracle

Our Uniswap oracle is a little more complex. Since the same pair _tokenA/tokenB_ can have multiple pools, we need to query them all. After we query them all, we calculate the average price, weighted by liquidity.

![The Uniswap Oracle](<../../.gitbook/assets/Uniswap Oracle.png>)

