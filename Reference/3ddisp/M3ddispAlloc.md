---
doctype: Reference
module: 3ddisp
function: M3ddispAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispAlloc"
---

# M3ddispAlloc

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Allocate a 3D display or picking context.

## Syntax

```c
AIL_ID M3ddispAlloc(
    AIL_ID             SysId,       //in
    AIL_INT64          DispNum,     //in
    AIL_CONST_TEXT_PTR DispFormat,  //in
    AIL_INT64          InitFlag,    //in
    AIL_ID *           Disp3dIdPtr  //out
)
```

## Description

This function allocates a 3D display or picking context on the specified system so that it can be used by subsequent Aurora Imaging Library3D display functions.

A 3D display allows you to display a point cloud, depth map, and/or 3D graphics list. To do so, use [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md). You can manipulate the view of a 3D display, either interactively using the keyboard and mouse or within your application using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) and [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md). You can change the keyboard control bindings, or disable the ability to manipulate the view interactively, using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_ACTION_KEY_...`](../../Reference/3ddisp/M3ddispControl.md) or [`M_KEYBOARD_USE`](../../Reference/3ddisp/M3ddispControl.md) / [`M_MOUSE_USE`](../../Reference/3ddisp/M3ddispControl.md), respectively. You can also control other aspects of the 3D display using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md).

If you are developing an Aurora Imaging Library application that also acts as an Aurora Imaging Web Library server, you can allocate a display that can be presented in one or more instances of an Aurora Imaging Web Library client application (for example, running in a compatible web browser). To do so, use [`M_WEB`](../../Reference/3ddisp/M3ddispAlloc.md) when allocating the display. This type of display is referred to as an Aurora Imaging Web Library display. See [Fundamentals for creating your Aurora Imaging Web Library client application](../../UserGuide/C58_Using_AIWL_to_monitor_your_application/S03_Using_AIWL_to_monitor_your_application.md) for more information.

After allocating the 3D display or picking context, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the 3D display or picking context identifier returned is not [`M_NULL`](../../Reference/3ddisp/M3ddispAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3ddisp/M3ddispAlloc.md) was specified).

When the 3D display or picking context is no longer required, release it using[`M3ddispFree`](../../Reference/3ddisp/M3ddispFree.md)unless [`M_UNIQUE_ID`](../../Reference/3ddisp/M3ddispAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3ddisp/M3ddispAlloc.md) was specified, the smart identifier manages the 3D display or picking context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the 3D display or picking context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `DispNum` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DispFormat` *(in, AIL_CONST_TEXT_PTR)*

Reserved for future expansion and must be set to AIL_TEXT("`M_DEFAULT`").

### `InitFlag` *(in, AIL_INT64)*

Specifies the type of 3D display or picking context.

*For specifying the display type or picking context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EXCLUSIVE` | Specifies to present the 3D display full-screen, without a windowed border, in one of the Windows desktop screens.

To use an exclusive display, your Windows desktop should be using more than one screen. Allocating an exclusive display on the main screen might not be convenient; if you do so, the Aurora Imaging Library exclusive display will hide third-party applications designed to start on the main screen and hide the Windows taskbar. |
| `M_PICKING_AREA_CONTEXT` | Specifies to allocate a picking area context that can be used with [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md) to pick a 3D graphic in a given rectangular region in a 3D display. |
| `M_PICKING_CONTEXT` | Specifies to allocate a picking context that can be used with [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) to pick a 3D graphic at a given position in a 3D display. |
| `M_WEB` | Specifies to allocate a 3D display that is presented upon selection in one or more instances of an Aurora Imaging Web Library client application (for example, running in a compatible web browser).

To use an Aurora Imaging Web Library display, you must enable Aurora Imaging Web Library server functionality in your application using [`MappControl`](../../Reference/app/MappControl.md)with [`M_WEB_CONNECTION`](../../Reference/app/MappControl.md). You must also publish the display using [`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md).

An Aurora Imaging Web Library display cannot be presented by your Aurora Imaging Library application. It can only be presented by a connected instance of a client application (running on the local computer or a remote computer). In the client application, the 3D display is treated as thought it were a 2D display (for example, it has the object type `AIWL.M_DISPLAY` and you control it using [AIWL.MdispControl](../../UserGuide/C58_Using_AIWL_to_monitor_your_application/S05_JavaScript_AIWL_function_reference.md) or [AIWL::MdispControl](../../UserGuide/C58_Using_AIWL_to_monitor_your_application/S07_C_AIWL_function_reference.md)) See [Fundamentals for creating your Aurora Imaging Web Library client application](../../UserGuide/C58_Using_AIWL_to_monitor_your_application/S03_Using_AIWL_to_monitor_your_application.md) for more information. |
| `M_WINDOWED` *(default)* | Specifies to allocate a 3D display that is presented upon selection in its own window on the Windows desktop screen(s). |

### `Disp3dIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D display or picking context identifier or specifies the data type that the function should use to return the 3D display or picking context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D display or picking
                              context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D display or picking
                              context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DDISP_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D display or picking
                              context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the 3D display identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D display.

If allocation fails, [`M_NULL`](../../Reference/3ddisp/M3ddispAlloc.md) is written as the identifier. |
| `Address in which to write the picking area context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated picking area context.

If allocation fails, [`M_NULL`](../../Reference/3ddisp/M3ddispAlloc.md) is written as the identifier. |
| `Address in which to write the picking context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated picking context.

If allocation fails, [`M_NULL`](../../Reference/3ddisp/M3ddispAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D display or picking context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DDISP_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3ddisp/M3ddispAlloc.md) was specified).
