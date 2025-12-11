# Patched Go SDK for Windows 7

The Go SDK with patches that can run on Windows 7. Only reverted some commits that will break running on Windows 7 from [Go](https://github.com/golang/go).

This SDK is used for building binaries that can run on Windows 7 that does not supported officially by Go now. You can use it freely to build binaries targeting Windows 7 from Go.

If you need other pre-built SDK binaries that does not found in Release, you may fork and build it.

## Status

Currently found these changes in Go SDK will stop running Windows 7:

- `a17d959debdb04cd550016a3501dd09d50cd62e7` (`runtime: always use LoadLibraryEx to load system libraries`) (affecting older builds, details in below section)
- `7c1157f9544922e96945196b47b95664b1e39108` (`net: remove sysSocket fallback for Windows 7`)
- `48042aa09c2f878c4faa576948b07fe625c4707a` (`syscall: remove Windows 7 console handle workaround`)
- `693def151adff1af707d82d28f55dba81ceb08e1` (`crypto/rand,runtime: switch RtlGenRandom for ProcessPrng`)
- `534d6a1a9c81b25bdad1052e736b2f072caa3903` (`crypto/rand: prevent Read argument from escaping to heap`) (breaking previous patch on `crypto/rand,runtime: switch RtlGenRandom for ProcessPrng`)
- `6d418096b2dfe2a2e47b7aa83b46748fb301e6cb` (`os: avoid symlink races in RemoveAll on Windows`)
- `896097000912761dbd31cead2bec99f17534f521` (`os: add Root.RemoveAll`)

### Testing environment

All running tests are under manual operation due to there are no runners based on Windows 7 and Windows 8 in Github Actions.

Testing environment:
- Windows 7 SP1 / Windows Server 2008 R2 SP1 (Build 7601.17514) (with no other updates installed)
- Windows 8.1 Update 3 / Windows Server 2012 R2 SP1 (Build 9600.17514) (with no other updates installed)

### Compatibilities

- **The binary executables compiled by this SDK can run normally on Windows 7 (and Windows 8.1). This is guaranteed during the maintenance of the project.** Contact us if there are issues when running these executables on Windows 7 & 8.1.
- **Race Detector does not work on Windows 7 since Go 1.21.** This is a widespread problem which needs fixing for all Go 1.N (N>20). Due to the late report, and side-effects may occur after the fixing, this issue will not be fixed during the maintenance. Whether there are SDK releases with fixing on this issue after the sunsetting on the project needs discussions.
- Whether the SDK runs compiling jobs on Windows 7: **Theoretically yes, actually it depends on the installed OS environment running on**
  - Microsoft made functional changes to Windows throught Windows updates before Windows 10. This also changed the software compatibility of the OSes. For Windows 7, major updates that came with compatibility changes includes: *Service Pack 1* , *Platform Update for Windows 7 (KB2670838)* , *Windows Management Framework 5.0* and *SHA-2 Code Signing Support Update (KB4474419)* .
  - The Windows 7 testing environment only has Service Pack 1 installed to prevent inconsistant results of compatibility tests. The bootstrapping compiling of the SDK was never success in the testing environment (Windows 7), and it cannot compile a project with libraries imported outside of standard libraries due to no network connection for security concerns. So whether the SDK can run the compiling job on Windows 7 is unclear.
  - Currently the boostrapping compiling test and project compiling test run on Windows 8.1 Update 3 / Windows Server 2012 R2 SP1. Theoretically the SDK can run compile jobs smoothly on Windows 7 which had installed most of the updates (at least PowerShell needs to be updated).
  - If issues occur during compiling, which compiling jobs running on Windows 7, you may consider: compile the executables on newer OSes (Windows, macOS, Linux, etc.) then copy the executables to Windows 7 running machines through networks or storage devices.

## Go 1.21

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2 SP1:
  - Go 1.21rc1 ~ Go 1.21.4: Require update KB4474419 (SHA-2 code signing support update) installed. For computers that have Internet access, it is recommended to install KB4490628 (Servicing stack update) to install subsequent security update.
  - Go 1.21.5 and onward: Cannot run official distributed SDK or binaries built from official SDK, because of a change of crypto API in SDK.

## Go 1.22

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988 / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1 / [Git diff](https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1.diff)
1. https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6 / [Git diff](https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6.diff)
1. https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0 / [Git diff](https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0.diff)

## Go 1.23

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/429f9a72007759a757c8e96a2763306c076dbb8f / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8 / [Git diff](https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8.diff)
1. https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1 / [Git diff](https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1.diff)
1. https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5 / [Git diff](https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5.diff)

## Go 1.24

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0 / [Git diff](https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0.diff)
1. https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1 / [Git diff](https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1.diff)
1. https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c / [Git diff](https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c.diff)
1. https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c / [Git diff](https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c.diff)
1. https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d / [Git diff](https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d.diff)

## Go 1.25

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK, but a problem may occur when removing files, so now it is also in the range of this repository.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/0731a1bffeb285ee576629452e095bf833862b9b / [Git diff](https://github.com/XTLS/go-win7/commit/0731a1bffeb285ee576629452e095bf833862b9b.diff)
1. https://github.com/XTLS/go-win7/commit/830f1acfc984be44520621b001096845ebf40c7a / [Git diff](https://github.com/XTLS/go-win7/commit/830f1acfc984be44520621b001096845ebf40c7a.diff)
1. https://github.com/XTLS/go-win7/commit/b5e4a6d5b3d0b076414d04cc3d6002f816bc0c25 / [Git diff](https://github.com/XTLS/go-win7/commit/b5e4a6d5b3d0b076414d04cc3d6002f816bc0c25.diff)
1. https://github.com/XTLS/go-win7/commit/949393bde276adbeaf41688f086feb23e24abe88 / [Git diff](https://github.com/XTLS/go-win7/commit/949393bde276adbeaf41688f086feb23e24abe88.diff)
1. https://github.com/XTLS/go-win7/commit/fc29c4ae1cd53d6761d4324c4625cc8a149c55d3 / [Git diff](https://github.com/XTLS/go-win7/commit/fc29c4ae1cd53d6761d4324c4625cc8a149c55d3.diff)
