---
doctype: Reference
module: 3dmet
function: M3dmetDraw3d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetDraw3d"
---

# M3dmetDraw3d

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

> Draw the result of a 3D metrology operation into a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dmetDraw3d(
    AIL_ID    OperationDraw3dContext3dmetId,  //in
    AIL_ID    SrcResult3dmetId,               //in
    AIL_ID    DstList3dgraId,                 //out
    AIL_INT64 DstParentLabel,                 //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function draws the specified result of a fit or volume 3D metrology operation, stored in the specified 3D metrology result buffer, into a 3D graphics list.

For fit results, call this function directly to perform the draw. No allocated context or specified control settings are required. Fitted 3D geometries are drawn using the default settings of the 3D graphics list. For volume results, you typically allocate a draw 3D metrology context using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). You then set draw operations and options for the draw using [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) before calling [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md).

Prior to drawing results, the fit or volume operation must have completed successfully (that is, [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_STATUS_FIT`](../../Reference/3dmet/M3dmetGetResult.md) or [`M_STATUS_VOLUME`](../../Reference/3dmet/M3dmetGetResult.md) returns [`M_SUCCESS`](../../Reference/3dmet/M3dmetGetResult.md), respectively). For volume results, you must also have enabled [`M_SAVE_VOLUME_INFO`](../../Reference/3dmet/M3dmetControl.md) (using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) prior to performing the volume operation.

For fit results, this function can only draw the fitted 3D geometry. When drawing a fitted plane, this function will draw a 3D plane geometry clipped to the limits of the points that were used to fit the plane, unlike [`M3dgeoDraw3d`](../../Reference/3dgeo/M3dgeoDraw3d.md), which clips the 3D graphic using the 3D graphics list's clipping box.

For volume results, this function draws a meshed point cloud for diagnosing the volume computation. Use [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) to specify which volume elements to draw and how to draw them. A volume element is a triangular prism or pyramid that represents the computed 3D volume associated with one triangle of the source meshed point cloud. Pyramids occur for closed mesh volume calculations, since, in this case, the volume element is computed between a surface triangle and the origin of the working coordinate system. For a source depth map, the volume element is a rectangular prism. Note that instead of drawing individual volume elements, you can use [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) with [`M_VOLUME_ELEMENT_APPEARANCE`](../../Reference/3dmet/M3dmetControlDraw.md) to configure to draw only the surfaces of the source and reference objects; you can specify to draw the source object's surface, the reference object's surface, or both.

When drawing volumes, first set whether to draw all elements or a single volume element (using [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) with [`M_VOLUME_ELEMENT_INDEX`](../../Reference/3dmet/M3dmetControlDraw.md)). Then, enable the category of volume elements to draw, ether those that contributed positively to the volume calculation ([`M_DRAW_VOLUME_POSITIVE_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md)), those that contributed negatively ([`M_DRAW_VOLUME_NEGATIVE_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md)), or both ([`M_DRAW_VOLUME_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md)). Note that if you are drawing a single volume element and the corresponding category to which it belongs is not enabled, nothing will be drawn.

## Parameters

### `OperationDraw3dContext3dmetId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D metrology context that specifies the annotations to draw and how to draw them.

*For specifying the draw 3D metrology context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies a predefined draw 3D metrology context.

For volume results, the default context is predefined with all draw operations ([`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md)) set to use default settings.

For fit results, this is the only possible setting. Default settings of the 3D graphics list will be used. |
| `Draw 3D metrology context identifier` | Specifies a valid draw 3D metrology context identifier, which you have allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). |

### `SrcResult3dmetId` *(in, AIL_ID)*

Specifies the identifier of the fit or calculate 3D metrology result buffer, previously allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_FIT_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md) or [`M_CALCULATE_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md), respectively.

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. The 3D graphics list must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to use as the annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ROOT_NODE` *(default)* | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the 3D graphic added to the 3D graphics list.
