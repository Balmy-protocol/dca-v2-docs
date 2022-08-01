# Positions

Mean Finance allows users to exchange ERC20 tokens for others ERC20 tokens. In order to do so, users will need to create a Mean Finance position with the following properties:

* **From**: ERC20 token to sell
* **To**: ERC20 token to buy
* **Rate**: amount of token _"from"_  to swap for their _"to"_ equivalent at the time of the swap
* **Amount of swaps**: how many swaps to execute
* **Swap interval**: daily, weekly, etc (check the [section below](./#swap-intervals) for more information)

Let's go over an example to try to make things clearer. Let's say John has some DAI and he wants to buy WETH. He doesn't know how to time the market, so he wants to create a Mean Finance position. If he wanted to buy 5 DAI worth of WETH daily, for a year, his position would look like this:

* **From**: DAI
* **To**: WETH
* **Rate**: 5 DAI
* **Amount of swaps**: 365
* **Swap interval**: daily

Now, when creating the position, John will need to deposit all necessary funds from the start. In his case, it would be `5 * 365 = 1825 DAI` in total. On the other side, he won't need to deposit anything else or pay any gas fees when the swaps are executed every day. To learn more about fees, check the [fees section](../fees.md).

