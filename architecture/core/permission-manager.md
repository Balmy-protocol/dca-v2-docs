# Permission Manager

## Introduction

In v1, we quickly realized that there were a lot of use cases that were really hard to fulfill. Specifically, we noticed that users would need to transfer full ownership of their positions to other protocols so that they could then use their positions.

This of course was a problem, so we quickly realized that we needed an ad-hoc permission system, regardless of the ownership of the position. Enter the _Permission Manager_.

The Permission Manager is a contract that handles both [NFT ownership/transfer and additional permissions](../../concepts/positions/nft-permissions.md). Note that ideally, this contract would actually be part of the hub, but we ran out of space.

## Responsibilities

* Handles NFT ownership + transfer
  * Mints & burns can only be executed by the hub, when a position is created/terminated
* Supports permit ([EIP-2612](https://eips.ethereum.org/EIPS/eip-2612)) for NFT approvals&#x20;
* Handles [additional permissions](../../concepts/positions/nft-permissions.md#additional-permissions)
* Supports permit ([EIP-2612](https://eips.ethereum.org/EIPS/eip-2612)) for additional permissions

![The Permission Manager](<../../.gitbook/assets/Permission Manager (1).png>)
