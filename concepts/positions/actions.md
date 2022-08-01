# Actions

As we explained before, users can create a position at any time. Once their position is created, there are some actions that they can perform on them.

### Modify a position

Users can choose to modify their positions in a few different ways. They can:

* Add more funds to their position
  * For example, increasing the rate from 5 DAI to 10 DAI
* Remove funds from their position
  * For example, reducing the rate from 5 DAI to 3 DAI
* Reorganize the available funds
  * For example, going from a 5 DAI rate in 8 swaps, to a 4 DAI rate in 10 swaps

Now, as you have probably noticed, only a position's "rate" and "amount of swaps" properties can be modified. The "swap interval", "from" and "to" properties cannot be changed.

### Withdraw swapped balance

When a swap is executed on a position, the remaining balance decreases and their swapped balance increases. In John's example, the DAI balance would decrease, while the WETH balance would increase. John can choose to withdraw their swapped balance at any point.

### Terminate a position

Users can also choose to terminate their position at any point. When this happens, the position is destroyed, and all remaining and swapped balance is returned to the user.
