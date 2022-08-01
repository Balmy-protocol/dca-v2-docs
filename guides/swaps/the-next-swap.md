# Next Swap

Unlike AMMs, in Mean Finance swappers cannot specify how much they'd like to swap. These amounts are determined by the positions created for the different pairs. As a result, the first step towards executing a swap would be to understand which tokens need to be provided, and how big is the reward.

## Time Until Next Swap

As we explained before, all positions have a swap interval that determines when they can be swapped. So it could happen that, at some point, a pair has no swaps to execute. If that is the case, trying to execute `swap` for only that pair would end up reverting. That's why you should check when the next swap will be available.

To do so, you can call `secondsUntilNextSwap` on the [Companion](../../architecture/periphery/companion.md). This functions takes a list of pairs, and returns the amount of seconds until the next swap is available for each of those pairs.

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@mean-finance/dca-v2-periphery/interfaces/IDCAHubCompanion.sol';

contract MyContract {

    IDCAHubCompanion public immutable companion;

    constructor(IDCAHubCompanion _companion) {
        companion = _companion;
    }

    /// @notice Returns how many seconds left until the next swap is available for a list of pairs
    /// @dev Tokens in pairs may be passed in either tokenA/tokenB or tokenB/tokenA order
    /// @param _pairs Pairs to check
    /// @return The amount of seconds until next swap for each of the pairs
    function secondsUntilNextSwap(Pair[] calldata _pairs) external view returns (uint256[] memory) {
        return companion.secondsUntilNextSwap(_pairs);
    }  
}
```

## Next Swap's Information

When `secondsUntilNextSwap` returns 0 for a pair, it means that you can execute a swap right now. If that's the case, you can call `getNextSwapInfo` to get all information regarding this swap, and determine if you want to execute it.&#x20;

{% hint style="info" %}
If`secondsUntilNextSwap`is not 0, you can still call`getNextSwapInfo`, but the result will be zero-ed and meaningless.&#x20;
{% endhint %}

### The Parameters

Calling `getNextSwapInfo` or `swap` is not as straight forward as we'd like. This is mainly because Solidity has restrictions when it comes to non-storage mappings. So, in order to lower the gas costs, we expect the parameters to be already organized in a particular way.

```solidity
function getNextSwapInfo(
    address[] calldata tokens, 
    IDCAHub.PairIndexes[] calldata pairs
) external view returns (IDCAHub.SwapInfo memory _swapInformation);

/// @notice A pair of tokens, represented by their indexes in an array
struct PairIndexes {
    // The index of the token A
    uint8 indexTokenA;
    // The index of the token B
    uint8 indexTokenB;
}
```

If you look at the function's definition, you'll see that it takes two different parameters: `tokens` and `pairs`. The idea is the following, let's assume that you'd like to understand how a swap with the following pairs would look like:

* WBTC/DAI
* WBTC/USDC
* YFI/WETH

The fist thing to do, would be to build a list of all the tokens involved, and sort them by address. So, since:

* DAI = `0x6b175474e89094c44da98b954eedeac495271d0f`
* WETH = `0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2`
* USDC = `0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48`
* WBTC = `0x2260fac5e5542a773aa44fbcfedf7c193bc2c599`
* YFI = `0x0bc529c00C6401aEF6D220BE8C6Ea1667F6Ad93e`

The list would look like this:

```solidity
address[] memory _tokens = [
    0x0bc529c00C6401aEF6D220BE8C6Ea1667F6Ad93e, // YFI
    0x2260fac5e5542a773aa44fbcfedf7c193bc2c599, // WBTC
    0x6b175474e89094c44da98b954eedeac495271d0f, // DAI
    0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48, // USDC
    0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2 // WETH
];
```

Once the list is built, we need to associate the pairs by their indexes on the list. Take into account that this list also needs to be sorted. Based on the example, it would look like this:

```solidity
IDCAHub.PairIndexes[] memory _pairs = [
    { indexTokenA: 0, indexTokenB: 4 }, // YFI/WETH
    { indexTokenA: 1, indexTokenB: 2 }, // WBTC/DAI
    { indexTokenA: 1, indexTokenB: 3 } // WBTC/USDC
]
```

Once you have these two parameters built, you can call the Hub.

#### A little help from the Periphery

We know this is fairly complicated, so we've tried to make it a little bit simpler. We've built a [library](../../architecture/periphery/libraries.md#inputbuilding) that takes a list of pairs `{ address tokenA; address tokenB }` and builds these parameters for you.&#x20;

We've also added a function on the [Companion](../../architecture/periphery/companion.md) that takes this easier format, generates the parameters and calls the hub for you:

```solidity
  /// @notice Takes a list of pairs and returns how it would look like to execute a swap for all of them
  /// @param _pairs The pairs to be involved in the swap
  /// @return How executing a swap for all the given pairs would look like
  function getNextSwapInfo(Pair[] calldata _pairs) external view returns (IDCAHub.SwapInfo memory);
```

{% hint style="warning" %}
Please notice that while this new call is perfect for off-chain queries, the generation of the parameters can be expensive for on-chain purposes. So please consider both the Hub's and the Companion's versions when integrating.
{% endhint %}

### The Result

When calling `getNextSwapInfo`, you will get a struct called `SwapInfo` as a result. It will contain all information necessary about the swap, from the ratio between tokens in the same pair, to how much the swapper needs to provide, and how much they'll get as a reward. The struct looks like this:&#x20;

```cpp
/// @notice Information about a swap
struct SwapInfo {
    // The tokens involved in the swap
    TokenInSwap[] tokens;
    // The pairs involved in the swap
    PairInSwap[] pairs;
}

/// @notice Information about a token's role in a swap
struct TokenInSwap {
    // The token's address
    address token;
    // How much will be given of this token as a reward
    uint256 reward;
    // How much of this token needs to be provided by swapper
    uint256 toProvide;
    // How much of this token will be paid to the platform
    uint256 platformFee;
}

/// @notice Information about a pair in a swap
struct PairInSwap {
    // The address of one of the tokens
    address tokenA;
    // The address of the other token
    address tokenB;
    // How much is 1 unit of token A when converted to B
    uint256 ratioAToB;
    // How much is 1 unit of token B when converted to A
    uint256 ratioBToA;
    // The swap intervals involved in the swap, represented as a byte
    bytes1 intervalsInSwap;
}
```
