---
doctype: Reference
module: gra
function: MgraDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraDraw"
---

# MgraDraw

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

> Draw graphics, contained in a 2D graphics list, in an image.

## Syntax

```c
void MgraDraw(
    AIL_ID    GraListId,    //in
    AIL_ID    DestImageId,  //out
    AIL_INT64 ControlFlag   //in
)
```

## Description

This function draws graphics, contained in the specified 2D graphics list, destructively (raster-based) in the specified image.

A graphic's position and dimension values are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that you can also use [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md) to modify the input units of graphics that have already been added to the 2D graphics list.

If you attempt to draw the graphics contained in the 2D graphics list using world units, but without passing [`MgraDraw`](../../Reference/gra/MgraDraw.md) a calibrated image, the function will generate an error.

If parts of the graphics fall outside of the specified image buffer area, those parts are clipped off.

If you are drawing graphics in a 2D graphics list with interactive mode enabled ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md) set to [`M_ENABLE`](../../Reference/disp/MdispControl.md)), selected graphics (graphics with [`M_GRAPHIC_SELECTED`](../../Reference/gra/MgraControlList.md) set to [`M_TRUE`](../../Reference/gra/MgraControlList.md)) are typically surrounded by annotations that indicate the graphic is selected, such as the selection box (represented by a dotted line) with handles that allow user interaction. By default, this function will draw the annotations that indicate the graphic is selected, unless otherwise specified.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`GraListId`](../../Reference/gra/MgraDraw.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `GraListId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics list containing the graphics to destructively draw in the image. The 2D graphics list must have been previously allocated on the required system using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md).

### `DestImageId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to draw. If the 2D graphics list contains graphics set in world units, a camera calibration context must be associated with the destination image buffer; otherwise, an error is generated.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to also draw the annotations that indicate whether a graphic is selected, such as the selection box and handles. This parameter must be set to one of the following values:

*For specifying whether to draw annotations that indicate a graphic is selected*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that annotations that indicate whether a graphic is selected will be drawn destructively in the image buffer, along with the graphic. |
| `M_NO_INTERACTIVE_ANNOTATION` | Specifies that annotations that indicate whether a graphic is selected will not be drawn in the image buffer, even if there are selected graphics in the 2D graphics list. |
