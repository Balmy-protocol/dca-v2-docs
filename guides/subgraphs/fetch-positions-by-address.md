# Fetch positions by address

### Execution

You can execute this query on our Optimism subgraph, [here](https://thegraph.com/hosted-service/subgraph/mean-finance/dca-v2-optimism?query=Get%20all%20positions%20of%20user).

### Query

Get all positions from a user by wallet address. This query will return information like:

* Name of the token used to buy
* Name token to be bought
* Total deposited into the position (expressed in `from` token with applied decimals)
* Total withdrawn from the position (expressed in `to` token with applied decimals)
* Current will hold the current state of the position.
  * Rate with which the position will swap
  * Remaining swaps of the position
* Swap interval
  * Interval (expressed in `seconds`)
* Status (`ACTIVE`, or `TERMINATED`)
* Block at which the position was created
* Timestamp of the block at which the position was created

```graphql
{
  positions (where: { user: "{{ADDRESS_OF_USER}}" }) {
    user
    from {
      name
    }
    to {
      name
    }
    totalDeposits
    totalWithdrawn
    current {
      rate
      remainingSwaps
    }
    swapInterval {
      interval
    }
    status
    createdAtBlock
    createdAtTimestamp
  }
}

```

### Response

```graphql
{
  data {
    positions: [
      {
        user: Bytes
        from {
          name: string
        }
        to {
          name: string
        }
        totalDeposits: BigInt
        totalWithdrawn: BigInt
        current {
          rate: BigInt
          remainingSwaps: BigInt
        }
        swapInterval {
          interval: BigInt
        }
        status: "ACTIVE" or "TERMINATED"
        createdAtBlock: BigInt
        createdAtTimestamp: BigInt
      }
    ]
  }
}
```
