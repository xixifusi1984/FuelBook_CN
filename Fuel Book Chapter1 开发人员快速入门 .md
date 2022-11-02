# Fuel Book Chapter1:开发人员快速入门

本指南将引导开发人员在 Sway 中编写智能合约、进行简单测试、部署到 Fuel 以及构建前端。

在开始之前，了解将在整个文档中使用的术语以及它们之间的关系可能会有所帮助：

- **Fuel**
    
    ：燃料区块链。
    
- **FuelVM**
    
    ：为 Fuel 提供动力的虚拟机。
    
- **Sway**
    
    ：为 FuelVM 设计的特定领域语言；它的灵感来自 Rust。
    
- **Forc**
    
    ：Sway 的构建系统和包管理器，类似于 Rust 的 Cargo。
    

## [了解 Sway 程序类型](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#understand-sway-program-types)

Sway 程序有四种类型：

- `contract`
- `predicate`
- `script`
- `library`

合约、谓词和脚本可以生成可在区块链上使用的工件，而库只是为代码重用而设计的项目，不能直接部署。

智能合约与脚本或谓词的主要区别在于它是可调用的和有状态的。

脚本是链上可运行的字节码，可以调用合约来执行某些任务。它不代表任何资源的所有权，也不能被合约调用。

|  | 可部署在区块链上： | 可以有状态： | 可在区块链上调用： | 专为代码重用而设计： |
| --- | --- | --- | --- | --- |
| 合同 | ✅ | ✅ | ✅ | ❌ |
| 谓词 | ✅ | ❌ | ❌ | ❌ |
| 脚本 | ❌ | ❌ | ❌ | ❌ |
| 图书馆 | ✅（通过契约或谓词） | ❌ | ❌ | ✅ |

