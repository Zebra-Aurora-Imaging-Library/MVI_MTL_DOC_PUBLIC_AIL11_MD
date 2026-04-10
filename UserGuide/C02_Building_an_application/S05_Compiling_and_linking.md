---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Compiling_and_linking
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Compiling and linking"
---

# Compiling and linking

To compile an Aurora Imaging Library application program, you must include the _AIL.h_ header file, in addition to the required standard C include files. After you have compiled your application program, you will have to link it with the appropriate libraries or import libraries for your operating system, compiler, and target board. The Aurora Imaging Library LIB files are located in the installed Library directory (for example, _C:\Program Files\Aurora Imaging Library\11\Library\LIB_).

If you are working with .NET, you must first add the Zebra Aurora Imaging Library NuGet package in Visual Studio and then include the library in your code. See [Building a .NET application using Aurora Imaging Library](../C60_Using_AIL_with_NET/S03_Building_a_NET_application_using_AIL.md) for more information.

> **Note:** Note that Aurora Imaging Library functions run at a slower speed during debug mode. Therefore, once you are done debugging your application, you should compile in release mode to achieve maximum processing speed.

The following is a list of Aurora Imaging Library and Aurora Imaging Library Lite files for the Microsoft Windows operating system, which can be included in your program. Note that you must include ail11.lib to compile your application program.

| Microsoft Windows library files | Description |
| --- | --- |
| ail11.lib | Core library. |
| ailcom11.lib | Industrial Communication module library. |
| ailfpga11.lib | FPGA module library. |
| ailim11.lib | Image Processing module library (only some functionality is supported in Aurora Imaging Library Lite). |
| ail3d11.lib | Core 3D library. |

The following is a list of Aurora Imaging Library files for the Microsoft Windows operating system, which can be included in your program. Note that you must include _ail11.lib_ to compile your application program.

| Microsoft Windows library files | Description |
| --- | --- |
| ail11.lib | Core library. |
| ailagm11.lib | Advanced Geometric Matcher module library. |
| ailbead11.lib | Bead module library. |
| ailblob11.lib | Blob Analysis module library. |
| ailcal11.lib | Camera Calibration module library. |
| ailclass11.lib | Classification module library. |
| ailcode11.lib | Code module library. |
| ailcolor11.lib | Color Analysis module library. |
| aildlocr11.lib | Deep Learning OCR module library. |
| aildmr11.lib | SureDotOCR module library. |
| ailedge11.lib | Edge Finder and Analysis module library. |
| ailim11.lib | Image Processing module library. |
| ailmeas11.lib | Measurement module library. |
| ailmetrol11.lib | Metrology module library. |
| ailmod11.lib | Geometric Model Finder module library. |
| ailocr11.lib | Character Recognition module library. |
| ailpat11.lib | NGC Pattern Matching module library. |
| ailreg11.lib | Registration module library. |
| ailstr11.lib | String Reader module library. |
| ail3dblob11.lib | 3D Blob analysis module library. |
| ail3dim11.lib | 3D image processing module library. |
| ail3dmap11.lib | 3D Reconstruction module library. |
| ail3dmeas11.lib | 3D Measurement module library. |
| ail3dmet11.lib | 3D Metrology module library. |
| ail3dmod11.lib | 3D Model Finder module library. |
| ail3dreg11.lib | 3D Registration module library. |

The following is a list of Aurora Imaging Library and Aurora Imaging Library Lite files for the Linux operating system, which can be included in your program. Note that you must include _libail11.so_ to compile your application program.

| Linux library files | Description |
| --- | --- |
| libail11.so | Core library. |
| libailcom11.so | Industrial Communication module library. |
| libailfpga11.so | FPGA module library. |
| libailim11.so | Image Processing module library (only some functionality is supported in Aurora Imaging Library Lite). |
| libail3d11.so | Core 3D library. |

The following is a list of Aurora Imaging Library files for the Linux operating system, which can be included in your program. Note that you must include _libail11.so_ to compile your application program.

