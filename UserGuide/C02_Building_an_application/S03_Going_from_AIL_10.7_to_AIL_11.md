---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Going_from_AIL_10.7_to_AIL_11
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Going from AIL 10.7 to AIL 11"
---

# Going from Aurora Imaging Library 10.7 to Aurora Imaging Library 11

If you are going from Aurora Imaging Library 10.7 to 11, you should be aware of the following:

- You can install Aurora Imaging Library 10.7 and 11 on your computer side by side.
- You must use Aurora Imaging Control Center to select which version to use.
- Most tools have been renamed in Aurora Imaging Library 11. For example, Aurora Imaging Control Center, Aurora Imaging Capture Works, Aurora Imaging Configurator, Aurora Imaging CoPilot, Aurora Imaging Example Launcher, Aurora Imaging Gecho Viewer, Aurora Imaging Intellicam, and Aurora Imaging Profiler.
- You must upgrade your source code to use Aurora Imaging Library 11.
- A code conversion utility, sourceconverter, is provided to help transition source code from 10.7 to 11.

This section also contains licensing information, which is relevant whether you install both versions at the same time, or you are moving from one version to the other.

> **Note:** Note, Aurora Imaging Library releases might be referred to using either decimals or roman numerals throughout the Help. For example, the names Aurora Imaging Library X and Aurora Imaging Library 10 are used interchangeably.

## Side-by-side installations

It is now possible to have several versions of Aurora Imaging Library installed on your computer side by side. Note that currently, only side-by-side installations of version 10.7 and version 11 are supported. You cannot install the library on a computer with a version older than 10.7.

The complete set of features are only available for one version at a time; all other versions will work on the Host system only.

### Aurora Imaging Control Center

All versions of Aurora Imaging Library that are installed on your computer will appear in Aurora Imaging Control Center, which you must use to select the version of the library that you want to use. Note that only one version can be selected at a given time. You can inquire the selected version using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_SELECTED_VERSION`](../../Reference/app/MappInquire.md). When selected, all services and drivers for that version are enabled. For example, you are able to use the selected version's installed frame grabbers and Distributed Aurora Imaging Library. The services and drivers for the unselected version will be uninstalled, and it will work as if you had only installed the Host system. Note that pages in Aurora Imaging Configurator that are only relevant to a selected version will not be accessible for unselected versions. For example, the pages related to boards will be grayed out, unless you select that version in the Control Center.

You can use Aurora Imaging Control Center to access the files and applications installed along with each version. For example, you can access the Help for the required version by toggling between versions in the Control Center.

### Aurora Imaging Library add-on to Visual Studio

When you open Microsoft Visual Studio, the Aurora Imaging Library add-on to Visual Studio looks for which Aurora Imaging Library version is currently selected and enables the tools for that version. For example, if Aurora Imaging Library 11 is selected, the toolbar contains links to Aurora Imaging Library 11 tools, F1 contextual help points to the Aurora Imaging Library 11 Help, and IntelliSense suggests Aurora Imaging Library 11 values. Note that if you change the selected version while Microsoft Visual Studio is open, the add-on will not be updated and it will continue using the version that was selected upon opening. You must close and reopen Microsoft Visual Studio if you want to use the newly selected version.

## Converting your source code

Source code written for Aurora Imaging Library 10.7 will not compile with Aurora Imaging Library 11 as-is. This is due to updates, such as the replacement of certain patterns in constants, defines, header files, the assembly name in C#, the import statement in Python, and the command used to activate Python. If you have an Aurora Imaging Library 10.7 application and you want to use Aurora Imaging Library 11, you must transition your code.

Manually upgrading your Aurora Imaging Library 10.7 code to 11 can be time-consuming and prone to errors. We have provided a code conversion utility, sourceconverter, with Aurora Imaging Library 11 to automate this process. To use it, open a command prompt and enter the following:

```
sourceconverter <path>
```

Replace `&lt;path>` with the path to the folder containing your source code. Note, you can alternatively provide a file path to update a specific file.

The utility will search for and replace all patterns in the provided files that have been updated in Aurora Imaging Library 11. This includes renaming constants, header files, and updating all other affected parts of your code. Note that the sourceconverter utility only works for files in UTF-8 or ASCII format. If your files are in another format, you will need to convert them before using the utility. It is recommended to create a backup of your files before running the utility.

## Licensing

If you purchased a software license-key for Aurora Imaging Library X, you can use the same software license-key with Aurora Imaging Library 11. The associated hardware-component must be supported by the Aurora Imaging Library version and installed during setup for the license to work. Note that software license-keys purchased for Aurora Imaging Library 11 are not backwards-compatible with Aurora Imaging Library X.

If you are using a dongle to activate your license, the same dongle can be used with Aurora Imaging Library X and 11.

For network licensing, once you configure a client to connect to a license server for one version of the library, you don't need to repeat the configuration process for each version. The same server can handle requests for all versions.

For more information on licensing, see [Aurora Imaging Library and Aurora Imaging Library Lite licenses](../C69_Distribution_and_licensing/S06_Licenses.md).
