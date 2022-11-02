# Fuel Book chapter 6:对于开发者

## 6.1 [测试网节点](https://fuellabs.github.io/fuel-docs/master/for-developers/testnet-node.html#testnet-node)

- [**将 Rust SDK 连接到 Fuel 节点**](https://fuellabs.github.io/fuel-docs/master/for-developers/testnet-node.html#connect-rust-sdk-to-fuel-node)

[在此处](https://fuellabs.github.io/fuels-rs/v0.22.0/providers/external-node.html)查看这些文档以将您的 SDK 连接到 Fuels 测试网。

- [将合约部署到测试网](https://fuellabs.github.io/fuel-docs/master/for-developers/testnet-node.html#deploy-a-contract-to-testnet)

要将合约部署到测试网，[您需要使用创建钱包`forc-wallet`](https://github.com/FuelLabs/forc-wallet#forc-wallet)，然后运行以下命令： `forc deploy --url https://node-beta-1.fuel.network/graphql --gas-price 1`

查看开发人员快速入门，了解如何将合约部署到测试网的工作示例。

## 6.2 Sway**[语言](https://fuellabs.github.io/fuel-docs/master/for-developers/sway.html#sway-language-)**

开始使用 Sway 的资源：
• [Sway 官方文档](https://fuellabs.github.io/sway/latest/)
• [Sway 示例存储库](https://github.com/FuelLabs/sway/tree/master/examples)
• [研讨会：使用 Sway 开发智能合约](https://www.youtube.com/watch?v=SctJwG2LPK8)
**[需要帮忙？](https://fuellabs.github.io/fuel-docs/master/for-developers/sway.html#need-help)**
前往[Fuel discord](https://discord.com/invite/fuelnetwork)寻求团队的帮助。

## 6.3 [Typescript SDK](https://fuellabs.github.io/fuel-docs/master/for-developers/ts-sdk.html#typescript-sdk)

Fuel TS SDK 是一个用于在燃料网络上构建 dapp 的工具包。您可以使用 SDK 执行脚本、与合约交互、列出交易、余额等。

要开始使用 TypeScript SDK：

- [开发工具包文档](https://fuellabs.github.io/fuels-ts/)
- [TS 快速入门指南](https://fuellabs.github.io/fuels-ts/QUICKSTART.html)
- [TS SDK 回购](https://github.com/FuelLabs/fuels-ts)

[**需要帮忙？**](https://fuellabs.github.io/fuel-docs/master/for-developers/ts-sdk.html#need-help)

前往[Fuel discord](https://discord.com/invite/fuelnetwork)寻求团队的帮助。

## 6.4 **[Rust** SDK](https://fuellabs.github.io/fuel-docs/master/for-developers/rust-sdk.html#rust-sdk)

用于燃料的 Rust SDK。它可以用于多种用途，包括但不限于：

- 部署和测试 Sway 合约；
- 启动本地燃料网络；
- 使用手工制作的脚本或合约调用来制作和签署交易；
- 为合约方法生成类型安全的 Rust 绑定；此外，fuels-rs 仍在积极开发中。

Rust SDK 入门：

- [Rust SDK 文档](https://fuellabs.github.io/fuels-rs/latest/)
- [Rust SDK 存储库](https://github.com/FuelLabs/fuels-rs)

## [需要帮忙？](https://fuellabs.github.io/fuel-docs/master/for-developers/rust-sdk.html#need-help)

前往[Fuel discord](https://discord.com/invite/fuelnetwork)寻求团队的帮助。

# 6.5 Fuel[索引器](https://fuellabs.github.io/fuel-docs/master/for-developers/indexer.html#fuel-indexer)(Indexer)

Fuel Indexer 是一个独立的二进制文件，可用于索引区块链的各种组件。这些可索引的组件包括燃料网络中的块、交易、收据和状态，允许对高级 dApp 用例的区块链进行高性能只读访问。

Fuel Indexer 可以使用 WASM 模块对事件进行索引，[如 Hello World 示例中所述](https://fuellabs.github.io/fuel-indexer/master/examples/hello-indexer.html)。

燃料索引器入门：

- [索引器文档](https://fuellabs.github.io/fuel-indexer/master/the-fuel-indexer.html)

## [需要帮忙？](https://fuellabs.github.io/fuel-docs/master/for-developers/indexer.html#need-help)

前往[Fuel discord](https://discord.com/invite/fuelnetwork)寻求团队的帮助。

## 6.6 钱包和水龙头

6.6.1 Fuel[**钱包**](https://fuellabs.github.io/fuel-docs/master/for-developers/wallet-faucet.html#fuel-wallet)

您可以通过运行以下命令将合约部署到本地燃料实例，而无需使用钱包签名：

`forc deploy --unsigned`

要将合约部署到测试网，您需要一个 Fuel 钱包。您可以为您的钱包初始化一个钱包和一个帐户以签署交易。

[查看文档](https://github.com/FuelLabs/forc-wallet#forc-wallet)以获取初始化钱包和创建帐户的分步指南。

## 6.6.2 Fuel[水龙头](https://fuellabs.github.io/fuel-docs/master/for-developers/wallet-faucet.html#fuel-faucet)

测试网节点的资产可以从 [https://faucet-beta-1.fuel.network/](https://faucet-beta-1.fuel.network/)的水龙头获得。

## [区块浏览器](https://fuellabs.github.io/fuel-docs/master/for-developers/wallet-faucet.html#block-explorer)

测试网的区块浏览器可以在[这里](https://fuellabs.github.io/block-explorer-v2/)找到。