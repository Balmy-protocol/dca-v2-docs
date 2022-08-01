# Creating a Position

Once you have the hub's address, you can now create your position. Take into account that:

* `from` and `to` need to be different tokens
* `amount` must be a positive number
* `amountOfSwaps` must be a positive number
* `swapInterval` must be one of the allowed swap intervals. There is a list of pre-defined supported time intervals, but the hub has a configuration that determines which are allowed and which are not. The supported time intervals are:
  * 1 minute
  * 5 minutes
  * 15 minutes
  * 30 minutes;&#x20;
  * 1 hour
  * 4 hours
  * 1 day
  * 1 week

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@openzeppelin/contracts/token/ERC20/IERC20.sol';
import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';

contract MyContract {

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Creates a new position
    /// @param _from The address of the "from" token
    /// @param _to The address of the "to" token
    /// @param _amount How many "from" tokens will be swapped in total
    /// @param _amountOfSwaps How many swaps to execute for this position
    /// @param _swapInterval How frequently the position's swaps should be executed
    /// @param _owner The address of the owner of the position being created
    /// @return _positionId The id of the created position
    function deposit(
        address _from,
        address _to,
        uint256 _amount,
        uint32 _amountOfSwaps,
        uint32 _swapInterval,
        address _owner,
        IDCAPermissionManager.PermissionSet[] calldata _permissions
    ) external returns (uint256 _positionId) {
        // We need to increase the allowance for the hub before calling deposit
        IERC20(_from).approve(address(hub), _amount);
        _positionId = hub.deposit(_from, _to, _amount, _amountOfSwaps, _swapInterval, _owner, _permissions);
    }
}
```
