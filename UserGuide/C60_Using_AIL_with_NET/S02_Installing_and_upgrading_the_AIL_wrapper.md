---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Installing_and_upgrading_the_AIL_wrapper
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Installing and upgrading the AIL wrapper"
---

# Installing and upgrading the AIL.NET wrapper

When Aurora Imaging Library is either initially installed or upgraded, the AIL.NET wrapper is automatically installed or upgraded as well.

## Requirements and installation notes

The specific hardware and software requirements for using Aurora Imaging Library are detailed in [Requirements](../C02_Building_an_application/S01_Requirements.md).

If Aurora Imaging Library is installed after Visual Studio, the required Zebra Aurora Imaging Library NuGet package will be automatically added. If not, you can manually add the AIL.NET wrapper using either the NuGet Package Manager or the `add source` command.

```
For example: dotnet nuget add source C:\Program Files\Aurora Imaging Library\11\Library\AIL.NET\NuGet --name ail.net
```

To check that the AIL.NET wrapper was successfully installed, enter the following command:

```
dotnet nuget list source
```

AIL.NET should be present in the returned list.

Examples for C# can be found in the installed Examples directory (for example, _%AIL_USR_PATH11%\Examples\_).

## Installing AIL.NET and compiling examples for Linux

Aurora Imaging Library for Linux provides the AIL.NET wrapper and C# examples for Microsoft .NET 8.0. There are minor differences to the installation steps depending on whether you are installing Aurora Imaging Library after .NET or vice versa.

### Installing Aurora Imaging Library after .NET

To install Aurora Imaging Library after installing .NET, make sure the .NET executable is located in the _/usr/local/bin_ or _/usr/bin_ path; this is because sudo restricts the PATH environment variable. You can also create a link to the path using the following sudo command:

```
sudo ln -s /usr/local/bin/dotnet /PathToDotNetBin
```

Launch the Aurora Imaging Library setup and select the `.Net and Scripting` component. The setup will then install the AIL.NET wrapper.

### Installing .NET after Aurora Imaging Library

To install .NET after installing Aurora Imaging Library, make sure the `.Net and Scripting` component was selected when Aurora Imaging Library was installed. After installing .NET, add the AIL.NET wrapper using the `add source` command.

```
For example: dotnet nuget add source /opt/aurora_imaging_library/11/library/ail.net/NuGet --name ail.net
```

To check that the AIL.NET wrapper was successfully installed, enter the following command:

```
dotnet nuget list source
```

AIL.NET should be present in the returned list.

### Compiling C# examples

Compiling Aurora Imaging Library C# examples requires that dotnet be installed in _/usr/share/dotnet_ directory or that the DOTNET_ROOT environment variable be exported. To compile all C# examples, use the following command:

```
cd $AILDIR/examples make all_dotnet
```

## Upgrading the Zebra Aurora Imaging Library NuGet package

You can upgrade an existing project with a new version of Aurora Imaging Library using the NuGet Package Manager. Visual Studio will automatically manage package dependencies in your project.
