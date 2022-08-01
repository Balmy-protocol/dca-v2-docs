# Withdrawing from a Position

Once a swap is executed for your position, it will contain "swapped balance". This balance can be withdrawn at any point. Now, it is important to remember that you can withdraw this balance from one of your positions, or many at the same time.

### Withdrawing from one position

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';

contract MyContract {

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Withdraws all swapped tokens from a position to a recipient
    /// @param _positionId The position's id
    /// @param _recipient The address to withdraw swapped tokens to
    /// @return _swapped How much was withdrawn
    function withdrawSwapped(uint256 _positionId, address _recipient) external returns (uint256 _swapped) {
        _swapped = hub.withdrawSwapped(_positionId, _recipient);
    }
}
```

### Withdrawing from many positions

If you'd like to withdraw all swapped tokens from many of your positions, you could technically execute `withdrawSwapped` for each of them. However, since this can be expensive, we also offer `withdrawSwappedMany`. This method will allow you to withdraw more than one position in the same transaction.

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';

contract MyContract {

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Withdraws all swapped tokens from multiple positions
    /// @param _positions A list positions, grouped by `to` token
    /// @param _recipient The address to withdraw swapped tokens to
    /// @return _withdrawn How much was withdrawn for each token
    function withdrawSwappedMany(IDCAHub.PositionSet[] calldata _positions, address _recipient) external returns (uint256[] memory _withdrawn) {
        _withdrawn = hub.withdrawSwappedMany(_positions, _recipient);
    }
}
```
