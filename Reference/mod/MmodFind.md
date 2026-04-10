---
doctype: Reference
module: mod
function: MmodFind
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodFind"
---

# MmodFind

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

> Search for the model(s) that are in the specified Model Finder context in a target image buffer, Edge Finder result buffer, or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer.

## Syntax

```c
void MmodFind(
    AIL_ID ContextId,              //in
    AIL_ID TargetImageOrResultId,  //in
    AIL_ID ModResultId             //out
)
```

## Description

This function searches for the model(s) that are in the specified Model Finder context, in the specified target image, Edge Finder result buffer, or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer; the latter case is only available when searching for the synthetic rectangular model of an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md)type of context. Results are stored in the specified Model Finder result buffer. The resulting values can be read using the [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) function.

For each model defined in your Model Finder context, you can restrict the region in which to find the reference position of the model (the search region) to a rectangular subsection of your target image; this is useful if you know that occurrences can only be found in one region of the target image. There are three methods to do so:

- You can manually define the search region using successive calls to [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md) set to [`M_ENABLE`](../../Reference/mod/MmodControl.md), [`M_POSITION_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_X`](../../Reference/mod/MmodControl.md), and [`M_POSITION_DELTA_POS_Y`](../../Reference/mod/MmodControl.md). For models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type Model Finder contexts, the [`M_ANGLE_REGION`](../../Reference/mod/MmodControl.md) control type is also available, which allows you to set your rectangular search region at a non-zero angle.
- You can call [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md) set to [`M_ENABLE`](../../Reference/mod/MmodControl.md), [`M_SEARCH_POSITION_FROM_GRAPHIC_LIST`](../../Reference/mod/MmodControl.md), and the identifier of a 2D graphics list. The 2D graphics list must contain one graphic of type rectangle, and have its input units set to pixels; Aurora Imaging Library will use its position and dimensions to define the search region. Note that for models in [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type Model Finder contexts, the rectangular graphic must be set with no angle, but for models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type contexts, it can be set at any angle.
- For models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type Model Finder contexts, you can call [`MmodFind`](../../Reference/mod/MmodFind.md) directly with a target image buffer that is associated with a rectangular region of interest at any angle.

Setting the angle of the search region does not set or affect the nominal search angle in any way; if you expect to find occurrences at an angle, you must also set [`M_ANGLE`](../../Reference/mod/MmodControl.md). Likewise, searching within a region at an angle has no effect on the angle of a found occurrence's reference axis returned by [`MmodGetResult`](../../Reference/mod/MmodGetResult.md).

When searching in an Edge Finder result buffer, [`MmodFind`](../../Reference/mod/MmodFind.md) will search for the models only in the included edges of the Edge Finder result buffer. The search is performed using both the global search settings of the Model Finder context and each model's current search settings (see [`MmodControl`](../../Reference/mod/MmodControl.md)).

If an occurrence has both a score and target score greater than or equal to its model's respective certainty levels (specified using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_CERTAINTY`](../../Reference/mod/MmodControl.md) and [`M_CERTAINTY_TARGET`](../../Reference/mod/MmodControl.md)), it is automatically considered an occurrence (default 80%); the remaining occurrences will be the best of those greater than or equal to the model's acceptance levels (specified using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ACCEPTANCE`](../../Reference/mod/MmodControl.md) and [`M_ACCEPTANCE_TARGET`](../../Reference/mod/MmodControl.md)).

The maximum number of occurrences found depends on the maximum number specified for the context (specified using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`Index`](../../Reference/mod/MmodControl.md) set to [`M_CONTEXT`](../../Reference/mod/MmodControl.md) and [`ControlType`](../../Reference/mod/MmodControl.md) set to [`M_NUMBER`](../../Reference/mod/MmodControl.md)). If it is set to [`M_ALL`](../../Reference/mod/MmodControl.md), [`MmodFind`](../../Reference/mod/MmodFind.md) tries to find the maximum number of occurrences specified for each model (specified using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`Index`](../../Reference/mod/MmodControl.md) set to an index value and [`ControlType`](../../Reference/mod/MmodControl.md) set to [`M_NUMBER`](../../Reference/mod/MmodControl.md)), greater than or equal to the acceptance levels.

Note, the Model Finder context must be preprocessed ([`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md)) before calling this function.

If you are using an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the model is considered to have the same units as the target. If the target is calibrated, the model will be interpreted in world units. Conversely, if the target does not have a camera calibration context associated with it, the model will be interpreted in pixel units. The model will be defined in the same coordinate system as the target. You cannot associate a calibration context with the model.

