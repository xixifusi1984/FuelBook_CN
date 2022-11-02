# Fuel book chapter3:技术

## 3.1 [并行事务执行](https://fuellabs.github.io/fuel-docs/master/technology/parallel_tx_execution.html#parallel-transaction-execution)

### [并行事务执行](https://fuellabs.github.io/fuel-docs/master/technology/parallel_tx_execution.html#parallel-transaction-execution)

以太坊按顺序处理交易（即一个接一个）。随着现代处理器变得越来越多线程，但在单核加速上停滞不前，能够并行执行事务（即一次多个事务）是一个非常理想的属性。

如果没有确定和处理事务之间[依赖](https://en.wikipedia.org/wiki/Parallel_computing#Dependencies)关系的机制，并行执行事务是一种竞争条件，并且会导致非确定性执行。已经尝试向以太坊添加[optimistic并发执行](https://arxiv.org/abs/1901.01376)逻辑，但表现出不一致的性能优势，而且只能在非对抗性条件下工作。

[EIP-648](https://github.com/ethereum/EIPs/issues/648)提议为事务添加*访问列表*，即每个事务将触及的共享状态列表。使用这样的列表，客户端可以将事务划分为具有不相交访问列表的集合，并在每个集合中并行执行事务。然而，EIP 从未实施，部分原因是实施和设计效率低下，导致状态访问成为瓶颈而不是计算。

### [状态访问列表和 UTXO](https://fuellabs.github.io/fuel-docs/master/technology/parallel_tx_execution.html#state-access-lists-and-utxos)

Fuel 支持通过严格（即强制）访问列表的并行事务执行，类似于 EIP-648。每笔交易都必须指定交易*可以*与哪些合约进行交互；如果交易尝试访问不在此列表中的合约，则执行将*恢复*。使用这些访问列表，可以在涉及不相交合约集的交易中并行执行。有关其他上下文，请参见[此处](https://github.com/FuelLabs/fuel-specs/blob/master/specs/protocol/tx_validity.md#access-lists)。

访问列表是通过 UTXO 实现的。UTXO 提供了其他不错的属性，但出于并行事务执行的目的，它仅用作[严格的访问列表](https://forum.celestia.org/t/accounts-strict-access-lists-and-utxos/37)。

## 3.2 [欺诈证明](https://fuellabs.github.io/fuel-docs/master/technology/fraud_proofs.html#fraud-proofs)

欺诈证明是一种区块链验证机制，除非在某个可配置的时间窗口内提供声明无效的证据，否则接受对新块的声明。这允许信任最小化的轻客户端在网络中只有一个诚实的完整节点可用以产生欺诈证明的假设下是安全的。Fuel 协议和 FuelVM 都旨在在限制性环境（例如以太坊虚拟机）中进行欺诈证明。

[状态转换欺诈证明](https://arxiv.org/abs/1809.09044)是一种通用的欺诈证明机制，但存在需要全局状态树的缺点——这是一个固有的顺序瓶颈。[UTXO 欺诈证明](https://ethresear.ch/t/compact-fraud-proofs-for-utxo-chains-without-intermediate-state-serialization/5885)避免了这个瓶颈；他们只需要每次花费 UTXO 来“指向”UTXO 的创建。证明指针无效，或者所指向的内容与所花费的内容不匹配，足以彻底证明欺诈。

Fuel 交易格式和验证逻辑可通过 UTXO 欺诈证明进行欺诈证明。FuelVM 依赖于[交互式验证游戏](https://www.usenix.org/system/files/conference/usenixsecurity18/sec18-kalodner.pdf)协议，其中执行跟踪被一分为二，直到必须执行一个步骤来检查不匹配。FuelVM[指令集](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/instruction_set.md)专门设计用于在以太坊虚拟机中既易于表达又可防欺诈。