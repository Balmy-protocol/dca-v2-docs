# Terminating a Position

When you are done with your position, you can _terminate_ it. This action will send the unswapped and swapped balance to the specified recipients, and then it will destroy the position.

{% hint style="danger" %}
Please take into account that this action is irreversible. Once you terminate your position, you'll need to create a new one.
{% endhint %}

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';

contract MyContract {

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Terminates the position and sends all unswapped and swapped balance to the specified recipients    
    /// @param _positionId The position's id
    /// @param _recipientUnswapped The address to withdraw unswapped tokens to
    /// @param _recipientSwapped The address to withdraw swapped tokens to
    /// @return _unswapped The unswapped balance sent to `_recipientUnswapped`
    /// @return _swapped The swapped balance sent to `_recipientSwapped`
    function terminate(
        uint256 _positionId,
        address _recipientUnswapped,
        address _recipientSwapped
    ) external returns (uint256 _unswapped, uint256 _swapped) {
        (_unswapped, _swapped) = hub.terminate(_positionId, _recipientUnswapped, _recipientSwapped);
    }
}
```
