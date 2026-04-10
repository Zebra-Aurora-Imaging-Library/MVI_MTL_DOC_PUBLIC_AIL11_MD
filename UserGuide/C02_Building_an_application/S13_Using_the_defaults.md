---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Using_the_defaults
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Using the defaults"
---

# Using the defaults

During the installation of your Aurora Imaging Library software, you are asked a number of questions (such as, the type of Zebra hardware installed in your computer), so that the installer knows what to install. This information is also used to define the default settings that are configurable. You can change these default settings later, using the Aurora Imaging Configurator utility.

## Using the Aurora Imaging Configurator utility to change your default settings

Review the **Default Values** tab in the Aurora Imaging Configurator utility to make sure that your computer's default settings match your application's required default settings. If they don't match, you can use the Aurora Imaging Configurator utility to change them. For example, you can use the Aurora Imaging Configurator utility to change the default system, the default image buffer size and attributes, the default display settings, and the default digitizer's DCF.

## Using your defaults

You can use the [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) macro to allocate an Aurora Imaging Library application context and your default system. On this system, [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) also allows you to allocate your default image buffer, default display, and default digitizer. Alternatively, you can use [`MappAlloc`](../../Reference/app/MappAlloc.md), [`MsysAlloc`](../../Reference/sys/MsysAlloc.md), [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md), [`MdispAlloc`](../../Reference/disp/MdispAlloc.md), and [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEFAULT`](../../Reference/app/MappAlloc.md) to allocate these defaults. By allocating these using their default settings, you create a more portable, device-independent application, since these settings are not hard-coded in your application, and are determined when your client installs the Aurora Imaging Library software.

If the Aurora Imaging Library driver for a Zebra frame grabber has been installed, the size, number of bands, and attributes for the default image buffer will be established based on the specified DCF for the default digitizer. If a monochrome DCF is specified as the default, a single-band monochrome buffer is allocated as the default image buffer. If a color DCF is specified, a multi-band color buffer is allocated as the default image buffer. The default buffer size is the same as that of the image capture-size specified in the DCF.

When allocating both the default image buffer and the default display using [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md), the image buffer is given a displayable attribute, cleared, and selected to the display.

> **Note:** Note that although there are advantages to using the default settings, it can make debugging more difficult since the settings for the defaults are not determined within your application.
