# Interacting with Mean Finance

Mean Finance has a few different contracts that you can interact with. To read more about them, and what they do, you can check our [Core](../architecture/core/) and [Periphery](../architecture/periphery/) architecture sections.

## Interfaces

If you'd like to interact with any of our contracts, we highly recommend that you install our npm package and import our interfaces or ABIs. To do so, just run:

**Core:**

`npm i @mean-finance/dca-v2-core`

**Periphery:**

`npm i @mean-finance/dca-v2-periphery`

## Smart Contract Registry

All addresses are the same in our deployed networks. So far, we are deployed in **Optimism**, and **Polygon**.

| **Contract**       | **Address**                                |
| ------------------ | ------------------------------------------ |
| Uniswap Oracle     | 0x14AF365e0825B835C60867C985724e1DF11449ad |
| Chainlink Oracle   | 0x86E8cB7Cd38F7dE6Ef7fb62A5D7cCEe350C40310 |
| Oracle Aggregator  | 0x4b0C54236B86f41C5e5A5dc5d020f832692ff06d |
| NFT Descriptor     | 0xF3F361C1A84969dB21eB5Ed278BC987B7540923C |
| Permission Manager | 0x6f54391fE0386D506b51d69Deeb8b04E0544E088 |
| DCA Hub            | 0x059d306A25c4cE8D7437D25743a8B94520536BD5 |
| DCA Hub Companion  | 0xa3DB2c0D23720e8CDA0f4d80A53B94d20d02b061 |
| DCA Hub Swapper    | 0xBF5C27CC7c1c91E924fA7B4df7928371e8f713a6 |
| Position Migrator  | 0xCf51244AE89dE8f062ebc963C64bA96C1723e27e |
