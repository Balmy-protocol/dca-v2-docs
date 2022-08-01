# NFT Descriptor

As we explained [before](../../concepts/positions/nft-permissions.md#representation), the metadata + image of the position NFT is fully on-chain. There is a contract that, given a position id, returns a [base64](https://en.wikipedia.org/wiki/Base64) string with the ERC721 metadata JSON. This JSON also contains a SVG (also as a base64 string) that displays the information about the position, such as:

* Position id
* Position rate
* Swaps executed
* Swaps left
* Average swap price

We call the contract responsible for this work "NFT Descriptor". Since all of this work is performed on-chain, there is no need for external servers or IPFS.&#x20;

{% hint style="info" %}
This on-chain SVG approach approach is based on Uniswap's v3 NFT positions.
{% endhint %}
