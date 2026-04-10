---
doctype: Reference
module: agm
function: MagmFind
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmFind"
---

# MagmFind

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

> Find the specified model in the target image or Edge Finder result buffer.

## Syntax

```c
void MagmFind(
    AIL_ID    ContextAgmId,           //in
    AIL_ID    TargetImageOrResultId,  //in
    AIL_ID    ResultAgmId,            //out
    AIL_INT64 ControlFlag             //in
)
```

## Description

This function searches for the model, defined in the specified find AGM context, in the specified target image or Edge Finder result buffer. Results are stored in the specified find AGM result buffer. You can retrieve results using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md). Note that calling [`MagmFind`](../../Reference/agm/MagmFind.md) successively with target images of different sizes will slow the execution time.

This function only searches for single-definition models or trained composite-definition models. You can train a composite-definition model using [`MagmTrain`](../../Reference/agm/MagmTrain.md), and then copy the resulting model to a find AGM context using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md) with [`M_TRAINED_MODEL`](../../Reference/agm/MagmCopyResult.md).

Use [`MagmControl`](../../Reference/agm/MagmControl.md) to configure individual model settings before calling this function.

An occurrence is selected as a candidate if its coverage, fit, and detection scores are greater than or equal to the respective acceptance levels ([`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_ACCEPTANCE_COVERAGE`](../../Reference/agm/MagmControl.md), [`M_ACCEPTANCE_FIT`](../../Reference/agm/MagmControl.md), and [`M_ACCEPTANCE_DETECTION`](../../Reference/agm/MagmControl.md)).

This function returns results in decreasing coverage, fit, or detection score order, depending on [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_SORT_CANDIDATES_SCORE`](../../Reference/agm/MagmControl.md). This means that the most-likely occurrence is always returned first.

If you are searching for the model in an Edge Finder result buffer, [`MagmFind`](../../Reference/agm/MagmFind.md) will search for the model only in the included edges of the Edge Finder result buffer.

Note that AGM does not provide support for camera calibration. If you are searching for the model in a target image buffer, the target image buffer must be uncalibrated. If you are searching in an Edge Finder result buffer, the Edge Finder result buffer must be uncalibrated or [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) must be set to [`M_PIXEL`](../../Reference/edge/MedgeControl.md).

The AGM context must be preprocessed with [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md) before calling this function.

## Parameters

### `ContextAgmId` *(in, AIL_ID)*

Specifies the find AGM context identifier to use for the search. The AGM context must have been previously allocated on the system using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md).

### `TargetImageOrResultId` *(in, AIL_ID)*

Specifies the identifier of the target image or Edge Finder result buffer in which to find the model.

*For specifying the target in which to search*

| Value | Description |
| --- | --- |
| `Edge Finder result buffer identifier` | Specifies the identifier of the Aurora Imaging Library Edge Finder result buffer in which to find the model. The Edge Finder result buffer must have been previously allocated on the required system using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md); [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have already been called. In addition, the Edge Finder result buffer must be compatible with the AGM context. Enable compatibility using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_AGM_COMPATIBLE`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).

Note that because AGM does not support calibration, the Edge Finder result buffer must be uncalibrated or [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) must be set to [`M_PIXEL`](../../Reference/edge/MedgeControl.md).

This value is not available when [`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) is set to [`M_ENABLE`](../../Reference/agm/MagmControl.md). |
| `Image buffer identifier` | Specifies the identifier of the Aurora Imaging Library image buffer in which to find the model. The image buffer must be a 1-band 8-bit unsigned buffer. The minimum and maximum target image sizes are 6x6 and 32768x32768 pixels, respectively. It is important to note that the minimum and maximum target image sizes are Aurora Imaging Library limits. To make sure that you have enough memory, you can call [`MagmFind`](../../Reference/agm/MagmFind.md); if you don't have enough memory, you will get an Aurora Imaging Library error.

This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

Note that because AGM does not support calibration, the image buffer must be uncalibrated. |

### `ResultAgmId` *(out, AIL_ID)*

Specifies the identifier of the find AGM result buffer in which to store results. The result buffer must have been allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
