---
doctype: Reference
module: met
function: MmetCalculate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetCalculate"
---

# MmetCalculate

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

> Calculate features and validate geometric tolerances.

## Syntax

```c
void MmetCalculate(
    AIL_ID    ContextId,    //in
    AIL_ID    TargetBufId,  //in
    AIL_ID    ResultId,     //out
    AIL_INT64 ControlFlag   //in
)
```

## Description

This function calculates features and validates geometric tolerances. For physically measured features, [`MmetCalculate`](../../Reference/met/MmetCalculate.md) uses the target image's edge map. For more information on edge maps, see [Extracting the edges](../../UserGuide/C10_Edge_Finder/S04_Extracting_the_edges.md).

If a metrology context contains only constructed features, the [`TargetBufId`](../../Reference/met/MmetCalculate.md)parameter can be set to [`M_NULL`](../../Reference/met/MmetCalculate.md). This is useful, for example, when performing calculations on edgel features constructed from edgels or points established using other Aurora Imaging Library modules or third party software.

To control the Metrology module's calculation and validation behavior, you can set context constraints using [`MmetControl`](../../Reference/met/MmetControl.md). You can also use [`MmetControl`](../../Reference/met/MmetControl.md) to alter settings for features and tolerances that you have already added to the context. To add features to the context, use [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md). To add geometric tolerances to the context and define geometric relationships between features, use [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md).

Results of a metrology calculation are stored in the specified result buffer and can be obtained using [`MmetGetResult`](../../Reference/met/MmetGetResult.md).

If the target image is not calibrated, results are calculated in the pixel coordinate system (pixel units). If the target image is calibrated, results are calculated in the world coordinate system (real-world units), but they can be retrieved in either world or pixel units. Note that, if the target image is calibrated, feature values, metrology regions, and tolerances are considered as real-world values and are retrieved in world units.

Note that in the presence of distortion, some results are meaningless when converted from real-world to pixel units (for example, angle and scale results). For example, if a feature appears warped in the target image, but the camera calibration context compensates for this during the calculation, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system.

If complex distortions exist, you must first correct the image before using it with the Metrology module.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the metrology context. The metrology context must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md).

### `TargetBufId` *(in, AIL_ID)*

Specifies the identifier of the target image buffer from which to extract edges to calculate physically measured features and validate geometric tolerances. If a metrology context contains features which are only constructed, you can set this parameter to `M_NULL`. If you don't provide a target image and the context contains any measured features, an error is generated.

### `ResultId` *(out, AIL_ID)*

Specifies the metrology result buffer in which to write the results of the calculation. The metrology result buffer must have been previously allocated on the required system using [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
