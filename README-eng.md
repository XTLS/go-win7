# Patched Go SDK for Windows Server 2008 R2, Windows 7, Windows Server 2012, Windows Server 2012 R2 and Windows 8.1

The Go SDK with patches that can run on **Windows Server 2008 R2 SP1 + Convenience Rollup**, **Windows 7 SP1 + Convenience Rollup**, **Windows Server 2012 SP2**, **Windows Server 2012 R2 with update**, and **Windows 8.1 with update 3**. Only fixed some incompatible code that will break running on these operating systems from [Go](https://github.com/golang/go).

This SDK is used for building binaries that can run on listed OSes that does not supported officially by Go now. You can use it freely to build binaries targeting these listed OSes from Go.

If you need other pre-built SDK binaries that does not found in Release, you may fork and build it.

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

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Remove PEB hack for long path support (From Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`

#### Patches included in legacy patch as add-on:

- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.22

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Remove PEB hack for long path support (From Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.23

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Remove PEB hack for long path support (From Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.24

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Can run official distributed Go SDK and binaries built from official SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Remove PEB hack for long path support (From Microsoft Go)
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.25

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Require patches in SDK, and binaries must be built with patched SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Remove PEB hack for long path support (From Microsoft Go)
- Fix dysfunctional of `os.RemoveAll` failing on legacy Windows
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.26

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Require patches in SDK, and binaries must be built with patched SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Remove PEB hack for long path support (From Microsoft Go)
- Fix dysfunctional of `os.RemoveAll` failing on legacy Windows
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows

#### Patches included in legacy patch as add-on:

- Fix removed `sysSocket` fallback for legacy Windows
- Fix `LoadLibraryEx` flag not supported on unpatched legacy systems

## Go 1.27

- Windows 8.1 / Windows Server 2012 / Windows Server 2012 R2: Require patches in SDK, and binaries must be built with patched SDK.
- Windows 7 / Windows Server 2008 R2: Require patches in SDK, and binaries must be built with patched SDK.

#### Patches included in standard patch:

- Remove PEB hack for long path support (From Microsoft Go)
- Fix PE header locking on minimum Windows NT 10.0
- Fix dysfunctional of `os.RemoveAll` failing on legacy Windows
- Use `BCryptGenRandom` as a compatible substitution of `ProcessPrng`
- Fix removed console handle workaround for legacy Windows
