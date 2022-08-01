# Flash Swaps

In v2, all swaps are flash swaps. Flash Swaps are fairly complex to execute, but at the same time they hold the biggest potential for profit. This is because the user will receive their reward tokens before having to send the required tokens while, simultaneously, the caller can actually borrow all of the hub's liquidity for free. In that sense, this action can be seen both as a flash swap and a flash loan.

Flash Swaps can be divided into three parts:

1. Calling the pair
2. Receiving the reward and borrowed (if any) tokens, and doing something with them
3. Returning the needed and borrowed (if any) tokens

{% hint style="info" %}
The caller in step one can be different from the contract in step 2 and 3, but for the purposes of this example, they will all be the same contract.
{% endhint %}

```solidity
pragma solidity >=0.8.7 <0.9.0;

import '@openzeppelin/contracts/token/ERC20/IERC20.sol';
import '@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol';
import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';
import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHubSwapCallee.sol';

// In order to receive the reward + borrowed tokens in step 2, you need to implement IDCAHubSwapCallee
contract MyContract is IDCAHubSwapCallee {

    using SafeERC20 for IERC20;
    
    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }

    /// @notice Executes a flash swap
    /// @param _tokens The tokens involved in the next swap
    /// @param _pairsToSwap The pairs that you want to swap. Each element of the list points to the index of the token in the _tokens array
    /// @param _borrow How much to borrow of each of the tokens in _tokens. The amount must match the position of the token in the _tokens array
    /// @param _data Bytes to send to the contract during the callback
    /// @return Information about the executed swap    
    function executeSwap(        
        address[] calldata _tokens,
        IDCAHub.PairIndexes[] calldata _pairsToSwap,
        uint256[] calldata _borrow,
        bytes calldata _data
    ) external returns (IDCAHub.SwapInfo memory) {
        // Step 1, calling the hub and executing the swap
        return hub.swap(_tokens, _pairsToSwap, address(this), address(this), _borrow, _data);
    }


    // solhint-disable-next-line func-name-mixedcase
    function DCAHubSwapCall(
        address _sender, // The swap originator, in this case this same contract
        IDCAHub.TokenInSwap[] calldata _tokens, // How much did we get as reward & how much needs to be sent back
        uint256[] calldata _borrowed, // How much was borrowed from the tokens in `_tokens`
        bytes calldata _data // Whatever was sent to `executeSwap`
    ) external {
        // Step 2, do something with the reward and borrowed tokens
        ... 
        
        // Step 3, return the borrowed and needed tokens
        for (uint i; i < _tokens.length; i++) {
            uint256 _amountToReturn = _borrowed[i] + _tokens[i].toProvide;
            if (_amountToReturn > 0) {
                IERC20(_tokens[i].token).safeTransfer(msg.sender, _amountToReturn);
            }            
        }
    }  
}
```
