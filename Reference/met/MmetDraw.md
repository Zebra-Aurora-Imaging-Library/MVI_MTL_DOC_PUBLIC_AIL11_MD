---
doctype: Reference
module: met
function: MmetDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / met / MmetDraw"
---

# MmetDraw

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

> Draw specific features or tolerances of a metrology context or of a metrology result.

## Syntax

```c
void MmetDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextOrResultMetId,    //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   LabelOrIndex,            //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features or tolerances of a metrology context or of a metrology result, in the destination image buffer or 2D graphics list.

If you pass a context, [`MmetDraw`](../../Reference/met/MmetDraw.md) can only draw the specified feature/tolerance if the function can internally compute it without the need of a source image. This is possible if the feature/tolerance is ultimately based on constructed edgel features built with external edgels/points ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_EDGEL`](../../Reference/met/MmetAddFeature.md) and [`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md)) and the external edgels/points have been specified using [`MmetPut`](../../Reference/met/MmetPut.md), and/or if it is ultimately based on parametric features. Note that [`MmetDraw`](../../Reference/met/MmetDraw.md) does not modify the context with internally calculated results.

You can draw results and settings, obtained relative to an offset, at the top-left corner of the destination image, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md) and zoom them using [`MgraControl`](../../Reference/gra/MgraControl.md) with[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md). For more information, see [Drawing with an offset and scale](../../UserGuide/C26_Generating_graphics/S04_Drawing_graphics.md).

*[Image: MmetDrawZooming.png]*

When zooming, [`MmetDraw`](../../Reference/met/MmetDraw.md) will draw into the destination image buffer even if the buffer is not large enough to contain all of the zoomed image. If necessary, the image will be clipped.

If a template reference has been set using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_TEMPLATE_REFERENCE_ID`](../../Reference/met/MmetControl.md), [`MmetDraw`](../../Reference/met/MmetDraw.md) can draw the results of features and tolerances extracted from that template reference. You will need to inquire [`M_TEMPLATE_REFERENCE_SIZE_X`](../../Reference/met/MmetInquire.md) and [`M_TEMPLATE_REFERENCE_SIZE_Y`](../../Reference/met/MmetInquire.md) to allocate an appropriate destination buffer.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultMetId` *(in, AIL_ID)*

Specifies the metrology result buffer or context from which to extract the features to draw. The metrology result buffer must have been previously allocated using [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md). The metrology context must have been previously allocated using [`MmetAlloc`](../../Reference/met/MmetAlloc.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of drawing operation to perform. This parameter can be set to the following values:

*For drawing a context characteristic*

| Value | Description |
| --- | --- |
| `M_DRAW_TEMPLATE_REFERENCE` | Draws the template reference. The [`LabelOrIndex`](../../Reference/met/MmetDraw.md) parameter must be set to [`M_GENERAL`](../../Reference/met/MmetDraw.md).

> **Note:** This operation cannot draw in a 2D graphics list. |

*For drawing a feature characteristic*

| Value | Description |
| --- | --- |
| `M_DRAW_ACTIVE_EDGELS` | Draws only the edgels that satisfied all edgel constraints. |
| `M_DRAW_ALL_EDGELS` | Draws all the edgels that were calculated in the target image. This includes the edgels that were used to measure a feature. |
| `M_DRAW_FEATURE` | Draws the specified feature(s). In this case, you can also set the [`LabelOrIndex`](../../Reference/met/MmetDraw.md) parameter to [`M_GLOBAL_FRAME`](../../Reference/met/MmetDraw.md). |
| `M_DRAW_FITTED_EDGELS` | Draws only the edgels that satisfied all edgel constraints and fit constraints. The fitted edgels are those used by the fit operation to define a feature. |
| `M_DRAW_NOISY_EDGELS` | Draws the edgels before they were denoised. Edgels are denoised using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_DENOISING_MODE`](../../Reference/met/MmetControl.md). |
| `M_DRAW_REGION` | Draws the feature's search region.

When drawing in a 2D graphics list, this operation cannot draw ring and ring sector regions when [`M_REGION_ACCURACY_HIGH`](../../Reference/met/MmetControl.md) is set to [`M_DISABLE`](../../Reference/met/MmetControl.md) and the 2D graphics context's input units is set to world units ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) set to [`M_WORLD`](../../Reference/gra/MgraControl.md)). |

*For drawing a tolerance characteristic*

| Value | Description |
| --- | --- |
| `M_DRAW_TOLERANCE` | Draws the geometric tolerance's icon. To view the icons, see [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md). |
| `M_DRAW_TOLERANCE_AREA` | Draws the area(s) used to define the area-based geometric tolerance. These tolerances are set using [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md) with [`M_AREA_...`](../../Reference/met/MmetAddTolerance.md). |
| `M_DRAW_TOLERANCE_AREA_NEGATIVE` | Draws the negative area(s) used to define the area-based geometric tolerance. Positive and negative areas occur when areas exist on both sides of a curve. The negative areas are those located on the side of the curve where the sum of the areas is the smallest. These tolerances are set using [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md) with [`M_AREA_...`](../../Reference/met/MmetAddTolerance.md).

