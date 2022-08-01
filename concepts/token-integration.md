# Token Integration

Mean Finance enables users to Dollar Cost Average (DCA) one ERC20 into other ERC20. Now, as we said before, not all ERC20 tokens are supported by our [oracle](price-oracle.md). If a token is not supported by the oracle, then a pair can't be created for it.

But, as it happens, some tokens that might be supported by the oracle provider are in fact not supported by Mean Finance. We have implemented an allow list that controls which tokens can be deposited or swapped.

We want to be as open as possible, but we need to put our users’ safety first. We decided that we wanted to be able to perform some due diligence before adding support for a token, even if that means that new integrations might slow down a little.

Why are we doing this? We want to prevent the use of poisonous tokens, but we’ve also seen that some real and popular tokens can be used to attack protocols. Like:

* Double entry ERC20s like [TUSD](https://blog.openzeppelin.com/compound-tusd-integration-issue-retrospective/) & [SNX](https://twitter.com/BalancerLabs/status/1525277944674930689)
* Tokens with extreme volatility like [LUNA](https://cointelegraph.com/news/defi-protocols-declare-losses-as-attackers-exploit-luna-price-feed-discrepancy)
* Tokens with low liquidity on Uniswap Oracles, like [VUSD](https://twitter.com/raricapital/status/1455569653820973057) & [others](https://medium.com/@hacxyk/we-rescued-4m-from-rari-capital-but-was-it-worth-it-39366d4d1812)

The allowlist allows us to investigate new tokens before adding them to Mean Finance, and prevent scenarios like the linked before.

And if you want Mean to support a new token, just come to the **#new-token-request** channel on our [Discord](https://discord.com/invite/ThfzDdn4pn) and we will try to add support for it as soon as possible.

