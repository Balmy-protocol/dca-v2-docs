# Fetch all pairs

### Execution

You can execute this query on our Optimism subgraph, [here](https://thegraph.com/hosted-service/subgraph/mean-finance/dca-v2-optimism?query=Get%20all%20pairs).

### Query

Get all pairs with their token names, block number and the timestamp they were created at.

```graphql
{
  pairs {
    tokenA {
      name
    }
    tokenB {
      name
    }
    createdAtBlock
    createdAtTimestamp
  }
}
```

### Response Type

```graphql
{
  data {
    pairs: [
      {
        tokenA {
          name: string
        }
        tokenB {
          name: string
        }
        createdAtBlock: BigInt
        createdAtTimestamp: BigInt
      }
    ]
  }
}
```