Note that what is considered the positive side changes if [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md) is enabled. In this case, the positive and negative sides are determined by whether the global reference frame's Y-axis initially intersects with either the first or second curve (edgel feature) that is specified with the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance. |
| `M_DRAW_TOLERANCE_AREA_POSITIVE` | Draws the positive area(s) used to define the area-based geometric tolerance. Positive and negative areas occur when areas exist on both sides of a curve. The positive areas are those located on the side of the curve where the sum of the areas is the largest. These tolerances are set using [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md) with [`M_AREA_...`](../../Reference/met/MmetAddTolerance.md).

Note that what is considered the positive side changes if [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md) is enabled. In this case, the positive and negative sides are determined by whether the global reference frame's Y-axis initially intersects with either the first or second curve (edgel feature) that is specified with the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance. |
| `M_DRAW_TOLERANCE_FEATURES` | Draws the features used to define a tolerance. The [`LabelOrIndex`](../../Reference/met/MmetDraw.md) parameter can be set to **M_TOLERANCE_LABEL()**, **M_TOLERANCE_INDEX()**, [`M_ALL_TOLERANCES`](../../Reference/met/MmetDraw.md), [`M_ALL_PASS_TOLERANCES`](../../Reference/met/MmetDraw.md), [`M_ALL_FAIL_TOLERANCES`](../../Reference/met/MmetDraw.md), or [`M_ALL_WARNING_TOLERANCES`](../../Reference/met/MmetDraw.md). |

*For*

| Value | Description |
| --- | --- |
| `M_DRAW_LABEL` | Specifies that the label will also be drawn. |
| `M_DRAW_NAME` | Specifies that the name will also be drawn. |

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of the result in the metrology result buffer for which to perform the drawing operation, or specifies that you are performing a general drawing operation for the metrology context. Set this parameter to one of the following values:

*For specifying the index of the element in the metrology result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default setting.

[`M_DEFAULT`](../../Reference/met/MmetDraw.md) is the same as [`M_ALL_FEATURES`](../../Reference/met/MmetDraw.md), when setting the [`Operation`](../../Reference/met/MmetDraw.md) parameter to a value in the [`FeatureOperations`](../../Reference/FeatureOperations.md) table.

[`M_DEFAULT`](../../Reference/met/MmetDraw.md) is the same as [`M_ALL_TOLERANCES`](../../Reference/met/MmetDraw.md), when setting the [`Operation`](../../Reference/met/MmetDraw.md) parameter to a value in the [`ToleranceOperations`](../../Reference/ToleranceOperations.md) table.

[`M_DEFAULT`](../../Reference/met/MmetDraw.md) is the same as [`M_GENERAL`](../../Reference/met/MmetDraw.md), when setting the [`Operation`](../../Reference/met/MmetDraw.md) parameter to a value in the [`ContextOperations`](../../Reference/ContextOperations.md) table. |
| `M_FEATURE_INDEX` | Specifies the index value of an existing individual feature to draw, in either the metrology context or result buffer, depending on whether you provide a context or result buffer to the [`ContextOrResultMetId`](../../Reference/met/MmetDraw.md) parameter. |
| `M_FEATURE_LABEL` | Specifies the label value of an existing individual feature to draw, in either the metrology context or result buffer, depending on whether you provide a context or result buffer to the [`ContextOrResultMetId`](../../Reference/met/MmetDraw.md) parameter. |
| `M_TOLERANCE_INDEX` | Specifies the index value of an existing individual tolerance to draw, in either the metrology context or result buffer, depending on whether you provide a context or result buffer to the [`ContextOrResultMetId`](../../Reference/met/MmetDraw.md) parameter. |
| `M_TOLERANCE_LABEL` | Specifies the label value of an existing individual tolerance to draw, in either the metrology context or result buffer, depending on whether you provide a context or result buffer to the [`ContextOrResultMetId`](../../Reference/met/MmetDraw.md) parameter. |
| `M_ALL_FAIL_TOLERANCES` | Specifies that the drawing operation will be performed on all tolerance results that have the status [`M_FAIL`](../../Reference/met/MmetGetResult.md). |
| `M_ALL_FEATURES` | Specifies to draw all feature results. |
| `M_ALL_PASS_FEATURES` | Specifies that the drawing operation will be performed on all feature results that have the status [`M_PASS`](../../Reference/met/MmetGetResult.md). |
| `M_ALL_PASS_TOLERANCES` | Specifies that the drawing operation will be performed on all tolerance results that have the status [`M_PASS`](../../Reference/met/MmetGetResult.md). |
| `M_ALL_TOLERANCES` | Specifies to draw all tolerance results. |
| `M_ALL_WARNING_FEATURES` | Specifies that the drawing operation will be performed on all feature results that have the status [`M_WARNING`](../../Reference/met/MmetGetResult.md). |
| `M_ALL_WARNING_TOLERANCES` | Specifies that the drawing operation will be performed on all tolerance results that have the status [`M_WARNING`](../../Reference/met/MmetGetResult.md). |
| `M_GENERAL` | Specifies to draw a setting of the context. |
| `M_GLOBAL_FRAME` | Specifies to draw the global frame of the context. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
