# Patched Go SDK for Windows Server 2008 R2, Windows 7, Windows Server 2012, Windows Server 2012 R2 and Windows 8.1

The Go SDK with patches that can run on **Windows Server 2008 R2 SP1 + Convenience Rollup**, **Windows 7 SP1 + Convenience Rollup**, **Windows Server 2012 SP2**, **Windows Server 2012 R2 with update**, and **Windows 8.1 with update 3**. Only reverted some commits that will break running on these operating systems from [Go](https://github.com/golang/go).

This SDK is used for building binaries that can run on listed OSes that does not supported officially by Go now. You can use it freely to build binaries targeting these listed OSes from Go.

If you need other pre-built SDK binaries that does not found in Release, you may fork and build it.

## Status

Currently found these changes in Go SDK will stop running on listed OSes:

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
- Windows 7 SP1 / Windows Server 2008 R2 SP1 (Build 7601.17514) (with KB3125574 and KB4474419)
- Windows 8.1 Update 3 / Windows Server 2012 R2 with update (Build 9600.17514) (with no other updates installed)

### Compatibilities

- **The binary executables compiled by this SDK can run normally on Windows NT 6.1/6.2/6.3. This is guaranteed during the maintenance of the project.** Contact us if there are issues when running these executables on Windows 7 & 8.1.
- Since Go 1.27, the OS baseline for Windows 7 / Windows Server 2008 R2 changes to:
  - **All updates from Windows Update released before April 2016 and KB4474419;**
  - **or install KB3020369 (minimum April 2015) + KB3125574 and KB4474419 + KB4490628**
  - This change of system requirement is for both security and API modernization. **Installing additional updates is still required** for better security and system functionality, like update for blocking EternalBlue.
  - *Only installing several key updates may also work, but compatibility may drift due to not-guaranteed upsteam changes.*
  - *If the system can run Chrome 109 normally, the SDK and binaries compiled from the SDK should be running normally.*
- **Race Detector does not work on Windows 7 since Go 1.21.** This is a widespread problem which needs fixing for all Go 1.N (N>20). Due to the late report, and side-effects may occur after the fixing, this issue will not be fixed.

## Go 1.21

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2 SP1:
  - Go 1.21rc1 ~ Go 1.21.4: Require update KB4474419 (SHA-2 code signing support update) installed. For computers that have Internet access, it is recommended to install KB4490628 (Servicing stack update) to install subsequent security update.
  - Go 1.21.5 and onward: Cannot run official distributed SDK or binaries built from official SDK, because of a change of crypto API in SDK.

## Go 1.22

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988 / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1 / [Git diff](https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1.diff)
1. https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6 / [Git diff](https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6.diff)
1. https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0 / [Git diff](https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0.diff)

## Go 1.23

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/429f9a72007759a757c8e96a2763306c076dbb8f / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8 / [Git diff](https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8.diff)
1. https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1 / [Git diff](https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1.diff)
1. https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5 / [Git diff](https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5.diff)

## Go 1.24

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0 / [Git diff](https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0.diff)
1. https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1 / [Git diff](https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1.diff)
1. https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c / [Git diff](https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c.diff)
1. https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c / [Git diff](https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c.diff)
1. https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d / [Git diff](https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d.diff)

## Go 1.25

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update: Can run official distributed Go SDK and binaries built from official SDK, but a problem may occur when removing files, so now it is also in the range of this repository.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

##### NOTE: Due to changes and adjustments from upstream, 1.25.0-1.25.7 dost not share the same patch with 1.25.8 and onward. Below is a list of patches for 1.25.0-1.25.7.

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/0731a1bffeb285ee576629452e095bf833862b9b / [Git diff](https://github.com/XTLS/go-win7/commit/0731a1bffeb285ee576629452e095bf833862b9b.diff)
1. https://github.com/XTLS/go-win7/commit/830f1acfc984be44520621b001096845ebf40c7a / [Git diff](https://github.com/XTLS/go-win7/commit/830f1acfc984be44520621b001096845ebf40c7a.diff)
1. https://github.com/XTLS/go-win7/commit/b5e4a6d5b3d0b076414d04cc3d6002f816bc0c25 / [Git diff](https://github.com/XTLS/go-win7/commit/b5e4a6d5b3d0b076414d04cc3d6002f816bc0c25.diff)
1. https://github.com/XTLS/go-win7/commit/949393bde276adbeaf41688f086feb23e24abe88 / [Git diff](https://github.com/XTLS/go-win7/commit/949393bde276adbeaf41688f086feb23e24abe88.diff)
1. https://github.com/XTLS/go-win7/commit/fc29c4ae1cd53d6761d4324c4625cc8a149c55d3 / [Git diff](https://github.com/XTLS/go-win7/commit/fc29c4ae1cd53d6761d4324c4625cc8a149c55d3.diff)

## Go 1.26

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update: Can run official distributed Go SDK and binaries built from official SDK, but a problem may occur when removing files, so now it is also in the range of this repository.
- Windows 7 SP1 / Windows Server 2008 R2: Require KB2533623 installed, require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/86eda38dabd1753092f64de7dcd2050d346c89b4 / [Git diff](https://github.com/XTLS/go-win7/commit/86eda38dabd1753092f64de7dcd2050d346c89b4.diff)
1. https://github.com/XTLS/go-win7/commit/c3e5e430625ba7ab5d638c753f94cc52253793f5 / [Git diff](https://github.com/XTLS/go-win7/commit/c3e5e430625ba7ab5d638c753f94cc52253793f5.diff)
1. https://github.com/XTLS/go-win7/commit/0d36c60d2c0754fde7e4e7e4773e0349527cff08 / [Git diff](https://github.com/XTLS/go-win7/commit/0d36c60d2c0754fde7e4e7e4773e0349527cff08.diff)

For Windows 7 SP1/Windows Server 2008 R2 SP1 without KB2533623 or KB3125574 installed, these patches are also needed:

1. https://github.com/XTLS/go-win7/raw/refs/heads/build/pre-KB3125574-1-26.diff

For Windows 7/Windows Server 2008 R2 without SP1 installed, these patches are also needed:

1. https://github.com/XTLS/go-win7/raw/refs/heads/build/pre-SP1-1-26.diff

## Go 1.27

- Windows 8.1 Update 3 / Windows Server 2012 SP2 / Windows Server 2012 R2 with update: Require patches in SDK, and binaries must be built with patched SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require KB2533623 and KB4474419 installed, require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows Server 2008 R2 SP1+ / Windows 7 SP1+ / Windows Server 2012 SP2 / Windows Server 2012 R2 with update / Windows 8.1 with update 3
