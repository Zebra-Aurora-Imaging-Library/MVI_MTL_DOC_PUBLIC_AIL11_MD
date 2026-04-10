---
doctype: UserGuide
part: "Getting started"
chapter: AIL_Introduction
section: Conventions_of_functions_and_modules
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / AIL_Introduction / Conventions of functions and modules"
---

# Function and module conventions

Functions in Aurora Imaging Library are grouped into modules and named based on their module and their purpose. Each function name is prefixed with M, followed by letters representing the function's module name, and ends with a word that describes what the function does. For example, [`MbufSave`](../../Reference/buf/MbufSave.md) saves a data buffer in a file.

## Similar functions in different modules

The following specifies functions that are available in most library modules.

- [`M...Alloc`](../../Reference/3dmap/M3dmapAlloc.md). Allocates the specified Aurora Imaging Library object (a data structure with an AIL ID) and its required resources. Examples of library objects include buffers, displays, and contexts. You must allocate these before you use them. Once you are finished using them, you should release them using [`M...Free`](../../Reference/3dmap/M3dmapFree.md).
- [`M...Free`](../../Reference/3dmap/M3dmapFree.md). Frees an allocated library object and its resources.
- [`M...Control`](../../Reference/3dmap/M3dmapControl.md). Sets the controls for the library object (for example, buffer, context, or element(s) that exist in the context). An [`M...Control`](../../Reference/3dmap/M3dmapControl.md) function lets you customize how to perform the operations of that module. Most control types have a default setting.
- [`M...Inquire`](../../Reference/3dmap/M3dmapInquire.md). Inquires about a setting. Most settings that can be set using [`M...Control`](../../Reference/3dmap/M3dmapControl.md) can be inquired using the corresponding [`M...Inquire`](../../Reference/3dmap/M3dmapInquire.md) function.
- [`M...Save`](../../Reference/3dmap/M3dmapSave.md). Saves a library object (for example, the specified context or buffer) to a file.
- [`M...Restore`](../../Reference/3dmap/M3dmapRestore.md)/ [`M...Load`](../../Reference/buf/MbufLoad.md). Restores a library object (for example, a context or buffer) from a file.
- [`M...HookFunction`](../../Reference/app/MappHookFunction.md). Lets you attach/detach a user-defined function to an event. Once an event-handler function is defined and hooked to an event, it is automatically called when the event occurs. The event-handler function is also referred to as a hook-handler function.
- [`M...GetHookInfo`](../../Reference/app/MappGetHookInfo.md). Retrieves information about the event that caused the hook-handler function to be called. This function should only be called within the scope of a hook-handler function call.

### Processing and analysis modules

The following specifies library functions common in most processing and analysis modules.

- [`M...AllocResult`](../../Reference/3dmap/M3dmapAllocResult.md). Allocates a result buffer. Result buffers store results obtained from the module's [`M...Calculate`](../../Reference/blob/MblobCalculate.md), [`M...Read`](../../Reference/code/McodeRead.md), or [`M...Find`](../../Reference/mod/MmodFind.md) operations. When the result buffer is no longer required, release its memory, using [`M...Free`](../../Reference/3dmap/M3dmapFree.md).
- [`M...Calculate`](../../Reference/blob/MblobCalculate.md)/[`M...Read`](../../Reference/code/McodeRead.md)/[`M...Find`](../../Reference/mod/MmodFind.md). Perform the operation(s) specific to the module, respecting the controls specified using [`M...Control`](../../Reference/3dmap/M3dmapControl.md). The results of the operations are stored in the specified result buffer.
- [`M...GetResult`](../../Reference/3dmap/M3dmapGetResult.md). Retrieves the results of the specified type from the specified result buffer.
- [`M...Copy/CopyResult`](../../Reference/3dmap/M3dmapCopy.md). Copies settings or results from a library object (for example, context) to another library object. Some functions also support moving results.
- [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md). Draws source information or results destructively into a destination image buffer, or stores them as vector graphics in a specialized library object called a 2D graphics list. The latter can be used for several purposes, including 2D display.
- [`M...Draw3d`](../../Reference/3dmap/M3dmapDraw3d.md). Stores source information or results as 3D vector graphics in a specialized library object called a 3D graphics list. The latter can be used for 3D display.
- [`M...Stream`](../../Reference/3dmap/M3dmapStream.md). Loads, restores, or saves a library object (for example, context) from/to a file or memory stream.

Similar functions exist for 3D modules. These functions are prefixed with M3D.

## Interactive functionality

Under Microsoft Windows, some library functions can launch dialog boxes that let you specify information at runtime. The most common functions that support dialog boxes are listed below:

- [`M...Save`](../../Reference/3dmap/M3dmapSave.md)with [`M_INTERACTIVE`](../../Reference/3dmap/M3dmapSave.md). Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file.
- [`M...Restore`](../../Reference/3dmap/M3dmapRestore.md)with [`M_INTERACTIVE`](../../Reference/3dmap/M3dmapRestore.md). Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file.
- [`M...Stream`](../../Reference/3dmap/M3dmapStream.md) with [`M_INTERACTIVE`](../../Reference/3dmap/M3dmapStream.md). Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the StreamType parameter is set to [`M_FILE`](../../Reference/3dmap/M3dmapStream.md).

For a full list of all the functions that support dialog boxes, type INTERACTIVE into the edit field of the **Search** tab, in the left pane of this Help file, and enable the "Match similar words" option.

You can also interactively add, move, resize, and rotate graphics in a 2D graphics list, if the 2D graphics list is associated to the 2D display and interactivity is enabled on the 2D display ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md)). For more information, see [Creating and modifying graphics interactively](../C26_Generating_graphics/S07_Creating_and_modifying_graphics_interactively.md).

> **Note:** Linux only supports interactive functionality with [`MbufRestore`](../../Reference/buf/MbufRestore.md), [`MbufSave`](../../Reference/buf/MbufSave.md), and the library's 2D Graphics module.
