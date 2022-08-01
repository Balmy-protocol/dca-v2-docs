# Modifying a Position

There are a few different ways you can modify your existing position. You can [add more funds](modifying-a-position.md#modify-the-rate), you can [decrease the amount of funds](modifying-a-position.md#modify-the-amount-of-swaps), and you can also [re-purpose](modifying-a-position.md#modify-both-the-rate-and-the-amount-of-swaps) the funds already in your position.

### Adding funds

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol';
import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';

contract MyContract {

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Takes the unswapped balance, adds the new deposited funds and modifies the position so that
    /// it is executed in _newSwaps swaps
    /// @param _positionId The position's id
    /// @param _amount Amount of funds to add to the position
    /// @param _newSwaps The new amount of swaps
    function increasePosition(
        uint256 _positionId,
        uint256 _amount,
        uint32 _newSwaps
    ) external {        
        // We need to increase the allowance for the hub before calling it
        IERC20Metadata _from = hub.userPosition(_positionId).from;
        _from.approve(address(hub), _amount);
        hub.increasePosition(_positionId, _amount, _newSwaps);
    }
}
```

### Decreasing funds

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';

contract MyContract {

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Withdraws the specified amount from the unswapped balance and modifies the position so that
    /// it is executed in _newSwaps swaps
    /// @param _positionId The position's id
    /// @param _amount Amount of funds to withdraw from the position
    /// @param _newSwaps The new amount of swaps
    /// @param _recipient The address to send tokens to
    function reducePosition(
        uint256 _positionId,
        uint256 _amount,
        uint32 _newSwaps,
        address _recipient
    ) external {
        hub.reducePosition(_positionId, _amount, _newSwaps, _recipient);
    }
}
```

### Re-purposing funds

This action can be used to take the current funds and re-purpose them. So for example, if there are still 100 DAI left to swap, you can set the amount of swaps to `5`, therefore creating a position with a 20 DAI rate.

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAHub.sol';

contract MyContract {

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Takes the unswapped balance, and modifies the position so that it is executed in _newSwaps swaps
    /// @param _positionId The position's id
    /// @param _newSwaps The new amount of swaps
    function repurposePosition(
        uint256 _positionId,
        uint32 _newSwaps
    ) external {        
        hub.increasePosition(_positionId, 0, _newSwaps);
    }
}
```

### Helper Library

Sometimes, it's hard understand how much is needed in order to configure a certain position, so that it has a specific amount of swaps and rate. That's why we've built a library that does that math for you.

{% hint style="info" %}
Take into account that you might still need to increase the allowance for the hub if you end up having to add more funds to your position.
{% endhint %}

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@mean-finance/dca-v2-periphery/libraries/ModifyPositionWithRate.sol';

contract MyContract {

    using ModifyPositionWithRate for IDCAHub;

    IDCAHub public immutable hub;
    
    constructor(IDCAHub _hub) {
        hub = _hub;
    }
    
    /// @notice Modifies the rate of a position. Could request more funds or return deposited funds
    /// depending on whether the new rate is greater than the previous one.
    /// @param _positionId The position's id
    /// @param _newRate The new rate to set
    function modifyRate(
        uint256 _positionId,
        uint120 _newRate
    ) external {
        hub.modifyRate(_positionId, _newRate);
    }

    /// @notice Modifies the amount of swaps of a position. Could request more funds or return
    /// deposited funds depending on whether the new amount of swaps is greater than the swaps left.
    /// @param _positionId The position's id
    /// @param _newSwaps The new amount of swaps
    function modifySwaps(
        uint256 _positionId,
        uint32 _newSwaps
    ) external {
        hub.modifySwaps(_positionId, _newSwaps);
    }

    /// @notice Modifies both the rate and amount of swaps of a position. Could request more funds or return
    /// deposited funds depending on whether the new parameters require more or less than the current unswapped funds.
    /// @param _positionId The position's id
    /// @param _newRate The new rate to set
    /// @param _newSwaps The new amount of swaps
    function modifyRateAndSwaps(
        uint256 _positionId,
        uint120 _newRate,
        uint32 _newSwaps
    ) external {
        hub.modifyRateAndSwaps(_positionId, _newRate, _newSwaps);
    }
}
```

