---
doctype: Reference
module: 3dmap
function: M3dmapDraw3d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapDraw3d"
---

# M3dmapDraw3d

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

> Draw 3D annotations based on a laser line profiling 3D reconstruction context.

## Syntax

```c
AIL_INT64 M3dmapDraw3d(
    AIL_ID    OperationDraw3dContext3dmapId,  //in
    AIL_ID    SrcReconContext3dmapId,         //in
    AIL_INT64 SrcIndex,                       //in
    AIL_ID    DstList3dgraId,                 //out
    AIL_INT64 DstParentLabel,                 //in
    AIL_ID    LaserPlaneTextureImageBufId,    //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function draws 3D annotations (for example, the camera's coordinate system) based on a laser line profiling 3D reconstruction context, in a 3D graphics list. Set the draw operations and options for the draw using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md). Example operations include drawing the laser plane and the frustum of the camera's view.

## Parameters

### `OperationDraw3dContext3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D draw context that specifies the annotations to draw and how to draw them. This parameter must be set to one of the following values:

*For specifying the 3D draw context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 3D draw context of the current Aurora Imaging Library application is used. |
| `3D draw context identifier` | Specifies a valid 3D draw context identifier, which you have allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md). |

### `SrcReconContext3dmapId` *(in, AIL_ID)*

Specifies the identifier of the laser line profiling 3D reconstruction context, previously allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md) and [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md). The 3D reconstruction setup must have been successfully calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md).

### `SrcIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. You can specify a 3D graphics list that you have previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to be used as the annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dmap/M3dmapDraw3d.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `LaserPlaneTextureImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the texture image, which you can apply to the drawn laser plane to enhance its visibility. This is typically the image buffer containing the grabbed image of the laser line.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the parent label of the graphics added to the graphics list.
