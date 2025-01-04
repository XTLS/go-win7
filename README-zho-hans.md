# 适用于 Windows 7 的带补丁 Go 工具链

该包含补丁的 Go 工具链可运行于 Windows 7 中，仅回滚了 [Go](https://github.com/golang/go) 中使其无法在 Windows 7 中的部分。

该工具链可用于构建需要在 Windows 7 中运行的 Go 二进制。官方新 SDK 构建的二进制无法在 Windows 7 中正常运行。可自由取用该带补丁的 SDK 来构建对应的二进制。

如果需要不在 Release 中没有的预先构建的 SDK，可分叉后自行构建。

## 状态表

目前已知 Go SDK 中这些更改会导致工具链自身及构建后的二进制在 Windows 7 中无法正常运行：

- `a17d959debdb04cd550016a3501dd09d50cd62e7` (`runtime: always use LoadLibraryEx to load system libraries`) （影响旧版未更新 Windows 7）
- `7c1157f9544922e96945196b47b95664b1e39108` (`net: remove sysSocket fallback for Windows 7`)
- `48042aa09c2f878c4faa576948b07fe625c4707a` (`syscall: remove Windows 7 console handle workaround`)
- `693def151adff1af707d82d28f55dba81ceb08e1` (`crypto/rand,runtime: switch RtlGenRandom for ProcessPrng`)
- `534d6a1a9c81b25bdad1052e736b2f072caa3903` (`crypto/rand: prevent Read argument from escaping to heap`) （导致应对 `crypto/rand,runtime: switch RtlGenRandom for ProcessPrng` 的补丁故障）

### 测试环境

由于 Github Actions 目前没有 Windows 7 及 Windows 8 的 runner，因此所有可运行性测试均使用人工测试。

测试环境使用：
- Windows 7 SP1 （未进行更新）
- Windows 8.1 Update 3（未进行更新）

### Go 1.21

- Windows 8.1 Update 3 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2 SP1：
  - Go 1.21rc1 ~ Go 1.21.4：需要系统安装编号为 KB4474419（SHA-2 代码签名支持更新）的更新。对有互联网连接的机器，推荐同时安装编号为 KB4490628（服务堆栈更新）的更新来获取后续安全更新。
  - Go 1.21.5 及以上版本：因为 crypto 包中对系统 API 调用的调整，无法运行官方 SDK 及用官方 SDK 构建的二进制。

### Go 1.22

- Windows 8.1 Update 3 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988 / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1 / [Git diff](https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1.diff)
1. https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6 / [Git diff](https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6.diff)
1. https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0 / [Git diff](https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0.diff)

### Go 1.23

- Windows 8.1 Update 3 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/429f9a72007759a757c8e96a2763306c076dbb8f / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8 / [Git diff](https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8.diff)
1. https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1 / [Git diff](https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1.diff)
1. https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5 / [Git diff](https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5.diff)

### Go 1.24 （仍在测试，后续有可能会发生变化）

- Windows 8.1 Update 3 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0 / [Git diff](https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0.diff)
1. https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1 / [Git diff](https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1.diff)
1. https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c / [Git diff](https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c.diff)
1. https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c / [Git diff](https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c.diff)
1. https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d / [Git diff](https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d.diff)
