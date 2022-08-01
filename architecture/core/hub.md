# Hub

## Introduction

In v1, we followed a Factory/Pairs approach in terms of architecture. Each pair would be a different contract and there was a factory contract that would deploy new pairs, while also keeping track of the existing ones.

Even though the approach worked, it was really expensive to deploy new contracts for each new pair, specially on Ethereum Mainnet. Another problem with this approach, was that it was very difficult to take advantage of the [Coincidence of Wants](https://en.wikipedia.org/wiki/Coincidence\_of\_wants) between different pairs when swapping.

So, for v2, we came up with "The Hub". The hub is a single contract that handles all pairs, therefore eliminating the need for deploying a new contracts. At the same time, it also makes it easier and cheaper to execute swaps for many pairs in one transaction.

## Responsibilities

Since we unified all pairs in one place, the hub became the most important contract in the protocol. It is responsible for:

* Positions&#x20;
  * Creation / modification / termination
  * Withdraws
* &#x20;Swaps&#x20;
  * Calculating potential swaps
  * Executing swaps
* Config&#x20;
  * Config changes
  * Pause / unpause&#x20;

![The Hub](<../../.gitbook/assets/Hub (1).png>)