| Linux library files | Description |
| --- | --- |
| libail11.so | Core library. |
| libailagm11.so | Advanced Geometric Matcher module library. |
| libailbead11.so | Bead module library. |
| libailblob11.so | Blob Analysis module library. |
| libailcal11.so | Camera Calibration module library. |
| libailclass11.so | Classification module library. |
| libailcode11.so | Code module library. |
| libailcolor11.so | Color Analysis module library. |
| libaildlocr11.so | Deep Learning OCR module library. |
| libaildmr11.so | SureDotOCR module library. |
| libailedge11.so | Edge Finder and Analysis module library. |
| libailim11.so | Image Processing module library. |
| libailmeas11.so | Measurement module library. |
| libailmetrol11.so | Metrology module library. |
| libailmod11.so | Geometric Model Finder module library. |
| libailocr11.so | Character Recognition module library. |
| libailpat11.so | NGC Pattern Matching module library. |
| libailreg11.so | Registration module library. |
| libailstr11.so | String Reader module library. |
| libail3dblob11.so | 3D Blob analysis module library. |
| libail3dim11.so | 3D Image Processing module library. |
| libail3dmap11.so | 3D Reconstruction module library. |
| libail3dmeas11.so | 3D Measurement module library. |
| libail3dmet11.so | 3D Metrology module library. |
| libail3dmod11.so | 3D Model Finder module library. |
| libail3dreg11.so | 3D Registration module library. |

After installing Aurora Imaging Library 11, you must upgrade your source code and recompile. For details, see [Converting your source code](S03_Going_from_AIL_10.7_to_AIL_11.md).

## Compiler warnings for deprecated constants and functions

If you recompile an Aurora Imaging Library application that uses deprecated constants and functions, compiler warnings will typically be generated. Deprecated constants and functions should no longer be used since they could be removed in a future Aurora Imaging Library release or update. All code that produces warnings about deprecated features should be updated to the code's current equivalent. For information on what has been deprecated, see the Aurora Imaging Library release notes, which are accessible from Aurora Imaging Control Center or online (for example, [zebra.com/ail-info](https://zebra.com/ail-info)).

> **Note:** Note that an Aurora Imaging Library 10.7 application (or prior) will not compile with Aurora Imaging Library 11 without first updating your source code. For more information, see [Converting your source code](S03_Going_from_AIL_10.7_to_AIL_11.md).

Deprecated constants typically require that you replace them with the current equivalent constant. For example, **M_SETUP** is a deprecated constant for [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) and is equivalent to the current [`M_DEFAULT`](../../Reference/app/MappAllocDefault.md) constant; so all cases of [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) with **M_SETUP** must be replaced with [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) with [`M_DEFAULT`](../../Reference/app/MappAllocDefault.md). You can establish what the equivalent for a deprecated constant is by searching for it in the _AIL.h_ file or by referring to the Aurora Imaging Library release notes.

Deprecated functions require that you replace them with either an equivalent function or a combination of an equivalent function with a specific constant. For example,**MbufControlRegion()** should be replaced with [`MbufControlArea`](../../Reference/buf/MbufControlArea.md), while **MdigLut()** should be replaced with [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LUT_ID`](../../Reference/dig/MdigControl.md). You can verify what the equivalent for a deprecated function is by referring to the Aurora Imaging Library release notes.

When building an application in C/C++, you can disable warnings about deprecated features using the **M_AIL_WARN_ON_DEPRECATED** extension. To disable warnings, include the following statement before the `#include &lt;AIL.h>` statement in your application's code:

```
#define M_AIL_WARN_ON_DEPRECATED 0 
```

Note, however, that code with deprecated features might not compile with a future release, or update to, Aurora Imaging Library if you disable warnings and do not replace the code with the current equivalent.

When building an application in the  .NET environment, the process to disable compiler warnings is different and not specific to Aurora Imaging Library. For more information, see [Compiler warnings](../C60_Using_AIL_with_NET/S03_Building_a_NET_application_using_AIL.md).
