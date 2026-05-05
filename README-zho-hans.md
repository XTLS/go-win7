# 适用于 Windows Server 2008 R2、Windows 7、Windows Server 2012、Windows Server 2012 R2 及 Windows 8.1 的带补丁 Go SDK

该包含补丁的 Go SDK 可运行于 **Windows Server 2008 R2 SP1 + 便利性汇总更新**、**Windows 7 SP1 + 便利性汇总更新**、**Windows Server 2012 SP2**、**Windows Server 2012 R2 with update** 以及 **Windows 8.1 with update 3**，仅回滚了 [Go](https://github.com/golang/go) 中使其无法在这些操作系统中运行的部分。

该 SDK 可用于构建需要在以上列出的操作系统中运行的 Go 二进制。官方新 SDK 构建的二进制无法在这些操作系统中正常运行。可自由取用该带补丁的 SDK 来构建对应的二进制。

如果需要在 Release 中没有预先构建的 SDK，可分叉后自行构建。

## 状态表

目前已知 Go SDK 中这些更改会导致 SDK 自身及构建后的二进制在已经列出的操作系统中无法正常运行：

- `a17d959debdb04cd550016a3501dd09d50cd62e7` (`runtime: always use LoadLibraryEx to load system libraries`) （影响旧版未更新 Windows 7，具体见下）
- `7c1157f9544922e96945196b47b95664b1e39108` (`net: remove sysSocket fallback for Windows 7`)
- `48042aa09c2f878c4faa576948b07fe625c4707a` (`syscall: remove Windows 7 console handle workaround`)
- `693def151adff1af707d82d28f55dba81ceb08e1` (`crypto/rand,runtime: switch RtlGenRandom for ProcessPrng`)
- `534d6a1a9c81b25bdad1052e736b2f072caa3903` (`crypto/rand: prevent Read argument from escaping to heap`) （导致应对 `crypto/rand,runtime: switch RtlGenRandom for ProcessPrng` 的补丁故障）
- `6d418096b2dfe2a2e47b7aa83b46748fb301e6cb` (`os: avoid symlink races in RemoveAll on Windows`)
- `896097000912761dbd31cead2bec99f17534f521` (`os: add Root.RemoveAll`)

### 测试环境

由于 Github Actions 目前没有 Windows 7 及 Windows 8 的 runner，因此所有可运行性测试均使用人工测试。

测试环境：
- Windows 7 SP1 / Windows Server 2008 R2 SP1 (Build 7601.17514) （含 KB3125574 + KB4474419）
- Windows 8.1 Update 3 / Windows Server 2012 R2 with update (Build 9600.17514) （未进行更新）

### 兼容性说明

- **目前该 SDK 编译出的二进制可执行文件能正常在 Windows NT 6.1/6.2/6.3 中运行，这一点在项目维护期内可保证。** 如有运行上的问题还请联系。
- 从 Go 1.27 版本起，Windows 7 / Windows Server 2008 R2 的操作系统基准要求更新为：
  - **安装所有在 2016 年 4 月之前通过 Windows Update 发布的更新以及 KB4474419；**
  - **或者安装 KB3020369（至少为 2015 年 4 月版）+ KB3125574 补丁包以及 KB4474419 + KB4490628。**
  - 此次系统要求的变更旨在提升安全性并实现 API 现代化。为了获得更好的安全性和系统功能，**仍建议安装绝大部分更新**，例如针对“永恒之蓝”（EternalBlue）漏洞的修复补丁。
  - *仅安装若干关键更新可能也行得通，但由于上游变动无法保证，可能会出现兼容性漂移。*
  - *一般来说，只要系统能正常运行 Chrome 109 之类的程序，那么这个 SDK 以及由该 SDK 编译的二进制应该可以正常运行。*
- **Race Detector 自 Go 1.21 开始无法在 Windows 7 上正常使用。** 该问题覆盖面较广需要对所有 1.N (N>20) 版本进行修复，由于该问题报告较晚并且修复方案可能会出现预期外的问题，因此不会再考虑进行修复。

## Go 1.21

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2 SP1：
  - Go 1.21rc1 ~ Go 1.21.4：需要系统安装编号为 KB4474419（SHA-2 代码签名支持更新）的更新。对有互联网连接的机器，推荐同时安装编号为 KB4490628（服务堆栈更新）的更新来获取后续安全更新。
  - Go 1.21.5 及以上版本：因为 crypto 包中对系统 API 调用的调整，无法运行官方 SDK 及用官方 SDK 构建的二进制。

## Go 1.22

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988 / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1 / [Git diff](https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1.diff)
1. https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6 / [Git diff](https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6.diff)
1. https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0 / [Git diff](https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0.diff)

## Go 1.23

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/429f9a72007759a757c8e96a2763306c076dbb8f / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8 / [Git diff](https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8.diff)
1. https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1 / [Git diff](https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1.diff)
1. https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5 / [Git diff](https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5.diff)

## Go 1.24

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 SP1 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0 / [Git diff](https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0.diff)
1. https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1 / [Git diff](https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1.diff)
1. https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c / [Git diff](https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c.diff)
1. https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c / [Git diff](https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c.diff)
1. https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d / [Git diff](https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d.diff)

## Go 1.25

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update： 可直接运行官方 Go SDK 及其构建的二进制文件，但是可能会在文件删除操作上出现问题，因此纳入本仓库管理范围。
- Windows 7 SP1 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

##### 注意：由于上游更改，1.25.0-1.25.7 与 1.25.8 及更新的版本不使用同一个补丁。以下为 1.25.0-1.25.7 使用的补丁列表。

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/0731a1bffeb285ee576629452e095bf833862b9b / [Git diff](https://github.com/XTLS/go-win7/commit/0731a1bffeb285ee576629452e095bf833862b9b.diff)
1. https://github.com/XTLS/go-win7/commit/830f1acfc984be44520621b001096845ebf40c7a / [Git diff](https://github.com/XTLS/go-win7/commit/830f1acfc984be44520621b001096845ebf40c7a.diff)
1. https://github.com/XTLS/go-win7/commit/b5e4a6d5b3d0b076414d04cc3d6002f816bc0c25 / [Git diff](https://github.com/XTLS/go-win7/commit/b5e4a6d5b3d0b076414d04cc3d6002f816bc0c25.diff)
1. https://github.com/XTLS/go-win7/commit/949393bde276adbeaf41688f086feb23e24abe88 / [Git diff](https://github.com/XTLS/go-win7/commit/949393bde276adbeaf41688f086feb23e24abe88.diff)
1. https://github.com/XTLS/go-win7/commit/fc29c4ae1cd53d6761d4324c4625cc8a149c55d3 / [Git diff](https://github.com/XTLS/go-win7/commit/fc29c4ae1cd53d6761d4324c4625cc8a149c55d3.diff)

## Go 1.26

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update： 可直接运行官方 Go SDK 及其构建的二进制文件，但是可能会在文件删除操作上出现问题，因此纳入本仓库管理范围。
- Windows 7 SP1 / Windows Server 2008 R2：需要系统已安装 KB2533623，SDK 中已植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows 7 / Windows Server 2008 R2 的补丁

这些补丁必须按照从上到下的顺序来修补：

1. https://github.com/XTLS/go-win7/commit/86eda38dabd1753092f64de7dcd2050d346c89b4 / [Git diff](https://github.com/XTLS/go-win7/commit/86eda38dabd1753092f64de7dcd2050d346c89b4.diff)
1. https://github.com/XTLS/go-win7/commit/c3e5e430625ba7ab5d638c753f94cc52253793f5 / [Git diff](https://github.com/XTLS/go-win7/commit/c3e5e430625ba7ab5d638c753f94cc52253793f5.diff)
1. https://github.com/XTLS/go-win7/commit/0d36c60d2c0754fde7e4e7e4773e0349527cff08 / [Git diff](https://github.com/XTLS/go-win7/commit/0d36c60d2c0754fde7e4e7e4773e0349527cff08.diff)

对于没有安装 KB2533623 或 KB3125574 的 Windows 7 SP1/Windows Server 2008 R2 SP1，应该同时添加以下补丁：

1. https://github.com/XTLS/go-win7/raw/refs/heads/build/pre-KB3125574-1-26.diff

对于没有安装 SP1 的 Windows 7/Windows Server 2008 R2，应该同时添加以下补丁：

1. https://github.com/XTLS/go-win7/raw/refs/heads/build/pre-SP1-1-26.diff

## Go 1.27

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update： 需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。
- Windows 7 SP1 / Windows Server 2008 R2：需要系统已安装 KB2533623 以及 KB4474419，SDK 中已植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 用于 Windows Server 2008 R2 SP1+ / Windows 7 SP1+ / Windows Server 2012 SP2 / Windows Server 2012 R2 with update / Windows 8.1 with update 3 的补丁
