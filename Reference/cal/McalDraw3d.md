---
doctype: Reference
module: cal
function: McalDraw3d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalDraw3d"
---

# McalDraw3d

> Draw 3D annotations based on a calibrated or corrected image.

## Syntax

```c
AIL_INT64 McalDraw3d(
    AIL_ID    OperationDraw3dContextCalId,  //in
    AIL_ID    ContextCalOrImageBufId,       //in
    AIL_INT64 SrcIndex,                     //in
    AIL_ID    DstList3dgraId,               //out
    AIL_INT64 DstParentLabel,               //in
    AIL_ID    RelXYPlaneTextureImageBufId,  //in
    AIL_INT64 ControlFlag                   //in
)
```

## Description

This function draws 3D annotations (for example, the camera's coordinate system) based on a 3D draw calibration context, in a 3D graphics list. Set the draw operations and options for the draw using [`McalControl`](../../Reference/cal/McalControl.md). Example operations include drawing the relative XY plane and the frustum of the camera's view.

## Parameters

### `OperationDraw3dContextCalId` *(in, AIL_ID)*

Specifies the identifier of the 3D draw calibration context that specifies the annotations to draw and how to draw them. This parameter must be set to one of the following values:

*For specifying the 3D draw calibration context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 3D draw calibration context of the current Aurora Imaging Library application is used. |
| `3D draw calibration context identifier` | Specifies a valid 3D draw calibration context identifier, which you have allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/cal/McalAlloc.md). |

### `ContextCalOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context or calibrated image.

### `SrcIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. You can specify a 3D graphics list that you have previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to be used as the annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/cal/McalDraw3d.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `RelXYPlaneTextureImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the texture image, which you can apply to the relative XY plane to enhance its visibility. The texture image is typically a 2D image of the 3D scene. For example, you can specify the image used for calibration.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the parent label of the graphics added to the graphics list.
