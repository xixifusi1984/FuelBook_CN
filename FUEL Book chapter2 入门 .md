# FUEL Book chapter2:入门

# 2.1 **[为什么选择燃料](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#why-fuel)？**

### 2.1.1 [**具有欺诈证明的用户主权**](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#user-sovereignty-with-fraud-proofs)

Fuel 专门设计和构建为可证明欺诈，支持信任最小化的轻客户端。信任最小化的轻客户端和共享数据可用性使信任最小化的桥能够连接到其他模块化执行层（layers），这以前在 L1 之间是不可能实现的。

这在实践中意味着什么：

- 长期流动性准入
- 用户无需运行完整节点即可验证链
- 安全地桥接资产

Fuel 对欺诈证明的优化是使用了 UTXO 模型，这反过来意味着 Fuel 没有全局状态树或帐户树。如果你想将欺诈证明的一般结构应用到使用以太坊等账户模型的链上，成本可能是无限的，因此制作欺诈证明的成本非常高。在欺诈证明的一般构造中，给定一个前状态和一个状态转换，您在本地执行转换并将输出与块生产者产生的后状态进行比较。如果这些不同，则区块生产者的后期状态无效。如果你今天将这种通用的防欺诈模型应用到以太坊，那么有人可以创建一个调用许多不同合约的交易，而这些合约中的每一个都可能有多达 24kb 的字节码。为了在本地重新执行，您需要为与之交互的所有合约提供所有字节码。

