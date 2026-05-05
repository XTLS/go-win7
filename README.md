# Patched Go SDK for Windows Server 2008 R2, Windows 7, Windows Server 2012, Windows Server 2012 R2 and Windows 8.1

The Go SDK with patches that can run on **Windows Server 2008 R2 SP1 + Convenience Rollup**, **Windows 7 SP1 + Convenience Rollup**, **Windows Server 2012 SP2**, **Windows Server 2012 R2 with update**, and **Windows 8.1 with Update 3**. Only reverted some incompatible changes from [Go](https://github.com/golang/go).

If you need other pre-built binaries that does not found in Release, you can fork and build it freely.

## Compatibility

For Go 1.22 - 1.26, the baseline of the compatibility is:

- Windows 7
- Windows Server 2008 R2

Since Go 1.27, the baselines of the compatibility are:

- Windows 7 SP1
  - All updates from Windows Update released before April 2016 and KB4474419 + KB4490628;
  - or install KB3020369 (minimum April 2015) + KB3125574 and KB4474419 + KB4490628
- Windows Server 2008 R2
  - All updates from Windows Update released before April 2016 and KB4474419 + KB4490628;
  - or install KB3020369 (minimum April 2015) + KB3125574 and KB4474419 + KB4490628
- Windows Server 2012 SP2
- Windows 8.1 with Update 3
- Windows Server 2012 R2 with update

The decision is based on both security concern and API mordernization. Even though the the updates metioned are the baselines, you still need to install additional updates to keep better functionality and security for the OS, especially preventing EnternalBlue in wild for all OSes. Installing several key updates may also work, but compatibility may drift when upstream making changes.

For detailed compatibility notes, you can find in the detailed ReadMe.

Detailed ReadMe / 详细说明文件:

- [English](./README-eng.md)
- [中文（简体）](./README-zho-hans.md)

## Supported version

Current support status: 1.26.x (main), 1.25.x (check)

Possible final target: 1.30.x

For more details, read #16 .
