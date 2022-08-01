# Using ETH/MATIC/BNB

One of the main limitations around the Hub is that it only supports ERC20s. That's why we added support for protocol tokens (ETH, MATIC, BNB, etc) on the [Companion](../../architecture/periphery/companion.md). The functions are almost exactly the same as the Hub's, but they convert from protocol tokens to their wrapped version, and vice versa.

```solidity
pragma solidity >=0.8.0 <0.9.0;

import '@openzeppelin/contracts/token/ERC20/IERC20.sol';
import '@mean-finance/dca-v2-periphery/interfaces/IDCAHubCompanion.sol';

contract MyContract {

    address public constant PROTOCOL_TOKEN = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;
    IDCAHubCompanion public immutable companion;

    constructor(IDCAHubCompanion _companion) {
        companion = _companion;
    }

    /// @notice Creates a new position by converting the protocol's base token to its wrapped version    
    /// @param _to The address of the "to" token
    /// @param _amount How many "from" tokens will be swapped in total
    /// @param _amountOfSwaps How many swaps to execute for this position
    /// @param _swapInterval How frequently the position's swaps should be executed
    /// @param _owner The address of the owner of the position being created
    /// @param _permissions Extra permissions to add to the position. Can be empty
    /// @return The id of the created position
    function depositUsingProtocolTokenAsFrom(
        address _to,
        uint256 _amount,
        uint32 _amountOfSwaps,
        uint32 _swapInterval,
        address _owner,
        IDCAPermissionManager.PermissionSet[] calldata _permissions
    ) external payable returns (uint256) {
        return companion.depositUsingProtocolToken(PROTOCOL_TOKEN, _to, _amount, _amountOfSwaps, _swapInterval, _owner, _permissions);
    }
    

    /// @notice Creates a new position where the "to" token will be the protocol token    
    /// @param _from The address of the "from" token
    /// @param _amount How many "from" tokens will be swapped in total
    /// @param _amountOfSwaps How many swaps to execute for this position
    /// @param _swapInterval How frequently the position's swaps should be executed
    /// @param _owner The address of the owner of the position being created
    /// @param _permissions Extra permissions to add to the position. Can be empty
    /// @return The id of the created position
    function depositUsingProtocolTokenAsTo(
        address _from,
        uint256 _amount,
        uint32 _amountOfSwaps,
        uint32 _swapInterval,
        address _owner,
        IDCAPermissionManager.PermissionSet[] calldata _permissions
    ) external returns (uint256) {
         // We need to increase the allowance for the companion before calling it        
        IERC20(_from).approve(address(companion), _amount);
        return companion.depositUsingProtocolToken(_from, PROTOCOL_TOKEN, _amount, _amountOfSwaps, _swapInterval, _owner, _permissions);
    }

    /// @notice Withdraws all swapped tokens from a position to a recipient
    /// @param _positionId The position's id
    /// @param _recipient The address to withdraw swapped tokens to
    /// @return _swapped How much was withdrawn
    function withdrawSwappedUsingProtocolToken(uint256 _positionId, address payable _recipient) external returns (uint256 _swapped) {
        _swapped = companion.withdrawSwappedUsingProtocolToken(_positionId, _recipient);
    }

    /// @notice Withdraws all swapped tokens from multiple positions
    /// @param _positionIds A list positions whose 'to' token is the wToken
    /// @param _recipient The address to withdraw swapped tokens to
    /// @return _swapped How much was withdrawn in total
    function withdrawSwappedManyUsingProtocolToken(uint256[] calldata _positionIds, address payable _recipient)
        external
        returns (uint256 _swapped) {
            _swapped = companion.withdrawSwappedManyUsingProtocolToken(_positionIds, _recipient);
        }

    /// @notice Takes the unswapped balance, adds the new deposited funds and modifies the position so that
    /// it is executed in _newSwaps swaps
    /// @param _positionId The position's id
    /// @param _amount Amount of funds to add to the position
    /// @param _newSwaps The new amount of swaps
    function increasePositionUsingProtocolToken(
        uint256 _positionId,
        uint256 _amount,
        uint32 _newSwaps
    ) external payable {
        companion.increasePositionUsingProtocolToken(_positionId, _amount, _newSwaps);
    }

    /// @notice Withdraws the specified amount from the unswapped balance and modifies the position so that
    /// it is executed in _newSwaps swaps
    /// @param _positionId The position's id
    /// @param _amount Amount of funds to withdraw from the position
    /// @param _newSwaps The new amount of swaps
    /// @param _recipient The address to send tokens to
    function reducePositionUsingProtocolToken(
        uint256 _positionId,
        uint256 _amount,
        uint32 _newSwaps,
        address payable _recipient
    ) external {
        companion.reducePositionUsingProtocolToken(_positionId, _amount, _newSwaps, _recipient);
    }

    /// @notice Terminates the position and sends all unswapped and swapped balance to the specified recipients
    /// @param _positionId The position's id
    /// @param _recipientUnswapped The address to withdraw unswapped tokens to
    /// @param _recipientSwapped The address to withdraw swapped tokens to
    /// @return _unswapped The unswapped balance sent to `_recipientUnswapped`
    /// @return _swapped The swapped balance sent to `_recipientSwapped`
    function terminateUsingProtocolTokenAsFrom(
        uint256 _positionId,
        address payable _recipientUnswapped,
        address _recipientSwapped
    ) external returns (uint256 _unswapped, uint256 _swapped) {
        return companion.terminateUsingProtocolTokenAsFrom(_positionId, _recipientUnswapped, _recipientSwapped);
    }

    /// @notice Terminates the position and sends all unswapped and swapped balance to the specified recipients
    /// @param _positionId The position's id
    /// @param _recipientUnswapped The address to withdraw unswapped tokens to
    /// @param _recipientSwapped The address to withdraw swapped tokens to
    /// @return _unswapped The unswapped balance sent to `_recipientUnswapped`
    /// @return _swapped The swapped balance sent to `_recipientSwapped`
    function terminateUsingProtocolTokenAsTo(
        uint256 _positionId,
        address _recipientUnswapped,
        address payable _recipientSwapped
    ) external returns (uint256 _unswapped, uint256 _swapped) {
        return companion.terminateUsingProtocolTokenAsTo(_positionId, _recipientUnswapped, _recipientSwapped);
    }
}
```

