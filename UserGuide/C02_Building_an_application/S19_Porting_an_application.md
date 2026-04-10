---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Porting_an_application
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Porting an application"
---

# Porting a 32-bit Aurora Imaging Library application to 64-bit

As of Aurora Imaging Library X 2102, Aurora Imaging Library only supports 64-bit operating systems. For more details about supported operating systems, refer to [Requirements](S01_Requirements.md).

When porting an application to Aurora Imaging Library 64-bit, there are certain modifications that must be made to your application's code. This section explains these changes and the circumstances under which they are required.

## Modified functions

In the 64-bit version of Aurora Imaging Library, there are actually two versions of the following functions:

- [`M...Control`](../../Reference/3dmap/M3dmapControl.md).
- [`MgraArc`](../../Reference/gra/MgraArc.md).
- [`MgraArcFill`](../../Reference/gra/MgraArcFill.md).
- [`MgraDot`](../../Reference/gra/MgraDot.md).
- [`MgraLine`](../../Reference/gra/MgraLine.md).
- [`MgraRect`](../../Reference/gra/MgraRect.md).
- [`MgraRectFill`](../../Reference/gra/MgraRectFill.md).
- [`MgraText`](../../Reference/gra/MgraText.md).
- [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md).
- [`MmodDefine`](../../Reference/mod/MmodDefine.md).
- [`MregSetLocation`](../../Reference/reg/MregSetLocation.md).

The two versions of these functions differ in the expected data types of their parameters: one version has a _Double_ suffix, and one version has an _Int64_ suffix (for example, [`MblobControlDouble`](../../Reference/MblobControlDouble.md) and [`MblobControlInt64`](../../Reference/MblobControlInt64.md)). The _Double_ version of these functions should be used when the relevant parameter (typically the [`ControlValue`](../../Reference/blob/MblobControl.md) parameter) requires an _AIL_DOUBLE_ value and the _Int64_ version should be used when an _AIL_INT_ value is required.

In C++ (*.cpp), the original functions are available as inline functions which will automatically call the appropriate version of the function depending on the type of parameter received. No change to your source code should be required if you are compiling your 64-bit Aurora Imaging Library application in C++.

In C (*.c), the original functions are actually macros that call the most typically required version of the function. This means that if you are compiling your Aurora Imaging Library application in C, only minor modifications to your code will be necessary. Compiler warnings will be produced if you pass a function a double value where an integer is expected; in this case, you will need to explicitly change this function call to the proper version of the function.

### Retrieving a pointer to a modified function

Advanced users might want to retrieve a pointer to one of the modified functions. In this case, only the _Double_ or _Int64_ version should be used, since the original function is actually a macro or an overloaded function. For example, you must retrieve a pointer to [`MmetSetPositionDouble`](../../Reference/MmetSetPositionDouble.md) or [`MmetSetPositionInt64`](../../Reference/MmetSetPositionInt64.md), and not [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md).

Note that when retrieving pointers to functions, the following are also affected: [`MimArith`](../../Reference/im/MimArith.md) and [`MimArithMultiple`](../../Reference/im/MimArithMultiple.md). For these functions, a _Double_ version also exists; you must use it instead of the original.

> **Note:** Note that the actual name of the exported Aurora Imaging Library function might be slightly different than what's documented.

## Void pointers

Most Aurora Imaging Library functions have parameters of a fixed data type. However, there are some functions, predominantly [`M...Inquire`](../../Reference/3dmap/M3dmapInquire.md) and [`M...GetResult`](../../Reference/3dmap/M3dmapGetResult.md) functions, where one (or more) of their parameters is a void pointer. The expected data type for these void pointer parameters is determined in one of the two ways as described in [](S16_Custom_data_types_extensions_and_portability_functions.md).

For the 64-bit version of Aurora Imaging Library, you will have to use the required custom data types documented as of Aurora Imaging Library 9.0. For example, the following code for the 32-bit version of Aurora Imaging Library:

> **Code example:** [userguide.building_an_application.newestport](userguide.building_an_application.newestport)

Should be changed for the 64-bit version of Aurora Imaging Library as follows:

> **Code example:** [userguide.building_an_application.porting_64](userguide.building_an_application.porting_64)

### M_AIL_USE_SAFE_TYPE extension

When using void pointers, it is possible to make a mistake and pass a value with the wrong data type. These errors can cause unexpected behavior such as: stack corruption, array overflow, uninitialized returned data, and segmentation faults. To ease the transition from 32-bit to 64-bit, use the Aurora Imaging Library **M_AIL_USE_SAFE_TYPE** extension. It will help you find calls to functions whose prototypes have been modified. By default, the **M_AIL_USE_SAFE_TYPE** extension is enabled when compiling in debug mode.

For more information about the **M_AIL_USE_SAFE_TYPE** extension, see [Aurora Imaging Library custom data types, void pointers, extensions, and portability functions](S16_Custom_data_types_extensions_and_portability_functions.md).

> **Note:** Note that this extension is not supported under Linux.

## Project processor definitions

When porting an application to Aurora Imaging Library 64-bit, you must verify that your project processor definitions (PPDs) include WIN64 and _AMD64 (these PPDs appear in the Properties page of your application). Your application will not compile if it is missing WIN64 and _AMD64.

> **Note:** Not supported under Linux.
