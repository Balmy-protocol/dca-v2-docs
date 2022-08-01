# Companion

## Introduction

As we explained before, the [Hub](../core/hub.md) is the most important contract in the protocol. However, mostly due to contract size restrictions, the user experience is not the best. That's where the Companion comes into play.

The Companion is a smart contract that coexists and interacts with the Hub in order to make the user experience better and add some missing features.

{% hint style="info" %}
In order for the Companion to be able to interact with your positions, you will need to explicitly give it [permissions](../../concepts/positions/nft-permissions.md#additional-permissions) to do so.&#x20;
{% endhint %}

## Responsibilities

### Network Tokens

Our protocol works natively with ERC20 tokens. However, some users might want to deposit or receive network tokens (like ETH, MATIC or BNB) in their positions. That's where the Companion comes into play.&#x20;

It will allow users to seamlessly use their network tokens with their positions, while the Companion wraps and unwraps the tokens under the hood. You can read more about this feature in the [position management section](../../guides/position-management/using-eth-matic-bnb.md).

### Multicall

In order to make UX better for our users, we've implemented [Open Zeppelin's Multicall](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.4.0/contracts/utils/Multicall.sol) as part of our Companion. By doing this, we can allow our users to execute many different operations in one transaction. For example:

* Withdraw from one position and create another with the withdrawn balance
* Increase the funds for many of your positions at once
* And more!

![The Hub Companion](<../../.gitbook/assets/Companion (1).png>)

