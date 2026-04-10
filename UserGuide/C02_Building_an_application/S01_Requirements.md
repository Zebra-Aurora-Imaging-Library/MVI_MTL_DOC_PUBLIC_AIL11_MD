---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Requirements
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Requirements"
---

# Requirements to run Aurora Imaging Library

Aurora Imaging Library is available as a set of DLLs under the Windows and Linux operating systems. The following computer requirements should be respected to ensure that Aurora Imaging Library operates properly.

> **Note:** The Aurora Imaging Library release notes, accessible from Aurora Imaging Control Center and/or the Zebra Aurora Imaging Library product support webpages (for example, [zebra.com/ail-info](https://zebra.com/ail-info)), can provide additional documentation, such as new or modified features, systems, and products.

## Under Windows

Aurora Imaging Library 11 supports Windows 10 and 11. See the release notes for the exact supported versions.

### Hardware requirements

The minimum required hard disk space to install the Aurora Imaging Library development environment is 10 GB. The minimum required hard disk space to install the Aurora Imaging Library Lite development environment is 2 GB. A minimum of 3 GB of free hard disk space is required to install the Aurora Imaging Library runtime environment without any boards selected. In addition, a computer with a CPU that supports the SSE4.2 instruction set is required.

### Supported software

The Aurora Imaging Library installation media includes files that support the following development environments:

- Microsoft Visual Studio, including:
  - Microsoft Visual C++ compiler (unmanaged).
  - Microsoft Visual C# compiler.
- Microsoft .NET.
- Microsoft .NET Framework.
- Python development environments.
  - If you install Python after installing Aurora Imaging Library 11, you will need to run a specific command. See [Importing required libraries](../C59_Using_AIL_with_Python/S02_Installation_and_distribution_in_Python.md) for more information.

See the release notes for the supported versions of Microsoft Visual Studio, .NET, .NET Framework, and Python.

### Standby/sleep mode

Aurora Imaging Library does not support the sleep mode under Microsoft Windows. In Windows 10 and 11, an Aurora Imaging Library application detects when the operating system enters the sleep mode. Once the operating system comes out of this mode, you will be prompted to restart the computer for proper functionality of the application.

> **Note:** Note that for Aurora Imaging Library to operate properly, the Windows** fast startup** feature must be disabled. This is handled automatically during the Aurora Imaging Library installation and can be found in the Control Panel under _Control Panel\All Control Panel Items\Power Options\System Settings_.

## Under Linux

To run Aurora Imaging Library under Linux, there are minimum and recommended operating system and hardware requirements. For a list of the differences of using Aurora Imaging Library under Linux, see [Working with Linux](../C61_Using_AIL_under_Linux/S01_Working_with_Linux.md).

### Supported Linux distributions

Aurora Imaging Library 11 supports the following:

For Linux/x86-64:

- Ubuntu 24.04 LTS.

For Linux/Armv8:

- Ubuntu 24.04 LTS.
- NVIDIA JetPack 5.1.

See the release notes for the exact supported versions.

### Hardware requirements

The minimum required hard disk space to install the Aurora Imaging Library development environment is 10 GB. The minimum required hard disk space to install the Aurora Imaging Library Lite development environment is 2 GB. A minimum of 3 GB of free hard disk space is required to install the Aurora Imaging Library runtime environment without any boards selected. In addition, a computer with a CPU that supports the SSE4.2 instruction set or an embedded computer with an Arm Cortex-A family processor employing the Armv8-A 64-bit architecture is required.

### Supported software

The Aurora Imaging Library installation media includes Aurora Imaging Library files that support the following development environments under Linux:

- Microsoft .NET.
- Python development environments.
  - If you install Python after installing Aurora Imaging Library 11, you will need to run a specific command. See [Importing required libraries](../C59_Using_AIL_with_Python/S02_Installation_and_distribution_in_Python.md) for more information.
- GCC (GNU Compiler Collection).
- Intel C++ Compiler.

See the release notes for the supported versions of .NET and Python.
