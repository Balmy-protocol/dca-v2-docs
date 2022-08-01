# Fetch all active intervals

### Execution

You can execute this query on our Optimism subgraph, [here](https://thegraph.com/hosted-service/subgraph/mean-finance/dca-v2-optimism?query=Get%20all%20active%20intervals).

### Query

Get all tokens present at least in one of our listed pairs with their ids (also their addresses), token names, decimals and symbols.

```graphql
{
  swapIntervals(where: { active: true }) {
    interval
   	active 
  }
}
```

### Response

```graphql
{
  data {
    swapIntervals: [
      {
        active: boolean
        interval: BigInt
      }
    ]
  }
}
```
