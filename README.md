# Patched Go SDK for Windows Server 2008 R2, Windows 7, Windows Server 2012, Windows Server 2012 R2 and Windows 8.1

The Go SDK with patches that can run on **Windows Server 2008 R2 SP1 + Convenience Rollup**, **Windows 7 SP1 + Convenience Rollup**, **Windows Server 2012 with rollups**, **Windows Server 2012 R2 with update**, and **Windows 8.1 with Update 3**. Only fixed some incompatible code from [Go](https://github.com/golang/go).

If you need other pre-built binaries that does not found in Release, you can fork and build it freely.

## Compatibility

For Go 1.21 - 1.26, the baseline of the compatibility is:

- Windows 7 (better with updates installed)
- Windows Server 2008 R2 (better with updates installed)

Since Go 1.27, the baselines of the compatibility are:

- Windows 7 SP1
  - All updates from Windows Update released before April 2016 and KB4474419 + KB4490628;
  - or install KB3020369 (minimum April 2015) + KB3125574 and KB4474419 + KB4490628
- Windows Server 2008 R2
  - All updates from Windows Update released before April 2016 and KB4474419 + KB4490628;
  - or install KB3020369 (minimum April 2015) + KB3125574 and KB4474419 + KB4490628
- Windows Server 2012 with rollups
- Windows 8.1 with Update 3
- Windows Server 2012 R2 with update

The decision is based on both security concern and API mordernization. Even though the the updates metioned are the baselines, you still need to install additional updates to keep better functionality and security for the OS, especially preventing known attacks in wild for all OSes. Installing several key updates may also work, but compatibility may drift when upstream making changes.

If the machine with older OS is used for special purpose(s) and cannot / not suitable to install updates, it is not recommended to use newer SDKs or binaries built by newer SDKs on the machine, or unexpected problems may raise. This type of machine may also have problems running software released in circa 2019-2023, or by other newer SDKs/compilers.

Updates below are recommended besides the baseline requirements:

- KB3140245 (Windows Server 2012, Windows Server 2008 R2 SP1, and Windows 7 SP1): Providing support for TLS 1.1 and 1.2 in system SChannel, which is used by many system components. You may need to use Easy fix from Microsoft to enable TLS 1.2 support correctly which update several registery keys.
- .NET Framework updates: This enable correct TLS 1.2 support in .NET Framework applications correctly in older OSes.
  - .NET Framework 4.5.1 or 4.5.2 (Windows Server 2012 R2, Windows Server 2012, Windows 8.1): Install at least latest updates for 4.5.1 or 4.5.2 as a baseline.
  - .NET Framework 4.6.2 (Windows Server 2008 R2 SP1, Windows 7 SP1): TLS 1.2 is correctly supported in .NET Framework since 4.6.2.

For detailed compatibility notes, you can find in the detailed ReadMe.

Detailed ReadMe / 详细说明文件:

- [English](./README-eng.md)
- [中文（简体）](./README-zho-hans.md)

## Supported version

Current support status: 1.27.x (main), 1.26.x (check)

Possible final target: 1.30.x

For more details, read #16 .
