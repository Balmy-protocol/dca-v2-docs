# Permission Management

{% hint style="info" %}
Before diving into the technical aspect of permissions, we highly recommend that you first read the [appropriate section](../concepts/positions/nft-permissions.md).
{% endhint %}

In Mean Finance, the [Permission Manager](../architecture/core/permission-manager.md) is the contract that handles everything related to permissions. It is actually pretty simple to use.

## Checking if address has permissions

We have two different functions for checking permissions, `hasPermission` and `hasPermissions`. The first one is meant to check one permission, while the latter can be used to check many permissions in one call.

```solidity
pragma solidity >=0.8.7 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAPermissionManager.sol';

contract MyContract {

    IDCAPermissionManager public immutable permissionManager;
    
    constructor(IDCAPermissionManager _permissionManager) {
        permissionManager = _permissionManager;
    }    
    
    /// @notice Returns whether the given address has the permission for the given token
    /// @param _id The id of the token to check
    /// @param _address The address of the user to check
    /// @param _permission The permission to check
    /// @return Whether the user has the permission or not
    function hasPermission(
        uint256 _id,
        address _address,
        Permission _permission
    ) external view returns (bool) {
        return permissionManager.hasPermission(_id, _address, _permission);
    }

    /// @notice Returns whether the given address has the permissions for the given token
    /// @param _id The id of the token to check
    /// @param _address The address of the user to check
    /// @param _permissions The permissions to check
    /// @return _hasPermissions Whether the user has each permission or not
    function hasPermissions(
        uint256 _id,
        address _address,
        Permission[] calldata _permissions
    ) external view returns (bool[] memory _hasPermissions) {
        return permissionManager.hasPermissions(_id, _address, _permissions);
    }

}
```

## Setting/Deleting permissions

The Permission Manager has only one function that sets, modifies and deletes permissions, called `modify`. This function can set permissions for batch of operators, overwriting whatever permissions the operators had before. If you are looking to revoke all permissions from an operator, you'll need call `modify` and send an empty list of permissions.

{% hint style="info" %}
Operators that are not sent to `modify` won't see their permissions affected in any way.
{% endhint %}

```solidity
pragma solidity >=0.8.7 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAPermissionManager.sol';

contract MyContract {

    IDCAPermissionManager public immutable permissionManager;
    
    constructor(IDCAPermissionManager _permissionManager) {
        permissionManager = _permissionManager;
    }

    /// @notice Sets new permissions for the given tokens
    /// @dev Operators that are not part of the given permission sets do not see their permissions modified.
    /// In order to remove permissions to an operator, provide an empty list of permissions for them
    /// @param _id The token's id
    /// @param _permissions A list of permission sets
    function modify(uint256 _id, IDCAPermissionManager.PermissionSet[] calldata _permissions) external {
        permissionManager.modify(_id, _permissions);
    }    
}
```

## Setting/Deleting permissions with permit

Finally, we've also added support for setting permissions by using a signature, similarly to [EIP-2612](https://eips.ethereum.org/EIPS/eip-2612).

```solidity
pragma solidity >=0.8.7 <0.9.0;

import '@mean-finance/dca-v2-core/contracts/interfaces/IDCAPermissionManager.sol';

contract MyContract {

    IDCAPermissionManager public immutable permissionManager;
    
    constructor(IDCAPermissionManager _permissionManager) {
        permissionManager = _permissionManager;
    }

    /// @notice Sets permissions via signature
    /// @dev This method works similarly to `modify`, but instead of being executed by the owner, it can be set my signature
    /// @param _permissions The permissions to set
    /// @param _tokenId The token's id
    /// @param _deadline The deadline timestamp by which the call must be mined for the approve to work
    /// @param _v Must produce valid secp256k1 signature from the holder along with `r` and `s`
    /// @param _r Must produce valid secp256k1 signature from the holder along with `v` and `s`
    /// @param _s Must produce valid secp256k1 signature from the holder along with `r` and `v`
    function permissionPermit(
        IDCAPermissionManager.PermissionSet[] calldata _permissions,
        uint256 _tokenId,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external {
        permissionManager.permissionPermit(_permissions, _tokenId, _deadline, _v, _r, _s);
    }
}
```
