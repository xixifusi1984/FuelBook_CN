# Fuel Book chapter 4: FuelVM

## 4.1 [合约和调用模型](https://fuellabs.github.io/fuel-docs/master/fuelvm/contract_call_model.html#contract-and-call-model)

Fuel 使用与以太坊类似的模型进行合约和跨合约调用。合约可以用 a 调用其他合约`[CALL](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/instruction_set.md#call-call-contract)`（类似于以太坊[消息调用](https://github.com/ethereum/yellowpaper/blob/8fea825c80e27fa9df5d89fb3365d1067788724e/Paper.tex#L1451)）。与 EVM 只能通过调用转发其基础资产（即 ETH）不同，FuelVM 可以通过调用转发单个[原生可替代资产](https://fuellabs.github.io/fuel-docs/master/fuelvm/native_assets.html)。

交易可能会发起合约调用。以太坊交易可以[直接调用单个合约](https://github.com/ethereum/yellowpaper/blob/8fea825c80e27fa9df5d89fb3365d1067788724e/Paper.tex#L322)。燃料交易改为执行[脚本](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/main.md#script-execution)（附加到交易的任意字节码），该脚本可以调用任意数量的合约。

## 4.2 [记忆模型](https://fuellabs.github.io/fuel-docs/master/fuelvm/memory_model.html#memory-model)

EVM 使用线性内存（即从零开始并不断增长），没有定义的限制。分配内存会消耗[二次方的 gas 量](https://github.com/ethereum/yellowpaper/blob/8fea825c80e27fa9df5d89fb3365d1067788724e/Paper.tex#L2114)，这严重限制了单个上下文（即合约调用）中可用的内存量。

此外，EVM 的每个上下文都有一个单独的内存空间。上下文可以通过将数据复制到调用数据并返回数据缓冲区来相互通信。

*FuelVM每个事务*使用一个共享内存块。内存以已知的上限静态分配，允许直接实现堆类型，例如[向量](https://github.com/FuelLabs/sway/blob/master/sway-lib-std/src/vec.sw)。FuelVM 中的内存在上下文中是全局可读的，但在本地是可写的。每个上下文只能写入它拥有[所有权](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/main.md#ownership)的堆栈和堆的一部分。

## 4.3 [多个原生资产](https://fuellabs.github.io/fuel-docs/master/fuelvm/native_assets.html#multiple-native-assets)

Fuel支持多种原生资产作为一等公民。任何单个资产都可以使用`[CALL](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/instruction_set.md#call-call-contract)`. 合约具有所有可能资产的余额，而不仅仅是基础资产。

请注意，只有基础资产可用于支付gas。

## 4.4 [FuelVM 与 EVM](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#fuelvm-vs-evm-explained) 的对比

FuelVM 向 EVM、Solana、WASM、比特币和 Cosmos 学习。

本页旨在简明扼要地概述 FuelVM 与 EVM 的不同之处。

- [**FuelVM 具有全局共享内存架构，而不是上下文本地内存**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#the-fuelvm-has-a-globally-shared-memory-architecture-instead-of-context-local-memory)

FuelVM 具有全局共享内存架构。不是每个合约调用都有自己独立的内存空间、调用数据和返回数据，而是所有合约调用帧共享全局内存。这块内存在所有调用帧之间共享，并且是全局可读的。这使您可以在合约之间传递数据而无需昂贵的存储，并传递数据块而无需序列化、从调用数据复制到内存等。[在此处](https://fuellabs.github.io/fuel-docs/master/fuelvm/memory_model.html)阅读有关 FuelVM 内存模型的更多信息。

- [**FuelVM 专为欺诈证明而设计**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#the-fuelvm-is-designed-for-fraud-provability)

EVM 是一个复杂的机器来构建欺诈证明。它通常需要将第二层（例如 WASM 或 MIPS）解释为可证明欺诈的系统。查看带有欺诈证明的[用户主权以及欺诈证明](https://fuellabs.github.io/fuel-docs/master/why-fuel.html)如何[解锁关键功能](https://fuellabs.github.io/fuel-docs/master/modular-movement.html#trust-minimized-light-clients)。

- [**FuelVM 拥有多个原生资产**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#fuelvm-has-multiple-native-assets)

在以太坊中，唯一的原生资产是以太币。它是唯一一个在成本和通过电话推拉的能力方面获得一流待遇的公司。在 Fuel 中，任何合约都可以使用一组简单的资产操作码来铸造其基于 UTXO 的原生资产。所有这些都可以获得原生级调用和优化的好处。[在Sway 文档](https://fuellabs.github.io/sway/v0.23.0/blockchain-development/native_assets.html)和[此处](https://fuellabs.github.io/fuel-docs/master/fuelvm/native_assets.html)阅读有关对多个本机资产的支持的更多信息。

- [**FuelVM 使用 64 位字而不是 256 位**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#fuelvm-uses-64-bit-words-instead-of-256-bit)

现代处理器具有 64 位寄存器，所有指令集都在 64 位上运行。这些是最有效的指令，当您处理 256 位时，您正在处理大数字，并且由于现代处理器并非原生处理这些数字，这意味着您必须在软件中做更多的事情。

- [**FuelVM 是基于寄存器的，而不是基于堆栈的**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#the-fuelvm-is-register-based-instead-of-stack-based)

与基于堆栈的 VM 相比，基于寄存器的 VM 通常需要更少的指令来完成相同的工作。由于每项操作都是定价的，因此进行优化以减少完成相同工作量所需的操作数量会带来巨大的好处。

- [**FuelVM 从一开始就使用原子 UTXO 范式构建**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#the-fuelvm-is-built-with-an-atomic-utxo-paradigm-from-the-start)

Fuel 使用了一个 UTXO 系统，该系统可以实现更有效的资产转移和所有权系统，其中每次转移资金时都不必重建账户树。

- [**FuelVM 移除`approve`,`transferFrom`**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#the-fuelvm-removes-approve-transferfrom)

FuelVM 消除了使用脚本批准/transferFrom UX 的需要。与 EVM 不同，FuelVM 有脚本，允许从原始发送者发生许多操作，而无需部署合约。[在这里](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/main.md#script-execution)阅读更多。

- [**在 Fuel 中实施的 EIP**](https://fuellabs.github.io/fuel-docs/master/vs-evm.html#eips-implemented-in-fuel)

FuelVM 实现了多个社区建议和支持的 EIP，但由于需要保持向后兼容性而无法实现。查看非详尽列表，也可[在此处](https://fuellabs.github.io/fuel-docs/master/what-is-fuel.html)获得：

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

#