[在此处](https://fuellabs.github.io/fuel-docs/master/modular-movement.html)阅读有关信任最小化的轻客户端和主权的更多信息。

### 2.1.2 [卓越的 DevEx](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#superior-devex)

Fuel 的技术相结合，可提供卓越的开发人员体验。我们是这样做的：

### 2.1.3 Sway[和Fuel工具链](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#sway-and-fuel-toolchain)

FuelVM被设计成与工具开发垂直集成。

与从一开始就没有使用语言设计的 EVM 不同，FuelVM 与它的配套语言 Sway 一起构建，确保它具有方便且高效的操作，例如获取 tx 的特定部分。Sway 是一种基于 Rust 的 DSL，专为利用区块链 VM 而创建，无需冗长的样板。Sway 利用 Rust 的安全性并在编译时捕获错误和错误，让开发人员高枕无忧。[在此处](https://fuellabs.github.io/fuel-docs/master/sway-language.html)阅读有关 Sway的更多信息。

Fuel Labs 还开发了 Fuel 工具链：用于启用/协助 Fuel 应用程序开发体验的全套工具。[在此处](https://fuellabs.github.io/fuel-docs/master/fuel-toolchain.html)阅读有关 Fuel 工具链的更多信息。

### 2.1.4 [并行执行](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#parallel-execution)

![https://fuellabs.github.io/fuel-docs/master/images/fuel-parallel.png](https://fuellabs.github.io/fuel-docs/master/images/fuel-parallel.png)

Fuel 在不牺牲去中心化的情况下为以太坊带来了规模。FuelVM 旨在减少对传统区块链虚拟机架构的浪费处理，同时极大地增加了开发人员的潜在设计空间。FuelVM 可以使用 CPU 的所有线程和内核来验证交易。

### 2.1.5 Fuel [配置](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#fuel-configurations)

![https://fuellabs.github.io/fuel-docs/master/images/configs.png](https://fuellabs.github.io/fuel-docs/master/images/configs.png)

作为模块化执行层，Fuel 可以在这些类别中的任何一个中运行。开发人员可以通过切换客户端中的一些模块来按需配置 Fuel。

### 2.1.6 [改进的虚拟机](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#improved-vm)

以太坊社区提出了许多改进方案以提高 EVM 性能。不幸的是，这些改进建议中的许多尚未实施，因为它们会破坏向后兼容性。

建立在以太坊上的执行层为我们提供了一个新的机会来构建更好的东西。设计不需要向后兼容，事实上，可以做任何必要的事情来为以太坊提供全球吞吐量和采用。FuelVM 是对 EVM 的极大改进。[在此处查看](https://fuellabs.github.io/fuel-docs/master/what-is-fuel.html)在 FuelVM 中实施的 EIP（以太坊改进提案）的非详尽列表。

### [FuelVM 和 EVM 有很多重叠之处。以下是它们的不同之处，请在](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#the-fuelvm-and-evm-have-a-lot-of-overlap-heres-how-theyre-different-view-a-more-complete-list-at-fuelvm-vs-evm)[FuelVM 与 EVM中查看更完整的列表](https://fuellabs.github.io/fuel-docs/master/vs-evm.html)

- [**FuelVM 具有全局共享内存架构，而不是上下文本地内存**](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#the-fuelvm-has-a-globally-shared-memory-architecture-instead-of-context-local-memory)

FuelVM 具有全局共享内存架构。不是每个合约调用都有自己独立的内存空间、调用数据和返回数据，而是所有合约调用帧共享全局内存。这块内存在所有调用帧之间共享，并且是全局可读的。这使您可以在合约之间传递数据而无需昂贵的存储，并传递数据块而无需序列化、从调用数据复制到内存等。[在此处](https://fuellabs.github.io/fuel-docs/master/fuelvm/memory_model.html)阅读有关 FuelVM 内存模型的更多信息。

- [**FuelVM 专为欺诈证明而设计**](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#the-fuelvm-is-designed-for-fraud-provability)

EVM 是一个复杂的机器来构建欺诈证明。它通常需要利用第二层（例如 WASM 或 MIPS）解释为可证明欺诈的系统。查看FuelVM带有欺诈证明的[用户主权以及欺诈证明](https://fuellabs.github.io/fuel-docs/master/why-fuel.html)如何[解锁关键功能](https://fuellabs.github.io/fuel-docs/master/modular-movement.html)。

- [**FuelVM 拥有多个原生资产**](https://fuellabs.github.io/fuel-docs/master/why-fuel.html#fuelvm-has-multiple-native-assets)

在以太坊中，唯一的原生资产是以太币。它是唯一一个在成本和通过电话推拉的能力方面获得一流待遇的公司。在 Fuel 中，任何合约都可以使用一组简单的资产操作码来铸造其基于 UTXO 的原生资产。所有这些都可以获得原生级调用和优化的好处。[在Sway 文档](https://fuellabs.github.io/sway/v0.23.0/blockchain-development/native_assets.html)和[此处](https://fuellabs.github.io/fuel-docs/master/fuelvm/native_assets.html)阅读有关对多个本机资产的支持的更多信息。

[在此处](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/main.md)阅读 FuelVM 的完整规范。

# 2.2 [什么是Fuel？](https://fuellabs.github.io/fuel-docs/master/what-is-fuel.html#what-is-fuel)

Fuel v1 最初是用于单一以太坊的第 2 层（L2）可扩展性技术。部署于 2020 年底，这是以太坊主网的第一个optimistic rollup。

如今，Fuel 是最快的模块化执行层。Fuel 提供最高的安全性和灵活的吞吐量，专注于卓越的开发人员体验。

我们是这样做的：

- 并行交易执行
- 燃料虚拟机 (FuelVM)
- [使用 Sway 语言和 Forc 的开发人员工具](https://fuellabs.github.io/fuel-docs/master/fuel-toolchain.html)

[**并行事务执行**](https://fuellabs.github.io/fuel-docs/master/what-is-fuel.html#parallel-transaction-execution)

Fuel 通过使用 UTXO 模型形式的严格状态访问列表并行执行交易的能力，提供了无与伦比的处理能力。这使 Fuel 能够使用更多的线程和 CPU 内核，这些线程和内核通常在单线程区块链中处于空闲状态。因此，Fuel 可以提供比其单线程同类产品更多的计算、状态访问和事务吞吐量。

使用 EVM，很难知道事务之间是否以、及在何处存在依赖关系，因此您被迫按顺序执行事务。

FuelVM 使用 UTXO 模型，通过所谓的状态访问列表识别交易依赖关系，从而实现并行交易执行。使用 FuelVM，Fuel 全节点识别交易涉及的账户，在执行前映射出依赖关系。

### [FuelVM](https://fuellabs.github.io/fuel-docs/master/what-is-fuel.html#fuelvm)

FuelVM 从以太坊生态系统中学习，实施了多年来向以太坊 VM (EVM) 提出的改进，但由于需要保持向后兼容性而无法实施。

以下是在 FuelVM 中实现的一些 EIP：

| 生态工业园 | 描述 | 执行 |
| --- | --- | --- |
| 易于并行化 | 通过指定它们可以访问的地址，允许并行处理 EVM 中的事务。 | Fuel 可以通过我们的 UTXO 模型使用严格的状态访问列表来并行执行交易。这允许 Fuel 使用 CPU 的所有线程和内核来验证交易。 |
| EIP-2098：紧凑的签名表示 | 将签名从 65 字节减少到 64 字节，以简化客户端代码中的事务处理、降低 gas 成本并减小事务大小。 | Fuel 将签名数据压缩一个字节，从 65 到 64 字节。 |
| EIP-3074：AUTH和AUTHCALL操作码 | 引入了两个 EVM 指令AUTH和AUTHCALL，以启用批处理功能，允许气体赞助、到期、脚本编写等。 | Fuel 有脚本和谓词，当它们组合时，允许在一个批次中执行多个调用。 |
| EIP-3102：二叉树结构 | 为帐户和存储尝试引入二进制结构和默克尔化规则，将它们合并为单个“状态”trie。二进制尝试比十六进制尝试更小（~4x）的证明，使其成为无状态友好以太坊的首选设计。 | Fuel 使用二进制 Sparse Merkle Trie 而不是 Patricia Merkle Trie，这使得证明更小，性能更好。 |
| EIP-4758：停用 SELFDESTRUCT | SELFDESTRUCT将操作码重命名为SENDALL，仅立即将账户中的所有 ETH 移至目标；不再破坏代码或存储或更改随机数。禁用SELFDESTRUCT将是无国籍状态的要求。 | FuelVM 没有SELFDESTRUCT可以使客户端实现复杂化的操作码。 |
| EIP-5027：取消对合约代码大小的限制 | 取消对代码大小的限制，这样用户就可以部署大代码合约，而不必担心将合约拆分成多个子合约。随着去中心化应用的迅猛发展，智能合约的功能也越来越复杂，新开发的合约规模也在稳步增长。因此，我们面临更多关于 24576 字节合约大小限制的问题。 | FuelVM 对低于其物理限制的单个合约的大小没有限制。我们有一条指令允许您将另一个合约中的字节码加载到当前执行上下文中，即使您必须在多个交易中拆分字节码，您也可以将其用作单个合约。它将有一个单一的整体字节码和一个状态。在 EVM 中，如果你在两个交易中分配一个合约，它就是两个独立的合约，你必须做一些事情，比如委托调用来共享两个合约之间的状态，并且不能做在每个合约的字节码之间跳转之类的事情。 |
| EIP-5065：转移以太币的说明 | 添加一条新指令，将以太币传输到目标地址，而不将执行流程移交给它。以太坊目前没有理想的方式来转移以太币而不转移执行流程。人们已经提出了可重入保护和类似的解决方案来防止某些类型的攻击，但这并不是一个理想的解决方案。 | FuelVM 有一个名为 的指令TR，是 transfer 的缩写，它将原生资产转移到合约，但不允许接收合约执行逻辑。您可能希望这样做以确保接收合同无法重新输入。这在 EVM 中不作为原生的一流指令存在——你可以通过自毁合约来做到这一点，但这是一个只适用于 ETH 的混乱解决方法。 |
| EIP-86：交易来源和签名的抽象和EIP-2938：账户抽象 | 实施一组更改，以“抽象出”签名验证和随机数检查的综合目的，允许用户创建执行任何所需签名/随机数检查的“账户合约”，而不是使用目前硬编码到交易处理中的机制。 | FuelVM 具有无状态账户抽象，使应用层逻辑能够配置交易的有效性规则。在今天的以太坊上，如果用户有足够的以太币，交易是有效的，nonce 是正确的，并且签名是有效的。通过账户抽象，用户无需硬分叉即可更改交易逻辑的有效性。这可能意味着对签名方案的更改或本机锁定多重签名背后的帐户。 |
| EIP-1051：EVM 的溢出检查 | 此 EIP 为 EVM 算术运算添加了溢出检查，并添加了两个用于检查和清除溢出标志的新操作码。由于 EVM 对模 2^256 整数进行操作，并且不提供内置溢出检测或预防，因此需要对每个算术运算进行手动检查。 | 溢出检查内置在 FuelVM 中，可以选择禁用。 |
| EIP-2803：丰富的交易 | 如果一笔交易的地址为 x，那么交易的数据将被视为 EVM 字节码，并将从交易的 CALLER（又名：交易签名者）的上下文中执行。许多以太坊 DApp 需要用户批准多笔交易才能产生一种效果。这会导致糟糕的用户体验，并使与 DApp 交互的体验复杂化。 | FuelVM 有实现这一点的脚本。 |
| EIP-2926：基于块的代码默克尔化 | 字节码目前是在证明散列之后块见证大小的第二个贡献者。将 trie 从十六进制转换为二进制将见证的哈希部分减少 3 倍，从而使代码成为第一个贡献者。通过将合约代码分成块并提交到 Merkle 树中的这些块，无状态客户端将只需要在给定事务期间触及的块来执行它。 | 要在以太坊上获得代码散列，您需要将所有字节码散列在一起。问题是，如果你想用无状态或欺诈证明来做事情，为了证明这个哈希是有效的，你必须提供所有的字节码，每个合约最多 24KB。这个 EIP 建议我们应该对它进行 merkalize 而不是散列。FuelVM 通过使用代码根而不是代码哈希来实现这一点。 |

### Sway[语言](https://fuellabs.github.io/fuel-docs/master/what-is-fuel.html#sway-language)

Sway 是一种用于 Fuel 虚拟机 (FuelVM) 的领域特定语言 (DSL)，这是一种专为 Fuel 区块链设计的区块链优化 VM。Sway 基于 Rust，并包含利用区块链 VM 的语法，而没有不必要的冗长样板。

Sway 是与 FuelVM 一起创建的，专为高计算燃料环境而设计。在此处查看 Sway 文档。

### [开发者工具](https://fuellabs.github.io/fuel-docs/master/what-is-fuel.html#developer-tooling)

Fuel Labs 正在开发一套开发人员工具，以创建无缝的开发人员体验。通过在内部创建所有内容，Fuel Labs 保证了工具链的维护，避免了分散的开发者生态系统的陷阱。

Fuel 使用我们自己的称为 Sway 的特定领域语言和称为 Forc（Fuel Orchestrator）的支持工具链，为开发人员提供了强大而流畅的体验。我们的开发环境保留了 Solidity 等智能合约语言的优势，同时采用了 Rust 工具生态系统中引入的范式。现在，开发人员可以获得完全垂直集成的体验，从虚拟机到 CLI 的每个组件都可以协调工作。

**Fuel 工具链：** 强制：构建、运行测试、启动区块浏览器的本地实例、格式化。

[在这里](https://fuellabs.github.io/fuel-docs/master/fuel-toolchain.html)查看燃料工具链。

**即将推出：** 一套审计工具，从模糊测试和形式验证到代码覆盖和运行时门控。

# 2.2.1 Fuel[工具链](https://fuellabs.github.io/fuel-docs/master/fuel-toolchain.html#the-fuel-toolchain)

由 Fuel Labs 设计和构建的全套工具，用于启用/协助 Fuel 应用程序开发体验。

工具概述：

- `fuel-core`
    
    ：Fuel VM 节点客户端。
    
- `forc`
    
    : 燃料协调器。包括 Sway 编译器、打包和插件支持。
    
- `Fuel Indexer`[在这里](https://fuellabs.github.io/fuel-indexer/master/the-fuel-indexer.html)
    
    ：一个独立的二进制文件，可用于索引区块链的各种组件。
    
    查看文档。
    

Fuel Labs 的强制插件：

- `forc-fmt`
    
    : Sway 代码格式化程序。
    
- `forc-lsp`
    
    : Sway 语言服务器协议实现。
    
- `forc-explore`
    
    ：运行燃料块资源管理器。
    
- `forc-client`
    
    ：用于通过 CLI 部署和运行 Sway 应用程序。
    
- `forc-wallet`
    
    ：用于初始化钱包、添加账户和签署交易。
    
- `fuelup`
    
    ：Fuel 工具链管理器 - 检索上述所有内容的简单方法。
    

有关工具链的更具体文档，请在[此处](https://fuellabs.github.io/sway/v0.19.2/introduction/sway-toolchain.html)查看 Sway 文档。

### 在 Fuel 上创建项目

开发人员可以使用一个工具链获得开始为 Fuel VM 创建 Sway 应用程序所需的一切，这得益于创建 FuelVM 和 Sway 语言的同一团队。

开发人员在以太坊生态系统中工作时面临的一个常见问题是如何选择一组工具来开始。从历史上看，以太坊的创始人一直特别关注较为底层的协议、EVM 的细节以及Solidity，将创建可访问的高级工具的工作留给了社区。结果，来自不同的第三方出现了许多不同的项目，旨在提供这些高级解决方案，例如 truffle、dapptools、hard hat、foundry等等。新的以太坊开发人员可能很难驾驭这个空间，并且确定自己在选择其中一个工具链时做出了正确的选择，因此出现选择困难。

相比之下，Fuel Labs 采用精心策划的“batteries-included”的模块化方法来提供工具。我们的目标是提供一套全面的、标准化的、规范的工具集，不仅涵盖堆栈的较低级别（如协议和 VM 实现），还涵盖更高级别（如包管理、编辑器支持、常用插件等）更多的）。我们的目标是以公开、透明和欢迎外部贡献的方式这样做。

澄清一下，目标根本不是阻止第三方扩展Fuel生态系统。相反，我们的目标是使官方工具链变得更好，以激发上游贡献并在一个统一的开发者生态系统下进行协作。对于那些我们的工具不足并且需要添加或扩展的情况，我们通过提供可扩展的插件系统`forc`，以一种易于共享且为 Fuel 开发人员所熟悉的方式实现社区创造力。

Sway VS Code 插件是我们统一开发者体验愿景的一个很好的例子。该插件与 Sway 语言服务器交互，这是一个与 Sway 编译器直接通信并共享其大部分代码库的强制插件。该插件还允许启动 Fuel 节点，实时显示其状态，并（在不久的将来）通过类型脚本 SDK 与您的智能合约进行交互。一个完整的解决方案，完全来自插件内部，来自一个具有共同愿景的团队。

[您可以按照Fuelup 快速入门指南](https://fuellabs.github.io/fuelup/master/installation/index.html#quickstart)开始试验 Fuel 工具链。

# 2.3 [模块化运动](https://fuellabs.github.io/fuel-docs/master/modular-movement.html#modular-movement)

尽管 L2 为访问以太坊生态系统开辟了成本降低的空间，但总吞吐量的增长充其量只是适度的（无论是optimistic方法还是 ZK 方法）。在以太坊的高流量时代，L2 未能保持低成本，每笔交易通常会上涨到几美元。

作为一个社区，如果我们想要实现真正的全球访问区块链技术，我们不能满足于适度降低费用。我们需要戏剧性的改变。改变不仅减少了浪费和效率低下，而且开辟了区块链领域前所未有的新用例。

第 1 层 (L1) 区块链架构正在发生巨大的转变。我们正在从共识、数据可用性和执行紧密耦合的整体设计（例如今天的以太坊）转向模块化的未来，其中执行与数据可用性和共识分离（例如明天的 Eth2 或 Celestia）。这种分离允许在基础层进行专业化，从而显着增加带宽容量。

- [**为什么是模块化？**](https://fuellabs.github.io/fuel-docs/master/modular-movement.html#why-modular)

模块化区块链架构本身并不支持扩展。结果派生的属性使这成为可能。Fuel 是为防欺诈而构建的，支持信任最小化的轻客户端，在不需要大量资源使用的情况下实现高安全性。

- [**安全与资源要求**](https://fuellabs.github.io/fuel-docs/master/modular-movement.html#security-vs-resource-requirements)

![https://fuellabs.github.io/fuel-docs/master/images/resource-security-1.png](https://fuellabs.github.io/fuel-docs/master/images/resource-security-1.png)

在单体架构中，用户必须在高安全性和高计算资源使用率和可信安全性和低计算资源使用率之间进行选择。例如，[以太坊旨在允许消费级硬件能够运行完整节点](https://ethereum.org/en/run-a-node/)，这是一种通过下载和验证每笔交易来提供最大安全性的节点。通过运行一个完整的节点，用户不必相信链是有效的，而是可以验证自己。然而，运行一个完整的节点需要大量的磁盘空间和不可忽略的 CPU 分配，并且可能需要数天时间才能从创世同步区块链。

或者，用户可以运行轻客户端，也称为诚实的多数轻客户端。假设最重的链是有效的，轻客户端只下载块头并检查其工作量证明 (PoW)，而不是下载所有块来验证交易。诚实的多数轻客户端，他们相信大多数验证者都是诚实的，并且会拒绝欺诈性交易。

运行轻客户端所需的计算资源和存储量比完整节点低几个数量级。

记住差异的简单方法：诚实的多数轻客户端只有在大多数验证者都是诚实的情况下才是安全的。即使所有验证者都不诚实，完整节点也是诚实的。

通过运行一个完整的节点，您可以获得验证交易的最大安全性，但也必须花费大量的计算资源来实现这一点。因为轻客户端不需要 24/7 全天候运行并且不直接与链交互，所以计算要求要低得多，但安全性也很低。

- [**信任最小化的轻客户端**](https://fuellabs.github.io/fuel-docs/master/modular-movement.html#trust-minimized-light-clients)

![https://fuellabs.github.io/fuel-docs/master/images/fuel%20light%20client.png](https://fuellabs.github.io/fuel-docs/master/images/fuel%20light%20client.png)

Fuel 的设计让轻量级客户通过欺诈证明相信区块是有效的。这消除了对受信任方的需求，同时保持低资源需求并实现高安全性。对于像以太坊这样的单片链，有一种意识形态激励来保持对全节点的计算要求较低，以允许用户真正拥有主权。

由于 Fuel 是为防欺诈而构建的，因此对全节点的资源要求可能更高，从而增加了带宽要求，同时仍然允许用户通过信任最小化的轻客户端来验证链。

# 2.3.1 [单体架构](https://fuellabs.github.io/fuel-docs/master/monolithic.html#monolithic-architecture)（**[Monolithic Architecture](https://fuellabs.github.io/fuel-docs/master/monolithic.html#monolithic-architecture)**）

我们所知道的区块链有四个功能。没有特别的顺序：

- 执行：执行交易以更新状态。
- 解决：争议解决。
- 共识：定义状态并验证区块链上的所有节点具有相同的状态。
- 数据可用性：确保区块数据已发布到网络。

单片区块链是一种区块链架构，可在这一层上同时处理所有四种功能。

![https://fuellabs.github.io/fuel-docs/master/images/monolithic.png](https://fuellabs.github.io/fuel-docs/master/images/monolithic.png)

- [**单片机的挑战**](https://fuellabs.github.io/fuel-docs/master/monolithic.html#challenges-with-monolithic)

单体架构的一些限制和挑战：

- [**昂贵且低效的交易验证**](https://fuellabs.github.io/fuel-docs/master/monolithic.html#costly-and-inefficient-transaction-verification)

为了验证链中交易的有效性，全节点必须下载整个链并在本地执行每笔交易。

- [**资源限制**](https://fuellabs.github.io/fuel-docs/master/monolithic.html#resource-constraints)

区块链受其节点的资源容量约束。吞吐量受到*单个*节点的资源需求的限制，因为区块链是跨节点复制的，而不是分布式的。

- [**共享资源**](https://fuellabs.github.io/fuel-docs/master/monolithic.html#shared-resources)

在单体架构中，链的四个功能在相同的有限计算资源集上运行。例如，使用节点的执行能力意味着数据可用性剩余的容量更少。

- [**可扩展性**](https://fuellabs.github.io/fuel-docs/master/monolithic.html#scalability)

可扩展性被定义为吞吐量与去中心化的比率。要增加吞吐量（每秒事务数），您必须增加带宽、计算和存储容量，这会推高以用户身份运行完整节点的成本。这不是可扩展性，因为它减少了可以运行完整节点来验证链的人数。

## 2.4 Fuel[配置](https://fuellabs.github.io/fuel-docs/master/fuel-configurations.html#fuel-configurations)

Fuel 是一种模块化架构，旨在以多种不同的配置运行。

![https://fuellabs.github.io/fuel-docs/master/images/configs.png](https://fuellabs.github.io/fuel-docs/master/images/configs.png)

### 2.4.1 Fuel作为rollup

- [**定义汇总**](https://fuellabs.github.io/fuel-docs/master/rollup.html#defining-rollups)

Layer-2 是用于描述一组扩展解决方案的术语。rollup是建立在layer1之上的链下网络，它执行链下交易、捆绑这些交易，并将捆绑的交易作为单个交易发布到第 1 层链上。

查看[此资源以了解有关rollup和layer-2s的更多信息](https://ethereum.org/en/layer-2/)。

- [**作为 Rollup 或 Layer-2 的燃料**](https://fuellabs.github.io/fuel-docs/master/rollup.html#fuel-as-a-rollup-or-layer-2)

Fuel 旨在运行模块化执行层，这种配置类似于我们今天在以太坊上所谓的 rollups 或 layer-2s。Rollups 通常使用Optimistic或 ZK 配置来进行有效性或事务仲裁。Fuel 技术与其中任何一种都无关，可以用作有效性或欺诈证明系统。

layer2和rollup主要是为单片区块链堆栈设计的，这意味着它们通常不会像 Fuel 那样针对大量layer1带宽潜力进行优化，Fuel 是专门为处理这种潜力而配置的。

### 2.4.2 Fuel作为 layer-1

- **layer-1[定义](https://fuellabs.github.io/fuel-docs/master/l1.html#defining-a-layer-1)**

layer-1 是一个区块链网络，负责处理链的所有功能，包括结算、执行、数据可用性和共识。这被通俗地称为第 1 层，因为汇总或第 2 层位于这些链的顶部。

查看[此资源以了解有关layer-1的更多信息。](https://ethereum.org/en/layer-2/#what-is-layer-2)

- **Fuel[作为](https://fuellabs.github.io/fuel-docs/master/l1.html#fuel-as-a-layer-1)layer-1**

Fuel 技术包括作为完整的layer-1解决方案运行的所有组件。这些组件包括共识、数据可用性、结算和交易执行。在这种模式下运行的常见配置是权限证明和通过 Tendermint-BFT 风格的权益证明。

虽然 Fuel 可以在这种配置下运行，但我们不会在测试之外推广或支持这一点，因为 Fuel 的更广泛使命是增强现有区块链（例如以太坊）作为高性能执行层。

### 2.4.3 Fuel作为状态通道

- [**状态通道定义**](https://fuellabs.github.io/fuel-docs/master/state-channel.html#defining-state-channels)

状态通道是一种智能合约，可在预定义的各方之间执行链下交易。每笔交易都会更新链的状态，并且可以在链上进行加密证明。

查看[此资源以了解有关状态通道的更多信息。](https://ethereum.org/en/developers/docs/scaling/state-channels/)

- **Fuel[作为状态通道](https://fuellabs.github.io/fuel-docs/master/state-channel.html#fuel-as-a-state-channel)**

FuelVM 是一种具有确定性状态系统的定价虚拟机架构，这使其非常适合多方通道设计，在这种设计中，各方都必须清楚地了解每个通信步骤或窗口中系统的确切状态。

虽然我们没有提供开箱即用的 Fuel 技术的通道配置，但 FuelVM 非常适合处理这种特殊用例。

### 2.4.4 [燃料配置](https://fuellabs.github.io/fuel-docs/master/sidechain.html#fuel-configurations)

- [**定义侧链**](https://fuellabs.github.io/fuel-docs/master/sidechain.html#defining-sidechain)

侧链是一种区块链，具有与基础链的单向信任最小化桥接。

查看[此资源以了解有关侧链的更多信息。](https://ethereum.org/en/developers/docs/scaling/sidechains/)

- [**燃料作为侧链**](https://fuellabs.github.io/fuel-docs/master/sidechain.html#fuel-as-a-sidechain)

Fuel 技术还可以作为现有 layer-1 的侧链运行。这意味着在 layer-1 和 Fuel 之间有一个消息传递桥。在这种配置中，数据可用性将由侧链处理，而结算由第 1 层处理。

还可以选择在半可证明的配置中运行它，从而可以使用欺诈证明来确保使用第 1 层作为仲裁者来确保更好的有效性。