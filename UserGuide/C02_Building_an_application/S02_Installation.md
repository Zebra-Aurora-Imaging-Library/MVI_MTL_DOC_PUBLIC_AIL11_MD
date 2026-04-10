---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Installation
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Installation"
---

# Installation and before you start

In addition to your Aurora Imaging Library installation media, you will require a hardware license-key for development of applications. The key allows you to code, debug, and run your applications. Aurora Imaging Library only supports hardware license-keys for USB ports. To redistribute your Aurora Imaging Library applications, see [Distribution of Aurora Imaging Library applications](../C69_Distribution_and_licensing/S01_Distribution_of_applications.md).

Some anti-virus or security applications might interfere with Aurora Imaging Library installation. For an uninterrupted installation, it is recommended to temporarily disable your anti-virus or security application.

## Under Microsoft Windows

To install your Aurora Imaging Library software under Microsoft Windows, attach the development hardware license-key to the USB port of your computer, place the installation media in an appropriate drive, and execute the _EXE_ program.

### Disabling speed throttling when using an Intel hybrid processor

If you use an Intel hybrid processor with Windows, you can test an application's execution speed and, if necessary, disable speed throttling. Speed or power throttling occurs when a hybrid processor prioritizes power efficiency over performance, using its more efficient cores to reduce power consumption at the cost of performance. To verify if your Intel hybrid processor is slowing execution time, run the example _DisableExecutionSpeedThrottling_, which provides a real-time display of an application's execution speed, and offers a mechanism for disabling speed throttling.

> **Note:** You must add the code that disables speed throttling to the beginning of your application (that is, before allocating an Aurora Imaging Library application context and system), so that all internal threads inherit it.

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.

### Enabling 8.3 file name creation

Under Windows, you must enable automatic 8.3 file name creation so that the Aurora Imaging Library installer can access the temp folder when the user name contains a space. Doing so allows Windows to create short aliases for files and folders with lengthy names, which ensures compatibility with programs like the Aurora Imaging Library installer that don't support spaces in file/folder names. Alternatively, you can run the Aurora Imaging Library installer from a user account that is part of the Administrators group, provided the account name does not include spaces. Note that the same applies when uninstalling Aurora Imaging Library.

## Under Linux

To install your Aurora Imaging Library software under Linux:

1. Ensure that the following development tools have been installed (additional information can be provided in release notes).
   - GCC (GNU compiler collection) and G++.
   - LSB (Linux standard base support).
   - gtk3-devel (optional, needed for the mdispgtk and mdispwindowgtk examples).
   - qt6-devel (optional, needed for the mdispqt and mdispwindowqt examples).
2. Attach the development hardware license-key to the USB port of your computer, place the installation media in an appropriate drive, and execute one of the following commands as the root user or using the sudo command.
   To install the full version of Aurora Imaging Library, execute:
   ```
   # sh ./ail-11_XXXX-installer.run

   ```
   To install Aurora Imaging Library Lite, execute:
   ```
   # sh ./ail-lite-11_XXXX-installer.run

   ```
   Replace XXXX with the Aurora Imaging Library build number.

During installation, a new group named aurora_imaging is added to the /etc/group file, and you will be prompted to add at least one valid user to this group. Any user in this group can change the Aurora Imaging Library configuration using Aurora Imaging Configurator, modify examples, recompile Aurora Imaging Library drivers, and manage the Distributed Aurora Imaging Library services. Note that you don't need to add the root user to the aurora_imaging group.

During installation, Aurora Imaging Library drivers will be compiled to match the version of the running kernel. After installation, any user in the aurora_imaging group can recompile the drivers by accessing the **Driver Status** page of Aurora Imaging Configurator, and then clicking on the **Compile Drivers** button.

To uninstall Aurora Imaging Library completely, execute the following command (as root):

```
# /opt/aurora_imaging_library/11/tools/ail-uninstall.sh 
```

## General installation notes

You can install the library on a computer with Aurora Imaging Library 10.7 (or later) already installed. See [Side-by-side installations](S03_Going_from_AIL_10.7_to_AIL_11.md) for more information. When installing the library on a computer with a version prior to Aurora Imaging Library 10.7 already installed, the setup program will not install Aurora Imaging Library. The older version must be uninstalled before the setup program can install Aurora Imaging Library.

You can install Aurora Imaging Library on a virtual machine. See [Using Aurora Imaging Library on a virtual machine](S26_Using_Aurora_Imaging_Library_on_a_virtual_machine.md) for more information.

Note that the installation program also installs Aurora Imaging Intellicam (your frame grabber configuration program, not supported under Linux), Aurora Imaging Control Center, Example Launcher, Aurora Imaging Library interactive utilities (not supported under Linux), Aurora Imaging Configurator, and Aurora Imaging CoPilot (not supported under Linux).

Aurora Imaging Library can be run without the hardware license-key if you activate an Aurora Imaging Library provisional license. Once activated, the provisional license allows use of Aurora Imaging Library on your computer for 30 days. Each time you run Aurora Imaging Library, a dialog box appears indicating the number of days until the evaluation license expires. Once this time period has elapsed, Aurora Imaging Library will not run unless you purchase a permanent license. For more information on activating an Aurora Imaging Library license, see [Aurora Imaging Library and Aurora Imaging Library Lite licenses](../C69_Distribution_and_licensing/S06_Licenses.md).

An Aurora Imaging Library provisional license can only be installed once. Any attempt to tamper with your computer's calendar, before the date of expiry, will disable Aurora Imaging Library. In that event, Aurora Imaging Library can only be re-used once a permanent license is obtained.

### Testing the installation

You should test the installation process by running an example, such as _MappStart_, from Aurora Imaging Example Launcher. You can verify and edit the code that is executed by clicking on the **Edit** button in Example Launcher.

## Communicating properly and completing initial steps

During application development, you can use _MappStart_ to ensure that the software is communicating properly with the target system. To make sure your frame grabber is working properly with your camera, use Aurora Imaging Intellicam or run a grab example.

Before you start using Aurora Imaging Library, remember to:

1. Register Aurora Imaging Library on the support page of the Zebra website. This ensures that you are on our mailing list and will receive any information on product updates and promotions.
2. See the Aurora Imaging Library release notes, accessible from Aurora Imaging Control Center and/or the Zebra Aurora Imaging Library product support webpages (for example, [zebra.com/ail-info](https://zebra.com/ail-info)), which can provide documentation for updates.
3. Review the [Using the defaults](S13_Using_the_defaults.md) item in the Aurora Imaging Configurator to make sure that the default setup configuration matches your system configuration.
