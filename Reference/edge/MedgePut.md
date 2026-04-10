---
doctype: Reference
module: edge
function: MedgePut
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgePut"
---

# MedgePut

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

> Put edge chains from user-supplied arrays into an Edge Finder result buffer.

## Syntax

```c
void MedgePut(
    AIL_ID             ResultEdgeId,        //out
    AIL_INT            NumEdgels,           //in
    const AIL_INT *    ChainIndexArrayPtr,  //in
    const AIL_DOUBLE * PosXArrayPtr,        //in
    const AIL_DOUBLE * PosYArrayPtr,        //in
    const AIL_DOUBLE * AngleArrayPtr,       //in
    const AIL_DOUBLE * MagnitudeArrayPtr,   //in
    AIL_INT64          ControlFlag          //in
)
```

## Description

This function copies edge chains from user-supplied arrays to a specified Edge Finder result buffer. This can be useful if you want to construct an Edge Finder result buffer that cannot be obtained from one image. You can therefore combine results from multiple Edge Finder result buffers to, for example, build a complete model to add to a Model Finder context.

You must only add edge chains that respect the following constraints, whereby pixels are considered connected based on an 8-connected lattice:

- Consecutive edgels must occupy separate connected pixels.
- No branches are allowed (they start or end a separate chain).
- There must not be an edge in the edge map that is more than 1 pixel wide. This means that the first and fourth edgel in the edge chain must not be in neighboring pixels.

All edge chains must be at the same scale and this scale must be the scale at which they were originally extracted. If edges were extracted at a different scale ([`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md)), you must convert them to their original extraction scale.

If edgels are calculated with pixel accuracy, you must provide edgels with coordinates that are integer values; in this case, the distance between consecutive edgels should be one pixel. If edgels are calculated with subpixel accuracy, you can provide edgels with coordinates that are double values; in this case, the distance between consecutive edgels is such that, each pixel touched by the edge corresponds to one edgel. To change the accuracy edgels are calculated with, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_ACCURACY`](../../Reference/edge/MedgeControl.md).

To use an Edge Finder result buffer that contains edges set using [`MedgePut`](../../Reference/edge/MedgePut.md) with the Model Finder module, you must respect the following limitation. First, edges in the source image buffer must have been previously extracted and saved in the Edge Finder result buffer, using [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). Second, the coordinates of the edge chains in the user-supplied arrays must not exceed the boundaries of the source image buffer. You can inquire the width and height of the source image buffer using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_SIZE_X`](../../Reference/edge/MedgeGetResult.md) and [`M_SIZE_Y`](../../Reference/edge/MedgeGetResult.md).

The X- and Y-coordinates of the edgels you are adding to the Edge Finder result buffer must be provided in pixel units (as opposed to real-world values). If your current results have been calibrated, the coordinates added to those results will be automatically transformed to their appropriate real-world values. For more information, see [Working with real-world units](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

## Parameters

### `ResultEdgeId` *(out, AIL_ID)*

Specifies the identifier of the Edge Finder result buffer in which to put user-supplied data.

### `NumEdgels` *(in, AIL_INT)*

Specifies the number of edgels to add to the Edge Finder result buffer.

### `ChainIndexArrayPtr` *(in, *AIL_INT)*

Specifies the address of the array containing the indices of the edgels' edges to add to the Edge Finder result buffer. If you do not want to provide this information, set this parameter to `M_NULL`. In this case, only one edge will be added.

### `PosXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinate(s) of the edgel(s) to add to the Edge Finder result buffer.

### `PosYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinate(s) of the edgel(s) to add to the Edge Finder result buffer.

### `AngleArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the orientation of the edgel(s) to add to the Edge Finder result buffer. If you do not want to provide this information, set this parameter to `M_NULL`.

### `MagnitudeArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the magnitude of the edgel(s) to add to the Edge Finder result buffer. If you do not want to provide this information, set this parameter to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
