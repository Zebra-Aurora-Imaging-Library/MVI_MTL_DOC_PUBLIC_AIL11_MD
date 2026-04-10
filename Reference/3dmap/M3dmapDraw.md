---
doctype: Reference
module: 3dmap
function: M3dmapDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapDraw"
---

# M3dmapDraw

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
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Yes |

> Generate an image from a 3D reconstruction context or result buffer.

## Syntax

```c
void M3dmapDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextOrResult3dmapId,  //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   LabelOrIndex,            //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of a 3D reconstruction context, or result buffer.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer ([`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object. This is valid as long as the [`LabelOrIndex`](../../Reference/3dmap/M3dmapDraw.md) parameter of the concurrent calls is set to a different index.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResult3dmapId` *(in, AIL_ID)*

Specifies the identifier of a 3D reconstruction context or result buffer.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw.

### `Operation` *(in, AIL_INT64)*

Specifies what to draw in the destination image buffer or 2D graphics list.

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_DRAW_REGION_INTERPOLATED` | Draws all regions (pixels) of the laser line image that were interpolated due to missing data (gaps) in the calibration laser lines. Although the laser line can appear in these regions and be associated with a valid height, the heights will be less accurate since they are interpolated.

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction context allocated with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) and calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). |
| `M_DRAW_REGION_INVERTED` | Draws all regions (pixels) of the laser line image where an inversion occurred. An inversion occurs when a reference plane associated with a lower height is found above a reference plane associated with a higher height. The laser line can still appear in these regions, however, results taken from these regions will be incorrect.

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction context allocated with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) and calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). |
| `M_DRAW_REGION_MISSING_DATA` | Draws all regions (pixels) of the laser line image where the laser line cannot appear, because of missing data (gaps) in the calibration laser lines that could not be accounted for by interpolation.

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction context allocated with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) and calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). |
| `M_DRAW_REGION_UNCALIBRATED` | Draws all regions (pixels) of the laser line image where the laser line cannot appear, because they are outside the calibrated region.

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction context allocated with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) and calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). |
| `M_DRAW_REGION_VALID` | Draws all regions (pixels) of the laser line image where the laser line can appear and be associated with a valid height.

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction context allocated with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) and calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). |

*For specifying an operation that can draw in a 2D graphics list*

| Value | Description |
| --- | --- |
| `M_DRAW_CALIBRATION_LINES` | Draws all the fitted laser lines used for calibrating the 3D reconstruction setup.

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction context allocated with [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) and calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). |
| `M_DRAW_CALIBRATION_PEAKS` | Draws all the extracted laser lines used for calibrating the 3D reconstruction setup.

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction context allocated with [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) and calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md).

> **Note:** If drawing in an image buffer, the 2D graphics context cannot have any zoom factors associated with it ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) or [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md) must be set to 1.0). |
| `M_DRAW_PEAKS_LAST` | Draws the laser line that would have produced the most recent results stored in the 3D reconstruction result buffer using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).

This drawing operation is only available if [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapDraw.md) is a 3D reconstruction result buffer, allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md). |

### `LabelOrIndex` *(in, AIL_INT)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
