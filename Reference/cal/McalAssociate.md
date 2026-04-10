---
doctype: Reference
module: cal
function: McalAssociate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalAssociate"
---

# McalAssociate

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

> Associate/disassociate a camera calibration context with/from an image or digitizer.

## Syntax

```c
void McalAssociate(
    AIL_ID    SrcCalibrationOrAilId,     //in
    AIL_ID    TargetImageOrDigitizerId,  //out
    AIL_INT64 ControlFlag                //in
)
```

## Description

This function associates a camera calibration context with an image or digitizer. This function can also be used to disassociate a camera calibration context from an image or digitizer. Note that you do not have to first disassociate a camera calibration context from an image or digitizer to associate it with a different camera calibration context.

When you grab an image with a calibrated digitizer, the camera calibration context currently associated with the digitizer gets associated with the grabbed image.

When you associate a camera calibration context with an image using [`McalAssociate`](../../Reference/cal/McalAssociate.md) or by grabbing with a calibrated digitizer, the image receives a copy of the camera calibration context's current coordinate systems and a reference to the camera calibration context for all other settings. When you associate a camera calibration context with a digitizer, the digitizer only receives a reference to the camera calibration context.

When associating a camera calibration context with an image, its child buffers, if any, will also be associated with the same camera calibration context. However, any previous camera calibration information of the child buffers will be overwritten by the new context associated with the parent.

If you copy or process the calibrated image, the operation will typically copy the image's camera calibration settings to the destination image. If multiple source images are used for the operation, the destination image will only receive a copy of the camera calibration information if the source images have the same camera calibration information. Most functions propagate the calibration settings, while others clear this information. For more information, see [Camera calibration propagation](../../UserGuide/C28_Calibration/S08_Calibration_propagation.md).

If you associate a camera calibration context with an image buffer containing an ROI, all graphic elements in the 2D graphics list that were created in world units (using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) set to [`M_WORLD`](../../Reference/gra/MgraControl.md)) will be associated with the same callibration information as the image; all other graphic elements in the 2D graphics list, as well as any generated raster information, will not be calibrated. See [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) for more information.

## Parameters

### `SrcCalibrationOrAilId` *(in, AIL_ID)*

Specifies the camera calibration context to associate. This parameter can be set to one of the following. If an image buffer, digitizer or result buffer is passed to this parameter, the camera calibration context associated with that object is used; camera calibration settings saved with the image, digitizer or result are also copied.

*For specifying the camera calibration context to associate with*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to remove the association between the camera calibration context and the image or digitizer. |
| `Calibration context identifier` | Specifies the camera calibration context to associate with the target image or digitizer. |
| `Image buffer identifier` | Specifies the image whose camera calibration information and reference to a camera calibration context will be associated with the target image or digitizer.

The image buffer must be a 1- or 3-band buffer. |
| `Processing or analysis module result buffer identifier` | Specifies the processing or analysis module result whose camera calibration information and reference to a camera calibration context will be associated with the target image or digitizer. |

### `TargetImageOrDigitizerId` *(out, AIL_ID)*

Specifies the identifier of the image or digitizer with which to associate the camera calibration context.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to the following value:

*For specifying the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Sets the function's control to the default. |
