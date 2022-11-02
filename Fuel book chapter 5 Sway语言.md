# Fuel book chapter 5: Sway语言

Sway 是一种智能合约编程语言，它优先考虑安全性和速度，并将现代编程语言理论引入区块链。[在此处](https://fuellabs.github.io/sway/latest/)查看 Sway 文档。

[**Rust + Solidity = Sway**](https://fuellabs.github.io/fuel-docs/master/sway-language.html#rust--solidity--sway)

Sway 优先考虑编译时分析和安全性，类似于 Rust 的借用检查器和安全第一语义。此外，它具有 Rust 的语法。从 Solidity 开始，Sway 采用了智能合约范式语言的概念，该语言具有内置的顶级合约存储和区块链机制，用于符合人体工程学和安全的合约编程。

除了强大的文档、工具、清晰的错误消息等等——Sway 将静态审计的概念引入智能合约。编译器将捕获通常会聘请审计公司来捕获的东西。这是特别独特的。此外，Sway 具有高性能，并具有可扩展的优化通道和针对不同区块链架构的模块化后端。

[在此处](https://github.com/FuelLabs/sway/tree/master/examples)查看 Sway 示例。

## 5.1 [设计理念](https://fuellabs.github.io/fuel-docs/master/design-philosophy.html#sway-design-philosophy)

Sway 遵循 Rust 和 Solidity 的设计理念，并混入了一些 Fuel 意识形态。从 Solidity 开始，我们将智能合约编程的概念作为范式。这导致了存储块、合约 ABI 作为入口点等。

从 Rust 中，我们优先考虑了性能、控制和安全。在 Rust 中，这意味着借用检查器、安全并行（发送和同步）、带注释的不安全等，主要是为了使程序员免于引用释放的内存、共享可变状态和不需要的内存管理。这对于通用语言模型非常有用。然而，Sway 并不是通用的。Sway 的目标是区块链 VM 环境，在该环境中执行不是无限期的，内存分配和管理不太受关注。相反，我们需要优化天然气成本和合同级别的安全性。我们应用了性能、控制和安全的理念，并在这个新的背景下对其进行了解释。这是 Sway 获得状态可变性、状态变量命名空间、静态分析传递和气体分析/优化的编译时检查的地方。

## 5.2 [安全](https://fuellabs.github.io/fuel-docs/master/sway-safety.html#sway-safety)

Sway 提供多层安全性。一方面，我们提供规范的工具和“一种正确的方式”来做事。这可以减少歧义和更正确/有用的工具。该工具提供调试器、气体分析器、测试框架、SDK、格式化程序等。这些工具确保程序员在他们和他们试图实现的算法之间没有任何障碍。安全来自于舒适和符合人体工程学的环境。

此外，Sway 还实施了静态分析检查，如*[检查、效果、交互](https://docs.soliditylang.org/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern)*检查器、状态和存储纯度检查、默认不可变语义以及其他静态编译时审计以提高安全性。