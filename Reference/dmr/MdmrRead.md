---
doctype: Reference
module: dmr
function: MdmrRead
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrRead"
---

# MdmrRead

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

> Read dot-matrix strings from a target image.

## Syntax

```c
void MdmrRead(
    AIL_ID    ContextDmrId,      //in
    AIL_ID    TargetImageBufId,  //in
    AIL_ID    ResultDmrId,       //out
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function uses a SureDotOCR context to read one or more dot-matrix strings from a target image. Results are stored in a SureDotOCR result buffer. To get the results, use [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md).

Requirements for reading strings depend on the settings of the context, and the font and string models therein. To control these settings, use [`MdmrControl`](../../Reference/dmr/MdmrControl.md), [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md), and [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md). [`MdmrRead`](../../Reference/dmr/MdmrRead.md) takes into account all settings.

Before performing a read operation, there must be at least one font, one character, and one string model in the context; it must also be in a preprocessed state ([`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md)).

To read a string, it must not only meet the context's requirements, but it must also follow the basic rules of a string. It must, for example, be a linear sequence of characters.

Strings are read in the natural Latin-based reading order; that is, from left to right, and from top to bottom (with respect to the angle of the target). A string's position is the position of the center of the bounding box of its first character.

You can limit the read operation to a region of the target image buffer, using a rectangular ROI set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

If the target image is not calibrated, results are calculated in the pixel coordinate system (pixel units). If the target image is calibrated, results are calculated in the world coordinate system (real-world units), but you can retrieve them in either world or pixel units. To specify this, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dmr/MdmrControl.md).

In the presence of distortion, some results are meaningless when converted from real-world to pixel units (such as angle and scale). For example, if a string appears warped in the target image, but the camera calibration context compensates for this, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system. If complex distortions exist, correct the image before using it with SureDotOCR.

The read operation can occasionally take an unexpectedly long time to calculate. Use [`MdmrControl`](../../Reference/dmr/MdmrControl.md)with [`M_TIMEOUT`](../../Reference/dmr/MdmrControl.md) to set a maximum read time.

## Parameters

### `ContextDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context to use for the read operation. The context must have been previously allocated on the required system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md), and preprocessed using [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md).

### `TargetImageBufId` *(in, AIL_ID)*

Specifies the identifier of the target image buffer which contains the dot-matrix strings to read. The target image buffer must be a 1-band, 8-bit unsigned buffer. Its maximum size is 65536x65536 pixels.

### `ResultDmrId` *(out, AIL_ID)*

Specifies the identifier of the SureDotOCR result buffer in which to write the results of the read operation. The SureDotOCR result buffer must have been previously allocated on the required system using [`MdmrAllocResult`](../../Reference/dmr/MdmrAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
