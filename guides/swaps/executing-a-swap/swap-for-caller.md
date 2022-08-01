# Swap for Caller

As we just explained, [Flash Swaps](regular-swaps.md) can be very difficult to execute. So we've added an easier way to execute swaps through the [Swapper](../../../architecture/periphery/swapper.md), which we call "Swap for Caller". In this case, the swap acts more similar to most AMMs, where there are two steps:

1. Approve the Swapper to use the tokens
2. Execute the swap, where the tokens will be taken from our wallet, and we will receive the other tokens in exchange

```solidity
pragma solidity >=0.8.7 <0.9.0;

import '@openzeppelin/contracts/token/ERC20/IERC20.sol';
import '@mean-finance/dca-v2-periphery/interfaces/IDCAHubSwapper.sol';

contract MyContract {

    IDCAHubSwapper public immutable swapper;
    
    constructor(IDCAHubSwapper _swapper) {
        swapper = _swapper;
    }

    /// @notice Executes a swap for the caller, by sending them the reward, and taking from them the needed tokens
    /// @param _tokens The tokens involved in the swap
    /// @param _pairsToSwap The pairs to swap
    /// @param _minimumOutput The minimum amount of tokens to receive as part of the swap
    /// @param _maximumInput The maximum amount of tokens to provide as part of the swap
    /// @param _recipient Address that will receive all the tokens from the swap
    /// @param _deadline Deadline when the swap becomes invalid
    /// @return The information about the executed swap
    function swapForCaller(
        address[] calldata _tokens,
        IDCAHub.PairIndexes[] calldata _pairsToSwap,
        uint256[] calldata _minimumOutput,
        uint256[] calldata _maximumInput,
        address _recipient,
        uint256 _deadline
    ) external payable returns (IDCAHub.SwapInfo memory) {
    
      // Step 1, set allowance so that the swapper can take the tokens
      for (uint i; i < _tokens.length; i++) {
        IERC20(_tokens[i]).approve(address(swapper), _maximumInput[i]);
      }

      // Step 2, execute the swap
      return swapper.swapForCaller(_tokens, _pairsToSwap, _minimumOutput, _maximumInput, _recipient, _deadline);
    }
}
```
