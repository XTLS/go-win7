# Patched Go SDK for Windows Server 2008 R2, Windows 7, Windows Server 2012, Windows Server 2012 R2 and Windows 8.1

This is a patched Go SDK that allows the Go toolchain to run on legacy Windows versions and allows Go binaries built with the patched SDK to target those systems, including **Windows Server 2008 R2 SP1 + Convenience Rollup**, **Windows 7 SP1 + Convenience Rollup**, **Windows Server 2012 with rollups**, **Windows Server 2012 R2 with April 2014 Update (KB2919355)**, and **Windows 8.1 with April 2014 Update (KB2919355)**. Only fixed some incompatible code that will break running on these operating systems from [Go](https://github.com/golang/go).

This SDK is used for building binaries that can run on listed OSes that is not supported officially by Go now. You can use it freely to build binaries targeting these listed OSes from Go.

If you need other pre-built SDK binaries that are not found in Release, you may fork and build it.

### **WARNING** (This is not a drill):

This is a repository for Go SDK compatibility. However, this is not, and will never be, a perfect compatibility solution.

Some strange and seemingly inexplicable bugs may appear on older operating systems at any time, of course, most of the time they will not. If one does appear, however, please remember that some of these issues are things that should really be fixed by Go itself.

We will honor our compatibility commitments, subject to applicable conditions. We will do our best to keep basic functionality working on older operating systems, including building the SDK and compiling a Go binary. However, **CGO compatibility is not guaranteed**, and **the race detector is known not to work and will not be fixed**.

We provide compatibility support so that existing systems can keep running a newer Go than Go expected. **This is not an encouragement to continue using an unsupported operating system.**

If something does not work on an older system, it may be a bug, or it may simply be because the system is old. Please do not mistake the former for the latter, and do not mistake the latter for a compatibility guarantee.

**We are here to keep it working, not to make an old OS feel like a new OS.** If you want it to work like on a new system, please upgrade to a new system.

### Testing environment

Testing on legacy Windows versions is performed manually because GitHub Actions does not provide Windows 7 or Windows 8.1 hosted runners.

Testing environment:
- Windows 7 SP1 / Windows Server 2008 R2 SP1 (Build 7601.24384) (with KB3125574 and KB4474419)
- Windows 8.1 with April 2014 Update (KB2919355) / Windows Server 2012 R2 with April 2014 Update (KB2919355) (Build 9600.17415) (with no other updates installed)
- Windows Server 2012 with required rollups is supported based on compatibility analysis and targeted testing, but is not currently part of the primary manual test matrix.

### Compatibilities

- **Pure-Go binaries compiled with this SDK are expected to run on Windows NT 6.1/6.2/6.3, and this compatibility is part of the project's maintenance commitment.** Contact us if there are issues when running these executables on supported platforms.
- Starting with Go 1.27, the OS baseline for Windows 7 / Windows Server 2008 R2 is:
  - **All updates from Windows Update released before April 2016 and KB4474419;**
  - **or install KB3020369 (minimum April 2015) + KB3125574 and KB4474419 + KB4490628**
  - This change of system requirement is for both security and API modernization. **Installing additional updates is still required** for better security and system functionality, like update for blocking EternalBlue.
  - *Only installing several key updates may also work, but compatibility may drift due to not-guaranteed upstream changes.*
  - *If the system can run Chrome 109 normally, the SDK and binaries compiled from the SDK should be running normally. This is an informal compatibility indicator*
- **Race Detector does not work on Windows 7 since Go 1.21.** This is a widespread problem which needs fixing for all Go 1.N (N>20). Due to the late report, and side-effects may occur after the fixing, this issue will not be fixed.

### Standard patches / Legacy patches

This project provides two patch layers. The standard patch contains changes required for the supported baseline systems. The legacy patch is an optional compatibility layer for systems that lack additional legacy APIs or behavior. Unless otherwise stated, the standard patch is the recommended configuration.

### `BCryptGenRandom` / `RtlGenRandom`

Short history: Go once used `CryptGenRandom`. In 2016, Go switched its runtime startup RNG to `RtlGenRandom` to avoid slow program startups caused by `CryptGenRandom`, and later transitioned `crypto/rand` to it as well. Between 2019 and 2022, `BCryptGenRandom` was discussed after Go dropped support for Windows XP. Benchmarks indicated that both APIs (`BCryptGenRandom` and `RtlGenRandom`) had almost identical performance, but the change was never accepted, with the Go team continuing to stick with the implementation they were already familiar with. In 2023, to address a security issue related to unexpected DLL loading behaviour, Go switched to `ProcessPrng`, meaning `BCryptGenRandom` was never actually adopted by any Go release.

Current situation: In reality, `RtlGenRandom` has a known security issue related to unexpected DLL loading behaviour, and Microsoft recommends using CNG (Cryptography Next Generation) APIs instead. Both `BCryptGenRandom` and `ProcessPrng` are part of CNG. The performance impact of `BCryptGenRandom` is hardly noticeable in normal usage unless it is called billions of times in a short burst, and it is fully supported by our target platforms. After careful consideration, we have switched to `BCryptGenRandom`, updated all previous patches, and rebuilt the Go packages to align with the security principles of XTLS. For reproducibility, existing releases will remain unchanged, and all refreshed packages have been published under a special release tag.

## Go 1.21

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one (new deployment only)
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`

#### Patches included in legacy patch as add-on:

- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.22

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one (new deployment only)
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.23

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.24

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one (new deployment only)
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.25

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Require patches in SDK, and binaries must be built with patched SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one (new deployment only)
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Fix `os.RemoveAll` failing on legacy Windows
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.26

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2:
  - Before Go 1.26.9: Require patches in SDK, and binaries must be built with patched SDK.
  - After Go 1.26.8: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one (new deployment only)
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

#### Patches was introduced but removed

- Fix `os.RemoveAll` failing on legacy Windows (Fixed by `c7183cd20d5859b8ae5ef065dafcc28f75b1cdb3` / CL832805 in Go 1.26.9)

## Go 1.27

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Require patches in SDK, and binaries must be built with patched SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one (new deployment only)
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Fix PE header locking on minimum Windows NT 10.0
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches was introduced but removed

- Fix `os.RemoveAll` failing on legacy Windows (Fixed by `e89d44fee06cacdbc1e9753ee109a35d6a5c377c` / CL832804 in Go 1.27.2)

## Go 1.28

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Require patches in SDK, and binaries must be built with patched SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Lock SDK to a local one and never download a different one (new deployment only)
- Remove PEB hack for long path support (adapted from Microsoft Go)
- Fix PE header locking on minimum Windows NT 10.0
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows
