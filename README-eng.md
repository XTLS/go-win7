# Patched Go toolchain for Windows 7

The Go toolchain with patches that can run on Windows 7. Only reverted some commits that will break running on Windows 7 from [Go](https://github.com/golang/go).

This toolchain is used for building binaries that can run on Windows 7 that does not supported officially by Go now. You can use it freely to build binaries targeting Windows 7 from Go.

If you need other pre-built SDK binaries that does not found in Release, you may fork and build it.

## Status

Currently found these changes in Go SDK will stop running Windows 7:

- a17d959debdb04cd550016a3501dd09d50cd62e7 (`runtime: always use LoadLibraryEx to load system libraries`) (affecting older builds)
- 7c1157f9544922e96945196b47b95664b1e39108 (`net: remove sysSocket fallback for Windows 7`)
- 48042aa09c2f878c4faa576948b07fe625c4707a (`syscall: remove Windows 7 console handle workaround`)
- 693def151adff1af707d82d28f55dba81ceb08e1 (`crypto/rand,runtime: switch RtlGenRandom for ProcessPrng`)
- 534d6a1a9c81b25bdad1052e736b2f072caa3903 (`crypto/rand: prevent Read argument from escaping to heap`) (breaking previous patch on `crypto/rand,runtime: switch RtlGenRandom for ProcessPrng`)

### Go 1.21

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2 SP1:
  - Go 1.21rc1 ~ Go 1.21.4: Require update KB4474419 (SHA-2 code signing support update) installed. For computers that have Internet access, it is recommended to install KB4490628 (Servicing stack update) to install subsequent security update.
  - Go 1.21.5 and onward: Cannot run official distributed SDK or binaries built from official SDK, because of a change of crypto API in toolchain.

### Go 1.22

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988 / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1 / [Git diff](https://github.com/XTLS/go-win7/commit/41373f90356fd86e9cbe78c7a71c76066a6730c1.diff)
1. https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6 / [Git diff](https://github.com/XTLS/go-win7/commit/481cebf65c4052379cf3cda5db5588c48f2446f6.diff)
1. https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0 / [Git diff](https://github.com/XTLS/go-win7/commit/21d5caecf644a12d938c45f18e2b55f04b47f0b0.diff)

### Go 1.23

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/429f9a72007759a757c8e96a2763306c076dbb8f / [Git diff](https://github.com/XTLS/go-win7/commit/e4701f06a6358bda901e72cbff44f414d902e988.diff)
1. https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8 / [Git diff](https://github.com/XTLS/go-win7/commit/ca03d8ed77dab8b91b69a9d44e6e56844fbcd5d8.diff)
1. https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1 / [Git diff](https://github.com/XTLS/go-win7/commit/719ab22f14443a88dd274f7a41afb86dd41628b1.diff)
1. https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5 / [Git diff](https://github.com/XTLS/go-win7/commit/ac17c301268dd7835236e61c9bfefdf2b8e633d5.diff)

### Go 1.24 (Still testing, will change in the future)

- Windows 8.1 Update 3 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 SP1 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches for Windows 7 / Windows Server 2008 R2

These patches must be applied from up to down:

1. https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0 / [Git diff](https://github.com/XTLS/go-win7/commit/f429f15f6305e4432afd7309b317e903bd76a5c0.diff)
1. https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1 / [Git diff](https://github.com/XTLS/go-win7/commit/41f545de980e9285b68ece40d4b4e63feef9c5a1.diff)
1. https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c / [Git diff](https://github.com/XTLS/go-win7/commit/b6c99a977f732ee5553ddc75ae0fe3b47927fc1c.diff)
1. https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c / [Git diff](https://github.com/XTLS/go-win7/commit/36d7775e030192d3bf2dc111d1f6cfa89eae5f0c.diff)
1. https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d / [Git diff](https://github.com/XTLS/go-win7/commit/a3e4d4735a5d89f60b907308b556c5a53614914d.diff)
