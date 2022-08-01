# Fees

Fees in Mean Finance are quite simple. We can divide them into two categories: gas fees and protocol fees.

## Gas fees

Users only need to pay gas fees when they interact with their positions. This includes:

* Creating their position
* Modifying their position
* Withdrawing their swapped balance
* Terminating their position
* Giving or revoking permissions

This means that users **won't** need to pay any gas for swaps. This network fee will be payed by [swappers](swappers.md#swappers).

## Protocol fees

When users interact with their positions, no protocol fee is charged. Protocol fees are collected only on swaps. When a swap is executed, a protocol fee is collected from all users involved in that swap. As of now, that fee is 0.6%.

Let's try with an example using John's position:

* Rate: 5 DAI
* Swap interval: daily
* Amount of swaps: 365

Let's assume that 4 swaps were executed, where 5 DAI = 1 ETH for all swaps. Now, John's swapped balance at the end of the forth swap would be 4 ETH. But, taking into account the protocol fee, it would actually be `4 - 0.6% = 3.976`

It is important to mention that if John were to terminate the position at this point, he would get back:

* The DAI what wasn't swapped: 361 \* 5 = 1805 DAI&#x20;
* The ETH that was swapped: 3.976 ETH

See that no fee was charged from the balance that remained un-swapped.
