# 适用于 Windows Server 2008 R2、Windows 7、Windows Server 2012、Windows Server 2012 R2 及 Windows 8.1 的带补丁 Go SDK

该包含补丁的 Go SDK 可本身可以运行于部分到达支持条件的老系统上，并且其编译出来的二进制也可以在受支持的老系统上运行，包含 **Windows Server 2008 R2 SP1 + 便利性汇总更新**、**Windows 7 SP1 + 便利性汇总更新**、**Windows Server 2012 with rollups**、**Windows Server 2012 R2 with April 2014 Update (KB2919355)** 以及 **Windows 8.1 with April 2014 Update (KB2919355)**，仅修复了 [Go](https://github.com/golang/go) 中使其无法在这些操作系统中运行的部分。

该 SDK 可用于构建需要在以上列出的操作系统中运行的 Go 二进制。官方新 SDK 构建的二进制无法在这些操作系统中正常运行。可自由取用该带补丁的 SDK 来构建对应的二进制。

如果需要在 Release 中没有预先构建的 SDK，可分叉后自行构建。

### **警告** （这不是演习）：

这是一个提供 Go SDK 兼容性的项目。但请注意：这既不是也永远不会是一套完美的兼容性解决方案。

旧版操作系统上的一些莫名其妙的 bug 随时可能出现；当然，大多数时候并不会出现。但如果出现了，请记住：其中有些问题本来就应该由 Go 来修复。

我们会在适用条件下履行兼容性承诺。我们会尽力让较旧的操作系统继续运行基本功能，包括编译；但 **CGO 不提供兼容性保证** ， **竞态检测器（race detector）则已知明确不会修复** 。

我们提供兼容性支持，是为了让较新的 Go 能在现有设备上继续运行， **不是为了鼓励你继续使用已经停止支持的操作系统** 。

如果某项功能在旧系统上无法运作，这可能是 bug，也可能只是系统已经太旧了。请不要把前者误认为后者，也不要把后者当成我们的兼容性承诺。

**我们负责让它继续保持基础运行，但不负责让旧系统表现得像新系统一样。** 如果你想要它表现得像在新系统上面一样，请升级到新系统。

### 测试环境

由于 Github Actions 目前没有提供基于 Windows 7 及 Windows 8.1 的 runner，因此所有可运行性测试均使用人工测试。

测试环境：
- Windows 7 SP1 / Windows Server 2008 R2 SP1 (Build 7601.24384) （含 KB3125574 + KB4474419）
- Windows 8.1 with April 2014 Update (KB2919355) / Windows Server 2012 R2 with April 2014 Update (KB2919355) (Build 9600.17415) （未进行更新）
- Windows Server 2012 with required rollups 的兼容性是基于兼容性分析以及定向测试得出，而非主力的人工测试目标。

### 兼容性说明

- **目前该 SDK 编译出的纯 Go 二进制可执行文件能正常在 Windows NT 6.1/6.2/6.3 中运行，在符合条件下的兼容性在项目维护期内可保证。** 如有运行上的问题还请联系。
- 从 Go 1.27 版本起，Windows 7 / Windows Server 2008 R2 的操作系统基准要求更新为：
  - **安装所有在 2016 年 4 月之前通过 Windows Update 发布的更新以及 KB4474419；**
  - **或者安装 KB3020369（至少为 2015 年 4 月版）+ KB3125574 补丁包以及 KB4474419 + KB4490628。**
  - 此次系统要求的变更旨在提升安全性并实现 API 现代化。为了获得更好的安全性和系统功能，**仍建议安装绝大部分更新**，例如针对“永恒之蓝”（EternalBlue）漏洞的修复补丁。
  - *仅安装若干关键更新可能也行得通，但由于上游变动无法保证，可能会出现兼容性漂移。*
  - *一般来说，只要系统能正常运行 Chrome 109 之类的程序，那么这个 SDK 以及由该 SDK 编译的二进制应该可以正常运行。这是非正式的兼容性速查。*
- **Race Detector 自 Go 1.21 开始无法在 Windows 7 上正常使用。** 该问题覆盖面较广需要对所有 1.N (N>20) 版本进行修复，由于该问题报告较晚并且修复方案可能会出现预期外的问题，因此不会再考虑进行修复。

### 标准补丁/过老补丁

本项目提供两层补丁：用于在指定系统基线上即可使用的标准补丁，以及可选的用于未达到系统基线要求的过老补丁。除非另有说明，一般情况下应该优先使用标准补丁。

### `BCryptGenRandom` / `RtlGenRandom`

简史：Go 曾经使用过 `CryptGenRandom`。在 2016 年，Go 将其运行时启动时使用的随机数生成器切换到了 `RtlGenRandom`，以避免 `CryptGenRandom` 所导致的缓慢程序启动，之后又将 `crypto/rand` 相关实现转换到了该 API。在 2019 年至 2022 年期间，随着 Go 不再支持 Windows XP，`BCryptGenRandom` 曾被讨论作为替代方案。基准测试表明，两个 API (`BCryptGenRandom` 和 `RtlGenRandom`) 的性能几乎相同，但这一修改最终没有被接受，Go 团队继续坚持使用他们已经熟悉的实现。2023 年，为了解决与意外 DLL 加载行为相关的安全问题，Go 切换到了 `ProcessPrng`，这意味着 `BCryptGenRandom` 从未真正被任何 Go 版本采用。

当下：实际上，`RtlGenRandom` 存在一个与意外 DLL 加载行为相关的已知安全问题，而 Microsoft 建议改用 CNG (Cryptography Next Generation) API。`BCryptGenRandom` 和 `ProcessPrng` 都属于 CNG。正常使用情况下，`BCryptGenRandom` 的性能影响几乎无法察觉，除非在很短的时间内调用数十亿次；并且它完全受到我们的目标平台支持。经过仔细考虑，我们已经切换到 `BCryptGenRandom`，更新了之前的所有补丁，并重新构建了 Go 软件包，以符合 XTLS 的安全原则。为了保证可复现性，现有已经发布的版本将保持不变，所有更新后的软件包都已经在一个特殊的 release tag 下发布。

## Go 1.21

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`

#### 过老补丁包含附加内容：

- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.22

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
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
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
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
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
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
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
- 修复 `os.RemoveAll` 在旧版本系统上行为异常的问题
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 过老补丁包含附加内容：

- 修复未更新系统需要的 `sysSocket` 回退
- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

## Go 1.26

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2：
  - Go 1.26.9 之前：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。
  - Go 1.26.8 之后：可直接运行官方 Go SDK 及其构建的二进制文件。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 过老补丁包含附加内容：

- 修复未更新系统需要的 `sysSocket` 回退
- 修复未更新系统不认识 `LoadLibraryEx` 所需的新标志的问题

#### 曾经需要但现在不再需要的补丁

- 修复 `os.RemoveAll` 在旧版本系统上行为异常的问题 （在 Go 1.26.9 中通过 `c7183cd20d5859b8ae5ef065dafcc28f75b1cdb3` / CL832805 修复）

## Go 1.27

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
- 修复 PE 头部最小加载版本为 Windows NT 10.0 后的问题
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常

#### 曾经需要但现在不再需要的补丁

- 修复 `os.RemoveAll` 在旧版本系统上行为异常的问题 （在 Go 1.27.2 中通过 `e89d44fee06cacdbc1e9753ee109a35d6a5c377c` / CL832804 修复）

## Go 1.28

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2： 需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。
- Windows 7 / Windows Server 2008 R2：需要在 SDK 中植入补丁，并且只能运行用修补后的 SDK 构建的二进制。

#### 标准补丁包含内容：

- 指示 SDK 只能使用本地版本而不会自动下载其它版本（仅限新部署）
- 移除使用劫持进程环境块开启的长文件名支持（移植自 Microsoft Go）
- 修复 PE 头部最小加载版本为 Windows NT 10.0 后的问题
- 使用 `BCryptGenRandom` 替换 `ProcessPrng`
- 修复因移除针对旧版 Windows 控制台的额外措施导致可能发生的控制台异常
