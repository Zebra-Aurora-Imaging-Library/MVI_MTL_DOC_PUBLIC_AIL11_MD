---
doctype: Reference
module: gra
function: MgraAllocList
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraAllocList"
---

# MgraAllocList

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

> Allocate a 2D graphics list.

## Syntax

```c
AIL_ID MgraAllocList(
    AIL_ID    SysId,        //in
    AIL_INT64 ListGraType,  //in
    AIL_ID *  ListGraIdPtr  //out
)
```

## Description

This function allocates a 2D graphics list, which holds graphics in vector (mathematical) form. Different 2D graphics lists can coexist; use their identifier to specify which to use or change.

The 2D graphics list can be used to annotate the display non-destructively, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md). Since graphics in a 2D graphics list are vector-based, you can zoom the display without the pixelation (loss of clarity) of the graphics. Also, if you add, modify, or delete graphics, the display can be immediately updated to reflect the changes. To set whether the display is automatically updated, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_UPDATE_GRAPHIC_LIST`](../../Reference/disp/MdispControl.md).

The 2D graphics list can also be used to draw destructively in an image, using [`MgraDraw`](../../Reference/gra/MgraDraw.md). Once drawn, the graphics are raster-based and are part of the image buffer; they cannot be modified. Also, if this annotated image is displayed, zooming it will cause pixelation (loss of clarity) of the graphics and changes in line thickness. Using [`MgraDraw`](../../Reference/gra/MgraDraw.md) is similar to using a graphics function (for example, [`MgraArc`](../../Reference/gra/MgraArc.md)) to draw destructively in an image.

Graphics can be added to the list using any of the [`Mgra...`](../../Reference/gra/MgraAlloc.md) graphics functions, such as [`MgraArc`](../../Reference/gra/MgraArc.md). To remove all graphics from the list, use [`MgraClear`](../../Reference/gra/MgraClear.md). You can also use [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_DELETE`](../../Reference/gra/MgraControlList.md) to remove graphics (one or all) from the list.

When you add graphics to the list, they inherit the current settings of the specified 2D graphics context (as described in [`MgraAlloc`](../../Reference/gra/MgraAlloc.md)); however, there is no link between the 2D graphics list and the 2D graphics context. Graphics already in the list are not affected by changes made to the context. To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) and [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

When the 2D graphics list is no longer required, release it using[`MgraFree`](../../Reference/gra/MgraFree.md)unless [`M_UNIQUE_ID`](../../Reference/gra/MgraAllocList.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/gra/MgraAllocList.md) was specified, the smart identifier manages the 2D graphics list's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the 2D graphics list.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ListGraType` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ListGraIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 2D graphics list identifier or specifies the data type that the function should use to return the 2D graphics list identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 2D graphics list; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 2D graphics list; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_GRA_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 2D graphics list (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 2D graphics list.

If allocation fails, [`M_NULL`](../../Reference/gra/MgraAllocList.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 2D graphics list identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_GRA_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/gra/MgraAllocList.md) was specified).