For other types of Model Finder contexts, their models can be calibrated. If their models are calibrated, you must specify a calibrated target image or Edge Finder results. Conversely, if the models are uncalibrated, the target must be uncalibrated as well. Note, however, that if you calibrate the models using [`McalUniform`](../../Reference/cal/McalUniform.md) to apply a uniform camera calibration of pixel size equal to 1, the target image or Edge Finder results need not be calibrated. Conversely, if the models are uncalibrated, they can be used with a calibrated target only if it has been calibrated using [`McalUniform`](../../Reference/cal/McalUniform.md) to apply a uniform camera calibration of pixel size equal to 1.

If the target image or Edge Finder results are calibrated, results are calculated in the world coordinate system; otherwise, they are calculated in the pixel coordinate system.

If the target image or Edge Finder results are calibrated, you can retrieve results in real-world or in pixel units. However, in the presence of distortion, some results are meaningless when converted from real-world to pixel units (for example, angle, scale, and transformation coefficient results). For example, if a model appears warped in the target image, but the camera calibration context of the target image compensates for this during the model search, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system.

For an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, if the target image or Edge Finder results are not calibrated and the nominal search aspect ratio is not set to 1 ([`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASPECT_RATIO`](../../Reference/mod/MmodControl.md)), the nominal search aspect ratio is taken into account horizontally when performing the search, adjusting the target pixel width to equal the pixel height (the model's aspect ratio is not adjusted). As such, results will be returned for this corrected target coordinate system.

If you are searching for model(s) in an Edge Finder result buffer, you can speed up the search by setting [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). For more information on speeding up your search, see [Interfacing with Geometric Model Finder or Advanced Geometric Matcher](../../UserGuide/C10_Edge_Finder/S10_Interfacing_with_the_Geometric_Model_Finder_module.md).

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the Model Finder context to use for the search. The Model Finder context must have been previously allocated on the system using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md).

### `TargetImageOrResultId` *(in, AIL_ID)*

Specifies the target image, Edge Finder result buffer, or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer in which to search for the model(s).

*For specifying the target in which to search*

| Value | Description |
| --- | --- |
| `Edge Finder result buffer identifier` | Specifies the identifier of the Aurora Imaging Library Edge Finder result buffer in which to search for the model(s). The Edge Finder result buffer must have been previously allocated on the required system using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md); [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have already been called. In addition, the Edge Finder result buffer must be compatible with the Model Finder context. Enable compatibility using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_MODEL_FINDER_COMPATIBLE`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). |
| `Image buffer identifier` | Specifies the identifier of the Aurora Imaging Library image buffer in which to search for the model(s). The target image must be a 1-band 8-bit unsigned image. The minimum and maximum target image sizes are 16x16 and 32768x32768 pixels, respectively. It is important to note that the minimum and maximum target image sizes are Aurora Imaging Library limits. To make sure that you have enough memory, you can call [`MmodFind`](../../Reference/mod/MmodFind.md); if you don't have enough memory, you will get an Aurora Imaging Library error.

If your Model Finder context is of type [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md), [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md), [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md), or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md), the target image buffer can have a rectangular region of interest (ROI) defined, with or without rotation. Otherwise, using an image buffer with an ROI will cause an error. The ROI defines the search region during the [`MmodFind`](../../Reference/mod/MmodFind.md) operation. This means that, while performing the search, [`MmodFind`](../../Reference/mod/MmodFind.md) ignores the [`M_POSITION_...`](../../Reference/mod/MmodControl.md) and [`M_ANGLE_REGION`](../../Reference/mod/MmodControl.md) control types. The region must be defined in vector format from a 2D graphics list ([`M_VECTOR`](../../Reference/buf/MbufInquire.md) or[`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error will be generated if the ROI is only in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md)) or a non-rectangular shape.

Searching within a rotated region does not affect the angle at which Aurora Imaging Library expects to find occurrences in any way; it only limits the location in which the reference position of the model can be found. If you expect to find occurrences at non-zero angles within a search region, you should specify the nominal search angle, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ANGLE`](../../Reference/mod/MmodControl.md). Likewise, searching within a region at an angle has no effect on the angle of a found occurrence's reference axis returned by [`MmodGetResult`](../../Reference/mod/MmodGetResult.md). |
| `Segment-specific Model Finder result buffer identifier` | Specifies the identifier of the [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer in which to search for a synthetic rectangular model; the model must be in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md)type of context.

The segment-specific result buffer must have been previously allocated on the required system using [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md) with [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md); [`MmodFind`](../../Reference/mod/MmodFind.md) must have already been called. In addition, target edges must have been stored in the result buffer (that is, [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SAVE_TARGET_EDGES`](../../Reference/mod/MmodControl.md) must have been enabled prior to calling [`MmodFind`](../../Reference/mod/MmodFind.md). |

### `ModResultId` *(out, AIL_ID)*

Specifies the result buffer in which to write the results of the search. If you are using an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, you need to specify a corresponding [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.