有关详细信息，请参阅[有关程序类型的章节](https://fuellabs.github.io/sway/master/sway-program-types/index.html)。

## [安装](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#installation)

首先[安装 Rust 工具链](https://fuellabs.github.io/sway/v0.24.3/introduction/installation.html#dependencies)。

然后，[安装 Fuel 工具链](https://github.com/FuelLabs/fuelup)。

## [您的第一个 Sway 项目](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#your-first-sway-project)

我们将构建一个具有两个功能的简单计数器合约：一个用于递增计数器，一个用于返回计数器的值。

在继续之前，一些有用的信息：

- 本指南是使用 VSCode 作为代码编辑器创建的。
- 在 VSCode 中下载 Sway 语言扩展以获取语法高亮、键盘快捷键等。
- 在 VSCode 中下载 rust-analyzer 扩展以获得语法高亮、代码完成等功能。

**首先创建一个新的空文件夹。我们会调用它`fuel-project`。**

### [写合同](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#writing-the-contract)

然后安装后，在您的文件夹`forc`中创建一个合同项目：`fuel-project`

```

$ cd fuel-project
$ forc new counter-contract
To compile, use `forc build`, and to run tests use `forc test`
----
Read the Docs:
- Sway Book: https://fuellabs.github.io/sway/latest
- Rust SDK Book: https://fuellabs.github.io/fuels-rs/latest
- TypeScript SDK: https://github.com/FuelLabs/fuels-ts
Join the Community:
- Follow us @SwayLang: https://twitter.com/SwayLang
- Ask questions in dev-chat on Discord: https://discord.com/invite/xfpK4Pe
Report Bugs:
- Sway Issues: https://github.com/FuelLabs/sway/issues/new
```

这是`Forc`已经初始化的项目：

```

$ tree counter-contract
counter-contract
├── Forc.toml
└── src
    └── main.sw
```

`Forc.toml`是*清单文件*（类似于`Cargo.toml`Cargo 或`package.json`Node）并定义项目元数据，例如项目名称和依赖项。

在代码编辑器中打开您的项目并删除其中的样板代码，`src/main.sw`以便您从一个空文件开始。

每个 Sway 文件都必须以文件包含的程序类型声明开头；在这里，我们已经声明这个文件是一个合约。

```

contract;

```

接下来，我们将定义一个存储值。在我们的例子中，我们有一个`counter`64 位无符号整数类型的计数器，并将其初始化为 0。

```

storage {
    counter: u64 = 0,
}
```

### [ABI](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#abi)

ABI 定义了一个接口，ABI 中没有函数体。合约必须定义或导入 ABI 声明并实施它。在单独的库中定义您的 ABI 并将其导入您的合约被认为是最佳实践，因为这允许合约的调用者导入和使用脚本中的 ABI 来调用您的合约。

为简单起见，我们将直接在合约文件中定义 ABI。

```

abi Counter {
    #[storage(read, write)]
    fn increment();
    #[storage(read)]
    fn count() -> u64;
}
```

### [一行一行地走](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#going-line-by-line)

`#[storage(read, write)]`是一个注解，表示此函数有权读取和写入存储中的值。

`fn increment();`- 我们正在引入递增功能并表示它不应返回任何值。

`#[storage(read)]`是一个注释，表示此函数有权读取存储中的值。

`fn counter() -> u64;`- 我们正在引入增加计数器的功能并表示函数的返回值。

### [实施 ABI](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#implement-abi)

在 ABI 定义下方，您将编写 ABI 中定义的函数的实现。

```

impl Counter for Contract {
    #[storage(read)]
    fn count() -> u64 {
        storage.counter
    }
    #[storage(read, write)]
    fn increment() {
        storage.counter = storage.counter + 1;
    }
}
```

> 注 return storage.counter;等效于storage.counter。
> 

### [我们刚刚做了什么](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#what-we-just-did)

在中，我们从存储中`fn count()`读取并返回变量。`counter`

```

fn count() -> u64 {
    storage.counter
}
```

在`fn increment()`中，我们从存储中读取变量`counter`并将其值加一。然后我们将新值存储回`counter`.

```

fn increment() {
    storage.counter = storage.counter + 1;
}
```

到目前为止，您的代码应如下所示：

```

contract;
storage {
    counter: u64 = 0,
}
abi Counter {
    #[storage(read, write)]
    fn increment();
    #[storage(read)]
    fn count() -> u64;
}
impl Counter for Contract {
    #[storage(read)]
    fn count() -> u64 {
        storage.counter
    }
    #[storage(read, write)]
    fn increment() {
        storage.counter = storage.counter + 1;
    }
}
```

### [建立合同](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#build-the-contract)

从`fuel-project/counter-contract`目录内部，运行以下命令来构建您的合约：

```

$ forc build
  Compiled library "core".
  Compiled library "std".
  Compiled contract "counter-contract".
  Bytecode size is 232 bytes.
```

我们来看看`counter-contract`构建后文件夹的内容：

```

$ tree .
.
├── Forc.lock
├── Forc.toml
├── out
│   └── debug
│       ├── counter-contract-abi.json
│       ├── counter-contract-contract-id
│       ├── counter-contract-storage_slots.json
│       └── counter-contract.bin
└── src
    └── main.sw
```

我们现在有一个`out`目录，其中包含我们的构建工件，例如 ABI 的 JSON 表示和合约字节码。

## [测试你的合约](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#testing-your-contract)

我们将从使用 Cargo 生成模板添加 Rust 集成测试工具开始。让我们确保我们已经`cargo generate`安装了命令！

```

cargo install cargo-generate

```

> 注意：您可以通过访问它的存储库了解更多关于 cargo generate 的信息。
> 

现在，让我们使用以下内容生成默认测试工具：

```

cargo generate --init fuellabs/sway templates/sway-test-rs --name counter-contract

```

如果一切顺利，输出应如下所示：

```

⚠️   Favorite `fuellabs/sway` not found in config, using it as a git repository: https://github.com/fuellabs/sway
🤷   Project Name : counter-contract
🔧   Destination: /home/user/path/to/counter-contract ...
🔧   Generating template ...
[1/3]   Done: Cargo.toml
[2/3]   Done: tests/harness.rs
[3/3]   Done: tests
🔧   Moving generated files into: `/home/user/path/to/counter-contract`...
✨   Done! New project created /home/user/path/to/counter-contract
```

让我们看一下结果：

```

$ tree .
.
├── Cargo.toml
├── Forc.lock
├── Forc.toml
├── out
│   └── debug
│       ├── counter-contract-abi.json
│       ├── counter-contract-contract-id
│       ├── counter-contract-storage_slots.json
│       └── counter-contract.bin
├── src
│   └── main.sw
└── tests
    └── harness.rs
```

我们有两个新文件！

- 这`Cargo.tomlfuels`
    
    是我们新测试工具的清单，并指定了所需的依赖项，包括
    
    Fuel Rust SDK。
    
- 包含一些样板测试代码来帮助我们开始，`tests/harness.rs`
    
    虽然还没有调用任何合约方法。
    

现在我们有了默认的测试工具，让我们添加一些有用的测试。

在 的底部`test/harness.rs`，定义 的主体`can_get_contract_id()`。下面是你的代码应该是什么样子来验证计数器的值是否增加了：

```

#[tokio::test]
async fn can_get_contract_id() {
    let (instance, _id) = get_contract_instance().await;
    // Increment the counter
    let _result = instance.methods().increment().call().await.unwrap();
    // Get the current value of the counter
    let result = instance.methods().count().call().await.unwrap();
    // Check that the current value of the counter is 1.
    // Recall that the initial value of the counter was 0.
    assert_eq!(result.value, 1);
}
```

`cargo test`在终端中运行。如果一切顺利，输出应如下所示：

```

$ cargo test
  ...
  running 1 test
  test can_get_contract_id ... ok
  test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.11s
```

## [部署合约](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#deploy-the-contract)

现在是时候将合约部署到测试网了。我们将展示如何使用`forc`命令行执行此操作，但您也可以使用[Rust SDK](https://github.com/FuelLabs/fuels-rs#deploying-a-sway-contract)或[TypeScript SDK](https://github.com/FuelLabs/fuels-ts/#deploying-contracts)执行此操作。

为了部署合约，你需要有一个用于签署交易的钱包和一个用于支付 gas 的硬币。首先，我们将创建一个钱包。

### [安装钱包 CLI](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#install-the-wallet-cli)

按照[以下步骤设置钱包并创建帐户](https://github.com/FuelLabs/forc-wallet#forc-wallet)。

输入密码后，一定要保存输出的助记词。

有了这个，你会得到一个看起来像这样的燃料地址`fuel1efz7lf36w9da9jekqzyuzqsfrqrlzwtt3j3clvemm6eru8fe9nvqj5kar8`：保存此地址，因为在我们部署合约时您将需要它来签署交易。

### [获取测试网币](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#get-testnet-coins)

拿着你的账户地址，前往[测试网水龙头](https://faucet-beta-1.fuel.network/)，将一些硬币发送到你的钱包。

### [部署到测试网](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#deploy-to-testnet)

现在你有了一个钱包，你可以`forc deploy`像这样部署并传入测试网端点：

`forc deploy --url https://node-beta-1.fuel.network/graphql --gas-price 1`

> 注意 我们将gas price设置为1。没有这个标志，gas price默认为0，交易会失败。
> 

终端会询问您要签署此交易的钱包地址，粘贴您之前保存的地址，如下所示：`fuel1efz7lf36w9da9jekqzyuzqsfrqrlzwtt3j3clvemm6eru8fe9nvqj5kar8`

终端将输出您的合同 ID。请务必保存它，因为您将需要它来使用 Typescript SDK 构建前端。

终端将输出 a`message to sign`并提示您签名。打开一个新的终端选项卡并通过运行查看您的帐户`forc wallet list`。如果您按照这些步骤操作，您会注意到您只有一个帐户，`0`.

`message to sign`通过运行以下命令，从您的其他终端获取并使用您的帐户进行签名：

```

forc wallet sign` + `[message to sign, without brackets]` + `[the account number, without brackets]`

```

您的命令应如下所示：

```

$ forc wallet sign 16d7a8f9d15cfba1bd000d3f99cd4077dfa1fce2a6de83887afc3f739d6c84df 0
Please enter your password to decrypt initialized wallet's phrases:
Signature: 736dec3e92711da9f52bed7ad4e51e3ec1c9390f4b05caf10743229295ffd5c1c08a4ca477afa85909173af3feeda7c607af5109ef6eb72b6b40b3484db2332c
```

出现提示时输入您的密码，您将获得一个`signature`. 保存该签名，然后返回到您的另一个终端窗口，并将其粘贴到提示您的位置`provide a signature for this transaction`。

最后，您将收到一个`TransactionId`确认您的合同已部署的信息。使用此 ID，您可以前往[区块浏览](https://fuellabs.github.io/block-explorer-v2/)器并查看您的合约。

> 注意 您应该在您的前面TransactionId加上前缀0x以在块资源管理器中查看它
> 

![https://fuellabs.github.io/fuel-docs/master/images/block-explorer.png](https://fuellabs.github.io/fuel-docs/master/images/block-explorer.png)

## [创建一个与合约交互的前端](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#create-a-frontend-to-interact-with-contract)

现在我们要

1. **初始化一个 React 项目。**
2. **安装`fuels`SDK 依赖项。**
3. **修改应用程序。**
4. **运行我们的项目。**

### [初始化一个 React 项目](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#initialize-a-react-project)

为了更好地拆分我们的项目，让我们创建一个新文件夹`frontend`并在其中初始化我们的项目。

在终端中，返回一个目录并使用`[Create React App](https://create-react-app.dev/)`.

```

$ cd ..
$ npx create-react-app frontend --template typescript
Success! Created frontend at Fuel/fuel-project/frontend
```

你现在应该有你的外部文件夹，`fuel-project`里面有两个文件夹：`counter-contract`和`frontend`

![https://fuellabs.github.io/fuel-docs/master/images/quickstart-folder-structure.png](https://fuellabs.github.io/fuel-docs/master/images/quickstart-folder-structure.png)

### [安装`fuels`SDK 依赖项](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#install-the-fuels-sdk-dependencies)

在这一步中，我们需要为项目安装 3 个依赖项：

1. `fuelsWalletContractsProviders`
    
    ：包含所有主要工具的伞包；
    
    ,
    
    ,
    
    等等。
    
2. `fuelchain`
    
    ：Fuelchain 是 ABI TypeScript 生成器。
    
3. `typechain-target-fuels`[Fuel ABI Spec](https://github.com/FuelLabs/fuel-specs/blob/master/specs/protocol/abi.md)
    
    ：Fuelchain Target 是
    
    的具体解释器。
    

> ABI 代表应用程序二进制接口。ABI 通知应用程序与 VM 交互的接口，换句话说，它们向 APP 提供信息，例如合约具有哪些方法、哪些参数、它期望的类型等...
> 

### [安装](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#install)

进入`frontend`文件夹，然后安装依赖项：

```

$ cd frontend
$ npm install fuels --save
added 65 packages, and audited 1493 packages in 4s$ npm install fuelchain typechain-target-fuels --save-dev
added 33 packages, and audited 1526 packages in 2s
```

### [生成合约类型](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#generating-contract-types)

为了更容易与我们的合约交互，我们使用`fuelchain`来解释我们合约的输出 ABI JSON。这个 JSON 是在我们执行将`forc build`Sway 合约编译成二进制文件的那一刻创建的。

如果您看到该文件夹`fuel-project/counter-contract/out`，您将能够在那里看到 ABI JSON。如果您想了解更多信息，请在[此处阅读 ABI 规范](https://github.com/FuelLabs/fuel-specs/blob/master/specs/protocol/abi.md)。

内`counter-contract/frontend`跑;

```

$ npx fuelchain --target=fuels --out-dir=./src/contracts ../counter-contract/out/debug/*-abi.json
Successfully generated 4 typings!
```

现在您应该能够找到一个新文件夹`fuel-project/frontend/src/contracts`。这个文件夹是由我们的命令自动生成的`fuelchain`，这个文件抽象了我们需要做的工作来创建一个合约实例，并为合约生成一个完整的 TypeScript 接口，使得开发变得容易。

### [创建钱包（再次）](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#create-a-wallet-again)

为了与燃料网络进行交互，我们必须提交具有足够资金的签名交易来支付网络费用。Fuel TS SDK 目前不支持 Wallet 集成，这要求我们在 WebApp 中使用 privateKey 有一个不安全的钱包。

> 注意 这应该仅用于开发目的。永远不要公开带有私钥的 Web 应用程序。Fuel钱包正在积极开发中，请在此处关注进度。
> 

在前端项目的根目录下创建一个文件，createWallet.js

```

const { Wallet } = require("fuels");const wallet = Wallet.generate();console.log("address", wallet.address.toString());
console.log("private key", wallet.privateKey);

```

在终端中，运行以下命令：

```

$ node createWallet.js
address fuel160ek8t7fzz89wzl595yz0rjrgj3xezjp6pujxzt2chn70jrdylus5apcuq
private key 0x719fb4da652f2bd4ad25ce04f4c2e491926605b40e5475a80551be68d57e0fcb
```

> 注意 您应该使用生成的地址和私钥。
> 

保存私钥，稍后您将需要将其设置为文件中变量`WALLET_SECRET`的字符串值。`App.tsx`更多关于下面的内容。

[首先，获取您钱包的地址，并使用它从测试网水龙头](https://faucet-beta-1.fuel.network/)中获取一些硬币。

现在您已准备好构建和交付 ⛽

> 注意 团队正在努力简化创建钱包的过程，并消除创建钱包两次的需要。请留意这些更新。
> 

### [修改应用](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#modify-the-app)

在`frontend`文件夹中，让我们添加与我们的合约交互的代码。阅读评论以帮助您了解应用程序部分。

将文件更改`fuel-project/frontend/src/App.tsx`为：

```

import React, { useEffect, useState } from "react";
import { Wallet } from "fuels";
import "./App.css";
// Import the contract factory -- you can find the name in index.ts.
// You can also do command + space and the compiler will suggest the correct name.
import { CounterContractAbi__factory } from "./contracts";
// The address of the contract deployed the Fuel testnet
const CONTRACT_ID = "<YOUR-CONTRACT-ID>";
//the private key from createWallet.js
const WALLET_SECRET = "<YOUR-PRIVATE-KEY>"
// Create a Wallet from given secretKey in this case
// The one we configured at the chainConfig.json
const wallet = new Wallet(WALLET_SECRET, "https://node-beta-1.fuel.network/graphql");
// Connects out Contract instance to the deployed contract
// address using the given wallet.
const contract = CounterContractAbi__factory.connect(CONTRACT_ID, wallet);function App() {
  const [counter, setCounter] = useState(0);
  const [loading, setLoading] = useState(false);
  useEffect(() => {
    async function main() {
      // Executes the counter function to query the current contract state
      // the `.get()` is read-only, because of this it don't expand coins.
      const { value } = await contract.functions.count().get();
      setCounter(Number(value));
    }
    main();
  }, []);async function increment() {
    // a loading state
    setLoading(true);
    // Creates a transactions to call the increment function
    // because it creates a TX and updates the contract state this requires the wallet to have enough coins to cover the costs and also to sign the Transaction
    try {
      await contract.functions.increment().txParams({gasPrice:1}).call();
      const { value } = await contract.functions.count().get();
      setCounter(Number(value));
    } finally {
      setLoading(false);
    }
  }return (
    <div className="App">
      <header className="App-header">
        <p>Counter: {counter}</p>
        <button
          disabled={loading}
          onClick={increment}>
          {loading ? "Incrementing..." : "Increment"}
        </button>
      </header>
    </div>
  );
}export default App;

```

### [运行你的项目](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#run-your-project)

现在是时候在浏览器上运行项目了；

内`fuel-project/frontend`跑;

```

$ npm start
Compiled successfully!
You can now view frontend in the browser.
  Local:            http://localhost:3001
  On Your Network:  http://192.168.4.48:3001
Note that the development build is not optimized.
To create a production build, use npm run build.
```

![https://fuellabs.github.io/fuel-docs/master/images/quickstart-dapp-screenshot.png](https://fuellabs.github.io/fuel-docs/master/images/quickstart-dapp-screenshot.png)

### [✨⛽✨ 恭喜你完成了你的第一个燃料 DApp ✨⛽✨](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#-congrats-you-have-completed-your-first-dapp-on-fuel-)

[这是这个项目的回购](https://github.com/FuelLabs/developer-quickstart)。如果您遇到任何问题，最好的第一步是将您的代码与此 repo 进行比较并解决任何差异。

给我们发推[文@fuellabs_](https://twitter.com/fuellabs_)让我们知道你刚刚在 Fuel 上构建了一个 dapp，你可能会被邀请加入一个私人的建设者小组，被邀请参加下一次 Fuel 晚宴，获得项目的 Alpha 版，或者其他什么 👀。

### [更新合同](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#updating-the-contract)

如果您对合同进行了更改，您应该采取以下步骤使您的前端和合同恢复同步：

- 在你的合约目录中，运行`forc build`
- 在您的合约目录中，通过运行此命令并按照与上述相同的步骤使用您的钱包签署交易来重新部署合约：`forc deploy --url https://node-beta-1.fuel.network/graphql --gas-price 1`
- 在您的前端目录中，重新运行此命令：`npx fuelchain --target=fuels --out-dir=./src/contracts ../counter-contract/out/debug/*-abi.json`
- 在您的前端目录中，更新`App.tsx`
    
    文件中的合同 ID
    

## [需要帮忙？](https://fuellabs.github.io/fuel-docs/master/developer-quickstart.html#need-help)

前往[Fuel discord](https://discord.com/invite/fuelnetwork)寻求帮助。