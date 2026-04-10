---
doctype: Reference
module: edge
function: MedgeCalculate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeCalculate"
---

# MedgeCalculate

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

> Perform edge extraction and feature calculations.

## Syntax

```c
void MedgeCalculate(
    AIL_ID    ContextId,       //in
    AIL_ID    SourceImageId,   //in
    AIL_ID    SourceDeriv1Id,  //in
    AIL_ID    SourceDeriv2Id,  //in
    AIL_ID    SourceDeriv3Id,  //in
    AIL_ID    EdgeResultId,    //out
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function extracts edges and calculates edge features in the source image buffer or in the derivative image buffers. You can also use this function to perform post-calculations on included edges of an Edge Finder result buffer.

Results are stored in the specified Edge Finder result buffer. Select which edge features to calculate with [`MedgeControl`](../../Reference/edge/MedgeControl.md). Include, exclude, or delete edges that meet a specified criterion with [`MedgeSelect`](../../Reference/edge/MedgeSelect.md). Note that to calculate new features and/or select features of a new set of edges, [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must be called after [`MedgeControl`](../../Reference/edge/MedgeControl.md) and/or [`MedgeSelect`](../../Reference/edge/MedgeSelect.md).

If the source image or derivative images are calibrated, results are calculated in the world coordinate system, otherwise they are calculated in the pixel coordinate system.

If the source image or derivative images are calibrated, you can retrieve results in real-world or in pixel units. However, in the presence of distortion, some results are meaningless when converted from real-world to pixel units (for example, Feret angles). For example, if an edge appears warped in the source image, but the camera calibration context of the source image compensates for this during the extraction, the resulting Feret angles are meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system.

Calculations are typically done on a source image buffer ([`SourceImageId`](../../Reference/edge/MedgeCalculate.md)), where [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) calculates the required derivative image buffers that are used to extract edges. In this case, the derivative image buffers ([`SourceDeriv1Id`](../../Reference/edge/MedgeCalculate.md), [`SourceDeriv2Id`](../../Reference/edge/MedgeCalculate.md), and [`SourceDeriv3Id`](../../Reference/edge/MedgeCalculate.md)) must be set to [`M_NULL`](../../Reference/edge/MedgeCalculate.md). However, if you specify your own derivative image buffers, the edge extraction process is done with them instead. In this case, [`SourceImageId`](../../Reference/edge/MedgeCalculate.md) must be set to [`M_NULL`](../../Reference/edge/MedgeCalculate.md). Note that, when specifying your own derivative image buffers, the majority of processing control settings remain in effect, except for filter settings (for example, [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md)).

When calculated by Edge Finder, derivative buffers are always consistent (for example, they have the same scaling factor) and are always normalized to 10-bit images such that the maximum output value is 1024 for 8-bit source images. When you provide the derivative buffers, they should respect these limitations for best performance.

To avoid repeatedly calculating time-consuming edge features for all edges in a source image, [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) can also perform post-calculations on included edges of an Edge Finder result buffer. That is, once the results of a calculation have been stored in an Edge Finder result buffer, you can select a subset of edges from that Edge Finder result buffer (with [`MedgeSelect`](../../Reference/edge/MedgeSelect.md)) and/or add new features for calculation to that Edge Finder result buffer (with [`MedgeControl`](../../Reference/edge/MedgeControl.md)), and then perform the post-calculation with [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). In this case, processing time greatly decreases since calculations are not unnecessarily repeated. The operation of selecting edges and adding new features can be repeated until the required result is calculated. To perform post-calculations on the included edges of an Edge Finder result buffer, set [`SourceImageId`](../../Reference/edge/MedgeCalculate.md), [`SourceDeriv1Id`](../../Reference/edge/MedgeCalculate.md), [`SourceDeriv2Id`](../../Reference/edge/MedgeCalculate.md), and [`SourceDeriv3Id`](../../Reference/edge/MedgeCalculate.md) to [`M_NULL`](../../Reference/edge/MedgeCalculate.md).

Note that there are some restrictions when post-calculating edges; for more information, see [Post-calculation](../../UserGuide/C10_Edge_Finder/S07_Calculating_and_retrieving_results.md).

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the Edge Finder context to use for the extraction. The Edge Finder context must have been previously allocated on the required system using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md).

### `SourceImageId` *(in, AIL_ID)*

Specifies the source image buffer from which to extract edges. Typically, the source image buffer should be 1-band 8-bit unsigned. Other buffer depths and types are generally accepted, but can slightly decrease performance. Note that 3-band source image buffers are only supported when extracting edge contours. When the source image buffer is 32-bit float, Edge Finder uses floating-point precision calculations.

### `SourceDeriv1Id` *(in, AIL_ID)*

Specifies the X-source derivative image buffer used to extract edges. When the Edge Finder context type is [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md), [`SourceDeriv1Id`](../../Reference/edge/MedgeCalculate.md) specifies the first derivative of the image in the X-direction. When the Edge Finder context type is [`M_CREST`](../../Reference/edge/MedgeAlloc.md), [`SourceDeriv1Id`](../../Reference/edge/MedgeCalculate.md) specifies the second derivative of the image in the X-direction.

### `SourceDeriv2Id` *(in, AIL_ID)*

Specifies the Y-source derivative image buffer used to extract edges. When the Edge Finder context type is [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md), [`SourceDeriv2Id`](../../Reference/edge/MedgeCalculate.md) specifies the first derivative of the image in the Y-direction. When the Edge Finder context type is [`M_CREST`](../../Reference/edge/MedgeAlloc.md), [`SourceDeriv2Id`](../../Reference/edge/MedgeCalculate.md) specifies the second derivative of the image in the Y-direction.

### `SourceDeriv3Id` *(in, AIL_ID)*

Specifies the source cross-derivative image buffer used to extract edges. The cross-derivative can only be specified if the Edge Finder context type is [`M_CREST`](../../Reference/edge/MedgeAlloc.md). If the Edge Finder context type is [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md), set [`SourceDeriv3Id`](../../Reference/edge/MedgeCalculate.md) to `M_NULL`.

### `EdgeResultId` *(out, AIL_ID)*

Specifies the Edge Finder result buffer in which to write the results of the extraction. The Edge Finder result buffer must have been previously allocated on the required system using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether optimal performance during Edge Finder calculations is ensured. This parameter can only be used when providing derivative image buffers. When providing a source image buffer, optimal performance is always ensured; therefore, this parameter must be set to [`M_DEFAULT`](../../Reference/edge/MedgeCalculate.md).

*For the derivative image buffers*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that optimal performance is not ensured. |
| `M_NO_CHECK` | Specifies that optimal performance is ensured. [`M_NO_CHECK`](../../Reference/edge/MedgeCalculate.md) should only be used if each derivative image buffer has pixel values that do not go beyond the range of positive or negative 1024 (that is, 10-bit signed values). This ensures optimal performance since Edge Finder does not have to verify unnecessary values. |
