# Swap with DEX

## The idea

So far, we've explained two different ways of executing swaps: [flash swaps](regular-swaps.md), and [swaps for caller](swap-for-caller.md). In both cases, the caller needs to have or get the tokens needed by the hub, in order to exchange them for the reward tokens. But, what if the user doesn't have the required liquidity, or doesn't know how to get it?

That's why we've added to the [Swapper](../../../architecture/periphery/swapper.md) a way for users to actually execute swaps without having to provide any liquidity. The idea is quite simple: we will execute a flash swap, use the reward to get the needed funds in a DEX, send the needed funds to the Hub and keep the fee to ourselves. In the end, we would have executed a swap and kept the fee, without using any of our funds.

### How it looks like

We can divide this whole process into two:

* **Preparation**: we understand how much is needed for the swap, and get a quote from our DEX
* **Execution**: we send the DEX swap transaction data to the Swapper, who executes everything

![Swap with DEX - Preparation stag](<../../../.gitbook/assets/Swap with DEX  - Preparation.png>)

![Swap with DEX - Execution stage](<../../../.gitbook/assets/Swap with DEX  - Execution.png>)

{% hint style="info" %}
One important detail is that DEXes need to be whitelisted on the Swapper. So please contact us if you find that your favourite DEX is not whitelisted yet.
{% endhint %}

## The code

```solidity
pragma solidity >=0.8.7 <0.9.0;

import '@mean-finance/dca-v2-periphery/interfaces/IDCAHubSwapper.sol';

contract MyContract {

    IDCAHubSwapper public immutable swapper;
    
    constructor(IDCAHubSwapper _swapper) {
        swapper = _swapper;
    }

    /// @notice Executes a swap with the given DEX, and sends all unspent tokens to the given recipient
    /// @param _dex The DEX that will be used in the swap
    /// @param _tokensProxy The spender of the tokens (could be different from the dex)
    /// @param _tokens The tokens involved in the swap
    /// @param _pairsToSwap The pairs to swap
    /// @param _callsToDex The bytes to send to the DEX to execute swaps
    /// @param _doDexSwapsIncludeTransferToHub Some DEXes support swap & transfer, which would be cheaper in terms of gas
    /// If this feature is used, then the flag should be true
    /// @param _leftoverRecipient Address that will receive all unspent tokens
    /// @param _deadline Deadline when the swap becomes invalid
    /// @return The information about the executed swap
    function swapWithDex(
        address _dex,
        address _tokensProxy,
        address[] calldata _tokens,
        IDCAHub.PairIndexes[] calldata _pairsToSwap,
        bytes[] calldata _callsToDex,
        bool _doDexSwapsIncludeTransferToHub,
        address _leftoverRecipient,
        uint256 _deadline
    ) external returns (IDCAHub.SwapInfo memory) {
        return swapper.swapWithDex(
            _dex,
            _tokensProxy,
            _tokens, 
            _pairsToSwap, 
            _callsToDex, 
            _doDexSwapsIncludeTransferToHub, 
            _leftoverRecipient, 
            _deadline
        );
    }
}
```
