# Patched Go SDK for Windows Server 2008 R2, Windows 7, Windows Server 2012, Windows Server 2012 R2 and Windows 8.1

This is a patched Go SDK that allows the Go toolchain to run on legacy Windows versions and allows Go binaries built with the patched SDK to target those systems, including **Windows Server 2008 R2 SP1 + Convenience Rollup**, **Windows 7 SP1 + Convenience Rollup**, **Windows Server 2012 with rollups**, **Windows Server 2012 R2 with April 2014 Update (KB2919355)**, and **Windows 8.1 with April 2014 Update (KB2919355)**. Only fixed some incompatible code from [Go](https://github.com/golang/go).

If you need other pre-built binaries that are not found in Release, you can fork and build it freely. For quick download entrance, see [here](#quick-download)

## **WARNING** (This is not a drill):

This is a repository for Go SDK compatibility. However, this is not, and will never be, a perfect compatibility solution.

Some strange and seemingly inexplicable bugs may appear on older operating systems at any time, of course, most of the time they will not. If one does appear, however, please remember that some of these issues are things that should really be fixed by Go itself.

We will honor our compatibility commitments, subject to applicable conditions. We will do our best to keep basic functionality working on older operating systems, including building the SDK and compiling a Go binary. However, **CGO compatibility is not guaranteed**, and **the race detector is known not to work and will not be fixed**.

We provide compatibility support so that existing systems can keep running a newer Go than Go expected. **This is not an encouragement to continue using an unsupported operating system.**

If something does not work on an older system, it may be a bug, or it may simply be because the system is old. Please do not mistake the former for the latter, and do not mistake the latter for a compatibility guarantee.

**We are here to keep it working, not to make an old OS feel like a new OS.** If you want it to work like on a new system, please upgrade to a new system.

We will periodically check whether the patches meet our security model. If you think we are just making unnecessary trouble for everyone, well, <span title="I in malam crucem!">so be it and bear with us</span>, because we strive to maintain code security and our own sanity.

## Compatibility

### Supported operating Systems & Minimum update requirements

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
- Windows Server 2012 with required rollups
- Windows 8.1 with April 2014 Update (KB2919355)
- Windows Server 2012 R2 with April 2014 Update (KB2919355)

The decision is based on both security concern and API modernization. Even though the updates mentioned are the baselines, you still need to install additional updates to keep better functionality and security for the OS, especially preventing known attacks in wild for all OSes. Installing several key updates may also work, but compatibility may drift when upstream making changes.

### Legacy-system notes

If the machine with older OS is used for special purpose(s) and cannot / not suitable to install updates, it is not recommended to use newer SDKs or binaries built by newer SDKs on the machine, or unexpected problems may raise. This type of machine may also have problems running complex software released in circa 2019-2023, or by other newer SDKs/compilers. For these kinds of machines running in offline environment, it is better to pin a known working version of SDK, no matter the programming language is.

### Recommended updates

Updates below are recommended besides the baseline requirements:

- KB3140245 (Windows Server 2012, Windows Server 2008 R2 SP1, and Windows 7 SP1): Providing support for TLS 1.1 and 1.2 in system SChannel, which is used by many system components. You may need to use Easy fix from Microsoft to enable TLS 1.2 support correctly which update several registry keys.
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

For more details, read XTLS/go-win7#16 .

## Quick download

To get the latest release in maintenance state, click [here](https://github.com/XTLS/go-win7/releases/latest)

To get the real-time updated package archive of every major version currently maintaining, click [here](https://github.com/XTLS/go-win7/releases/tag/current)

To get the real-time updated package archive of every out-of-maintenance major version, click [here](https://github.com/XTLS/go-win7/releases/tag/archive)
