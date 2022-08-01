# Fetch all tokens

### Execution

You can execute this query on our Optimism subgraph, [here](https://thegraph.com/hosted-service/subgraph/mean-finance/dca-v2-optimism?query=Get%20all%20tokens).

### Query

Get all tokens present at least in one of our listed pairs with their ids (also their addresses), token names, decimals and symbols.

```graphql
{
  tokens {
    id
    name
    symbol
    decimals
  }
}
```

### Response

```graphql
{
  data {
    tokens: [
      {
        id: ID
        name: string
        symbol: string
        decimals: Int
      }
    ]
  }
}
```
