---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: How_to_create_a_portable_application
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / How to create a portable application"
---

# How to create a portable application

You can create a more portable, device-independent, and operating system-independent application if you use the following coding techniques in your application:

- Limit the amount of Aurora Imaging Library system-specific code (for example, Aurora Imaging Library system-specific control types and inquire types) in your application.
- Use the defaults when allocating your system and other Aurora Imaging Library objects (for example, your digitizer, buffer, and display). By doing this, you create a more portable application, since these settings are not hard-coded in your application, and are determined when the user installs the Aurora Imaging Library software. For more information on using the defaults, see [Using the defaults](S13_Using_the_defaults.md).
- Use custom data types and portability functions, as outlined in [Aurora Imaging Library custom data types, extensions, and portability functions](S16_Custom_data_types_extensions_and_portability_functions.md), instead of their operating system-specific equivalents.
- Avoid operating system-specific features (for example, using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_DIB`](../../Reference/buf/MbufAlloc2d.md), which is specific to a Windows-based operating system).
