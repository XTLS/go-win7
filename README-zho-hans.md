# 适用于 Windows Server 2008 R2、Windows 7、Windows Server 2012、Windows Server 2012 R2 及 Windows 8.1 的带补丁 Go SDK

该包含补丁的 Go SDK 可运行于 **Windows Server 2008 R2 SP1 + 便利性汇总更新**、**Windows 7 SP1 + 便利性汇总更新**、**Windows Server 2012 SP2**、**Windows Server 2012 R2 with update** 以及 **Windows 8.1 with update 3**，仅修复了 [Go](https://github.com/golang/go) 中使其无法在这些操作系统中运行的部分。

该 SDK 可用于构建需要在以上列出的操作系统中运行的 Go 二进制。官方新 SDK 构建的二进制无法在这些操作系统中正常运行。可自由取用该带补丁的 SDK 来构建对应的二进制。

如果需要在 Release 中没有预先构建的 SDK，可分叉后自行构建。

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

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（来自 Microsoft Go）
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`

#### 过老补丁包含附加内容：

- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.22

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（来自 Microsoft Go）
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 过老补丁包含附加内容：

- 修复未更新系统需要的 `sysSocket` 回退
- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.23

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（来自 Microsoft Go）
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 过老补丁包含附加内容：

- 修复未更新系统需要的 `sysSocket` 回退
- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.24

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（来自 Microsoft Go）
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 过老补丁包含附加内容：

- 修复未更新系统需要的 `sysSocket` 回退
- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.25

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（来自 Microsoft Go）
- 修复 `os.RemoveAll` 在旧版本系统上行为异常的问题
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 过老补丁包含附加内容：

- 修复未更新系统需要的 `sysSocket` 回退
- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.26

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（来自 Microsoft Go）
- 修复 `os.RemoveAll` 在旧版本系统上行为异常的问题
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 过老补丁包含附加内容：

- 修复未更新系统需要的 `sysSocket` 回退
- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.27

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（来自 Microsoft Go）
- 修复 PE 头部最小加载版本为 Windows NT 10.0 后的问题
- 修复 `os.RemoveAll` 在旧版本系统上行为异常的问题
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常
