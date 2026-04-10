---
doctype: Reference
module: mod
function: MmodInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / mod / MmodInquire"
---

# MmodInquire

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

> Inquire information about a specified Model Finder context, a specified model, or a specified result buffer.

## Syntax

```c
AIL_INT MmodInquire(
    AIL_ID    ContextOrResultId,  //in
    AIL_INT   Index,              //in
    AIL_INT64 InquireType,        //in
    void *    UserVarPtr          //out
)
```

## Description

This function inquires information about a specified Model Finder context, the models contained therein, or a specified Model Finder result buffer. Note that Model Finder result buffer values can be obtained with [`MmodGetResult`](../../Reference/mod/MmodGetResult.md).

In general, returned values (for example, position and width) are in the following units:

- For a non-synthetic model, returned values are in the pixel coordinate system, even if the target is calibrated. The only exception is when inquiring about angles (for example, [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodInquire.md)); in these cases, the angles values are in the relative coordinate system if the target is calibrated.
- For a synthetic model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context, returned values are in the user-defined units for the model; their relationship to pixels is defined using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_PIXEL_SCALE`](../../Reference/mod/MmodInquire.md). If the target is not calibrated, [`M_PIXEL_SCALE`](../../Reference/mod/MmodInquire.md) is used for the match. If the target is calibrated, model units are used; note that these should be the same as the calibrated units. In a few cases, instead of returning values in model units, values are returned in pixel units (for example, [`M_POSITION_...`](../../Reference/mod/MmodInquire.md)); these cases are clearly indicated.
- For a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) context, returned values are in model units, which in this case are the same as the target units. If the target is calibrated, the model's settings will be interpreted in world units. If the target is not calibrated, the model's settings will be interpreted in pixel units.

If an inquire type is set to [`M_DEFAULT`](../../Reference/mod/MmodControl.md), [`MmodInquire`](../../Reference/mod/MmodInquire.md) will return [`M_DEFAULT`](../../Reference/mod/MmodControl.md). To inquire its default value, add [`M_DEFAULT`](../../Reference/mod/MmodInquire.md) to the inquire type.

## Parameters

### `ContextOrResultId` *(in, AIL_ID)*

Specifies the Model Finder context or Model Finder result buffer about which to inquire information. The Model Finder context or result buffer must have been previously allocated on the required system using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) or [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md), respectively.

### `Index` *(in, AIL_INT)*

Specifies that information will be inquired about the Model Finder context, an individual model, or a Model Finder result buffer. This parameter should be set to one of the following values:

*For specifying a context, model, or result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default.

If a Model Finder context is specified, this parameter inquires information about model index 0.

If a result buffer is specified, same as [`M_GENERAL`](../../Reference/mod/MmodInquire.md). |
| `M_CONTEXT` | Inquires information about the Model Finder context, which has been set using the [`ContextOrResultId`](../../Reference/mod/MmodInquire.md) parameter. |
| `M_GENERAL` | Inquires general information about the Model Finder result buffer, which has been set using the [`ContextOrResultId`](../../Reference/mod/MmodInquire.md) parameter. |
| `0 <= IndexValue < M_NUMBER_MODELS` | Specifies the index of the individual model to inquire, if [`ContextOrResultId`](../../Reference/mod/MmodInquire.md) is set to a Model Finder context.

To retrieve the index of a model from its user label, set this parameter to the user label and set the inquire type to [`M_INDEX_FROM_LABEL`](../../Reference/mod/MmodInquire.md). |
| `LabelValue` | Specifies the user label of the individual model to inquire, if [`ContextOrResultId`](../../Reference/mod/MmodInquire.md) is set to a Model Finder context. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MmodInquire`](../../Reference/mod/MmodInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about the number of occurrences to find for the entire context or an individual model specified using its index

For [`M_CONTEXT`](../../Reference/mod/MmodInquire.md) or an individual model specified using its index, the [`InquireType`](../../Reference/mod/MmodInquire.md) parameter can be set to one of the following:

---

### `M_NUMBER`

Inquires the number of models for which to search.  For [`M_CONTEXT`](../../Reference/mod/MmodInquire.md), this setting returns the maximum number of occurrences of all models to find in the target.  For a model, this setting returns the maximum number of occurrences of the specified model to find in the target.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value. |
| `M_ALL` | Specifies to find all occurrences. |
| `Value > 0` | Specifies the number of occurrences. |

### For inquiring about the context or result buffer

To inquire about the system on which the Model Finder context or result buffer has been allocated, set the [`InquireType`](../../Reference/mod/MmodInquire.md) parameter to the value below.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the Model Finder context or result buffer was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For inquiring about the context

For [`M_CONTEXT`](../../Reference/mod/MmodInquire.md), the [`InquireType`](../../Reference/mod/MmodInquire.md) parameter can be set to one of the following:

---

### `M_ACCURACY`

Inquires the search accuracy.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies high accuracy. |
| `M_LOW` | Specifies low accuracy. |
| `M_MEDIUM` *(default)* | Specifies medium accuracy. |

---

### `M_ACTIVE_EDGELS`

Inquires the degree to which Aurora Imaging Library processes active edgels in the target.  This inquire type is only supported for a geometric Model Finder context ([`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the degree to which Aurora Imaging Library processes target edgels, as a percentage. |

---

### `M_ASPECT_RATIO`

Inquires the aspect ratio applied to the target before starting the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.5 <= Value <= 2.0` *(default)* | Specifies the pixel width/pixel height of the target. |

---

### `M_CONTEXT_TYPE`

Inquires the type of search context allocated.

| Value | Description |
| --- | --- |
| `M_GEOMETRIC` | Specifies that the Model Finder context uses a general geometric search algorithm. |
| `M_GEOMETRIC_CONTROLLED` | Specifies that the Model Finder context uses a controlled geometric search algorithm. |
| `M_SHAPE_CIRCLE` | Specifies that the Model Finder context uses a circular model search algorithm. |
| `M_SHAPE_ELLIPSE` | Specifies that the Model Finder context uses an elliptical model search algorithm. |
| `M_SHAPE_RECTANGLE` | Specifies that the Model Finder context uses a rectangular model search algorithm. |
| `M_SHAPE_SEGMENT` | Specifies that the Model Finder context uses a segment model search algorithm. |

---

### `M_DETAIL_LEVEL`

Inquires the level of details to extract from the model source and target image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies a high level of detail. |
| `M_MEDIUM` *(default)* | Specifies a medium level of detail. |
| `M_VERY_HIGH` | Specifies a very high level of detail. |

---

### `M_EDGE_ACCURACY`

Inquires the edge accuracy used when calculating the edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` *(default)* | Specifies a high level of edge accuracy. |
| `M_VERY_HIGH` | Specifies a very high level of edge accuracy. |

---

### `M_FIRST_LEVEL`

Inquires the resolution level for the initial stage of the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the first resolution is determined automatically. |
| `0 <= Value <= 7` | Specifies the resolution level. |

---

### `M_LAST_LEVEL`

Inquires the resolution level for the final stage of the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the last resolution is determined automatically. |
| `0 <= Value <= 7` | Specifies the resolution level. |

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_NUMBER_MODELS`

Inquires the number of models in the context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of models. |

---

### `M_PREPROCESSED`

Inquires whether the Model Finder context is preprocessed. The context must be preprocessed (using [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md)) before calling [`MmodFind`](../../Reference/mod/MmodFind.md). After certain settings of the Model Finder context are changed with [`MmodControl`](../../Reference/mod/MmodControl.md), or after a model is added or removed with [`MmodDefine`](../../Reference/mod/MmodDefine.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

---

### `M_RESOLUTION_COARSENESS_LEVEL`

Inquires the resolution of the edge approximation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the resolution. |

---

### `M_SAVE_TARGET_EDGES`

Inquires whether the target edges in the result will be saved.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that target edges cannot be saved. |
| `M_ENABLE` | Specifies that target edges can be saved. |

---

### `M_SEARCH_ANGLE_RANGE`

Inquires whether to perform calculations specific to angular-range search strategies.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value. |
| `M_DISABLE` | Specifies that calculations specific to angular-range search strategies are disabled. |
| `M_ENABLE` | Specifies that calculations specific to angular-range search strategies are enabled. |

---

### `M_SEARCH_POSITION_RANGE`

Inquires whether to perform calculations specific to position-range search strategies.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that calculations specific to position-range search strategies are disabled. |
| `M_ENABLE` *(default)* | Specifies that calculations specific to position-range search strategies are enabled. |

---

### `M_SEARCH_SCALE_RANGE`

Inquires whether to perform calculations specific to scale-range search strategies.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that calculations specific to scale-range search strategies are disabled. |
| `M_ENABLE` | Specifies that calculations specific to scale-range search strategies are enabled. |

---

### `M_SHARED_EDGES`

Inquires whether multiple occurrences can share edges.  This inquire type is not supported for a model defined in an[`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md), an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md), or an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md)type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the sharing of edges. |
| `M_ENABLE` | Specifies to enable the sharing of edges. |

---

### `M_SMOOTHNESS`

Inquires the degree of smoothness (denoising) applied to the model images and the target images during edge extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value applied to the images. |

---

### `M_SPEED`

Inquires the search speed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies a high speed. |
| `M_LOW` | Specifies a low speed. |
| `M_MEDIUM` *(default)* | Specifies a medium speed. |
| `M_VERY_HIGH` | Specifies a very high speed. |

---

### `M_TARGET_CACHING`

Inquires whether target caching is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable target caching. |
| `M_ENABLE` | Specifies to enable target caching in the result. |

---

### `M_TIMEOUT`

Inquires the maximum search time for [`MmodFind`](../../Reference/mod/MmodFind.md), in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies an infinite amount of search time. |
| `Value >= 0.0` *(default)* | Specifies the maximum search time, in msecs. |

### For inquiring about a model using its index

For inquiring about an individual model specified using its index, the [`InquireType`](../../Reference/mod/MmodInquire.md) parameter can be set to one of the following:

---

### `M_ACCEPTANCE`

Inquires the acceptance level for the score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

---

### `M_ACCEPTANCE_TARGET`

Inquires the acceptance level for the target score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable target score, as a percentage. |

---

### `M_ALLOC_OFFSET_X`

Inquires the X-offset of the model in the model source image.  This is only supported for image-type or Edge Finder-type models.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset of the model, in pixels. |

---

### `M_ALLOC_OFFSET_Y`

Inquires the Y-offset of the model in the model source image.  This is only supported for image-type or Edge Finder-type models.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset of the model, in pixels. |

---

### `M_ALLOC_SIGN`

Inquires whether the data range of the model image is signed or unsigned.

| Value | Description |
| --- | --- |
| `M_SIGNED` | Specifies that the data is signed. |
| `M_UNSIGNED` | Specifies that the data is unsigned. Synthetic, Model Finder-type and merge-type models will always return [`M_UNSIGNED`](../../Reference/mod/MmodInquire.md). |

---

### `M_ALLOC_SIZE_BAND`

Inquires the number of bands of the model image. When drawing the model image, use [`M_ALLOC_SIZE_BAND`](../../Reference/mod/MmodInquire.md) to establish the required number of bands for the destination buffer.

| Value | Description |
| --- | --- |
| `1` | Specifies a 1-band buffer. |
| `3` | Specifies a 3-band buffer. |

---

### `M_ALLOC_SIZE_BIT`

Inquires the depth per band, in bits, of the model image.

| Value | Description |
| --- | --- |
| `1` | Specifies that the data depth is 1 bit per band. |
| `8` | Specifies that the data depth is 8 bits per band. |
| `16` | Specifies that the data depth is 16 bits per band. |
| `32` | Specifies that the data depth is 32 bits per band. |

---

### `M_ALLOC_SIZE_X`

Inquires the width of the smallest buffer in which to draw the model box (the model and its margins).  For a model of an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this value also determines the mask size.  For a synthetic model of an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this value is affected by[`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) and model calibration.  For a synthetic model of an[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this value is determined by the smallest buffer that can contain the width of the model, and the margin that adds 10% of the width of the bounding box's active edges to the left and right edges of the model box. Note that the width of the model will be interpreted as if it had been specified in pixel units (even if they were not). If they were specified assuming a calibrated target, the returned size might not be very meaningful.

| Value | Description |
| --- | --- |
| `Value >= 16` | Specifies the width of the model (in X), in pixels. |

---

### `M_ALLOC_SIZE_Y`

Inquires the height of the smallest buffer in which to draw the model box (the model and its margins).  For a model of an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this value also determines the mask size.  For a synthetic model of an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this value is affected by[`M_PIXEL_SCALE`](../../Reference/mod/MmodInquire.md) and model calibration.  For a synthetic model of an[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this value is determined by the smallest buffer that can contain the height of the model, and the margin that adds 10% of the height of the bounding box's active edges to the top and bottom edges of the model box. Note that the height of the model will be interpreted as if it had been specified in pixel units (even if they were not). If they were specified assuming a calibrated target, the returned size might not be very meaningful.

| Value | Description |
| --- | --- |
| `Value >= 16` | Specifies the height of the model (in Y), in pixels. |

---

### `M_ALLOC_TYPE`

Inquires the data type and depth of the model image.

| Value | Description |
| --- | --- |
| `M_FLOAT + 32` | Specifies 32-bit floating-point data. |
| `M_SIGNED + 8` | Specifies 8-bit signed data. |
| `M_SIGNED + 16` | Specifies 16-bit signed data. |
| `M_SIGNED + 32` | Specifies 32-bit signed data. |
| `M_UNSIGNED + 1` | Specifies 1-bit unsigned data. |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |
| `M_UNSIGNED + 32` | Specifies 32-bit unsigned data. |

---

### `M_ANGLE`

Inquires the nominal search angle; this is the angle at which you expect to find the model's reference axis (specified using [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodInquire.md)) in the target.  For an ellipse or rectangle model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the model's reference axis is, by default, aligned with its principle axis (its width).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the nominal search angle, in degrees. |

---

### `M_ANGLE_DELTA_NEG`

Inquires the lower limit of the angular range, relative to the nominal search angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the lower limit of the angular range, in degrees. |

---

### `M_ANGLE_DELTA_POS`

Inquires the upper limit of the angular range, relative to the nominal search angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the upper limit of the angular range, in degrees. |

---

### `M_ANGLE_MULTIPLE_RANGE`

Inquires whether to search for models in multiple angular ranges.  This inquire type is only supported for a model in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that searching in multiple angular ranges is disabled. |
| `M_STEP_90` | Specifies to search for models at angular ranges at 90 degree steps from the nominal search angle. |
| `M_STEP_180` *(default)* | Specifies to search for models at a 180 degree step. |

---

### `M_ANGLE_REGION`

Sets the angle direction of the search region defined by the [`M_POSITION_...`](../../Reference/mod/MmodInquire.md) controls.  This control type is only valid for [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) models.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the search angle, in degrees. |

---

### `M_ASSOCIATED_CALIBRATION`

Inquires the camera calibration context associated with the specified model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Removes the association between the model and a camera calibration context. |
| `Calibration context identifier` | Specifies the camera calibration context that is associated with the model. |
| `M_NULL` | Specifies that there is no camera calibration context associated with the model. |

---

### `M_BOX_MARGIN_BOTTOM`

Inquires the margin at the bottom of the bounding box of the model's active edges, in model units.  This inquire type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the bottom margin is always 10.0% of the height of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the height of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_BOX_MARGIN_LEFT`

Inquires the margin at the left of the bounding box of the model's active edges, in model units.  This inquire type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the left margin is always 10.0% of the width of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the width of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_BOX_MARGIN_RIGHT`

Inquires the margin at the right of the bounding box of the model's active edges, in model units.  This inquire type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the right margin is always 10.0% of the width of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the width of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_BOX_MARGIN_TOP`

Inquires the margin at the top of the bounding box of the model's active edges, in model units.  This inquire type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the top margin is always 10.0% of the height of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the height of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_BOX_OFFSET_X`

Inquires the X-offset of the top-left corner of the model box from the model origin.  For predefined shape models, the origin of the model is the center of the shape. For CAD-file models, the origin is the same as the one defined in the CAD file.  [`M_BOX_OFFSET_X`](../../Reference/mod/MmodInquire.md) is updated when the margins that define the model box are changed.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset, in model units. |

---

### `M_BOX_OFFSET_Y`

Inquires the Y-offset of the top-left corner of the model box from the model origin.  For predefined shape models, the origin of the model is the center of the shape. For CAD-file models, the origin is the same as the one defined in the CAD file.  [`M_BOX_OFFSET_Y`](../../Reference/mod/MmodInquire.md) is updated when the margins that define the model box are changed.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset, in model units. |

---

### `M_BOX_SIZE_X`

Inquires the size along the X-axis of the model box.  You cannot set the size of the model box; however, you can set the size of the model's bounding box when defining the model using [`MmodDefine`](../../Reference/mod/MmodDefine.md). You can also set the size of the margins using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_BOX_MARGIN_...`](../../Reference/mod/MmodControl.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-size, in model units. |

---

### `M_BOX_SIZE_Y`

Inquires the size along the Y-axis of the model box.  You cannot set the size of the model box; however, you can set the size of the model's bounding box when defining the model using [`MmodDefine`](../../Reference/mod/MmodDefine.md). You can also set the size of the margins using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_BOX_MARGIN_...`](../../Reference/mod/MmodControl.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-size, in model units. |

---

### `M_CAD_Y_AXIS`

Inquires the direction of the Y-axis for a CAD-type model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_FLIP` | Specifies to flip the Y-axis for the model so that the Y-axis is positive going down. |
| `M_NO_FLIP` | Specifies not to flip the Y-axis for the model. |

---

### `M_CERTAINTY`

Inquires the certainty level for the score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the score, as a percentage. |

---

### `M_CERTAINTY_TARGET`

Inquires the certainty level for the target score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the target score, as a percentage. |

---

### `M_CHAIN_ANGLE`

Inquires the angle value of each edgel in the chains composing the model's active edges.

| Value | Description |
| --- | --- |
| `Value` | Specifies the angle value of the edgel. |

---

### `M_CHAIN_INDEX`

Inquires the indices which differentiate each active edge's chain of edgels, for each edgel within the model. The first chain is identified as index 1, with each subsequent chain's index incremented by 1.

| Value | Description |
| --- | --- |
| `Value` | Specifies the index of the chain. |

---

### `M_CHAIN_X`

Inquires the X-coordinates, in pixels (with subpixel accuracy), of each edgel in the chains composing the model's active edges. Coordinates are returned for all the active edge chains contained within the model and are relative to the model origin.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the edgel. |

---

### `M_CHAIN_Y`

Inquires the Y-coordinates, in pixels (with subpixel accuracy), of each edgel in the chains composing the model's active edges. Coordinates are returned for all the active edge chains contained within the model and are relative to the model origin.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the edgel. |

---

### `M_CORNER_RADIUS`

Inquires the radius used to round corners of the model.  This is only valid for models of type [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md), [`M_SQUARE`](../../Reference/mod/MmodDefine.md), [`M_DIAMOND`](../../Reference/mod/MmodDefine.md), [`M_TRIANGLE`](../../Reference/mod/MmodDefine.md), and[`M_CROSS`](../../Reference/mod/MmodDefine.md). Attempting to call this value for any other model type will generate an error.  This inquire type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the radius, in model units. |

---

### `M_COVERAGE_MAX`

Inquires the maximum expected model coverage.  This inquire type is only supported for a model defined in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum expected model coverage. |

---

### `M_COVERAGE_MIN`

Inquires the minimum expected model coverage.  This inquire type is only supported for a model defined in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the minimum expected model coverage. |

---

### `M_DEVIATION_TOLERANCE`

Inquires the tolerance for finding deformed rectangles or segments, for a model defined in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, respectively, given the other specified Model Finder constraints.  This inquire type is only supported for [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md) or [`M_SEGMENT`](../../Reference/mod/MmodDefine.md) types of models in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tolerance as a percentage of the allowable deformation of a rectangle or segment, given the other Model Finder constraints. |

---

### `M_ELLIPSE_TOLERANCE_TO_CIRCLE`

Inquires the tolerance for approximating the aspect ratio of an ellipse occurrence to 1 (that is, reporting that the ellipse model matched a circle).  [`M_ELLIPSE_TOLERANCE_TO_CIRCLE`](../../Reference/mod/MmodInquire.md) is only supported for a model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 0.5` *(default)* | Specifies the tolerance applied to the ellipse occurrence's aspect ratio. |

---

### `M_FIT_ERROR_WEIGHTING_FACTOR`

Inquires the contribution of the fit error in the score and target score calculation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the fit error weighting factor, as a percentage. |

---

### `M_FIT_SCORE_MIN`

Inquires the minimum expected occurrence fit score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum expected occurrence fit score. |

---

### `M_FOREGROUND_VALUE`

Inquires the foreground value of the model. The foreground is the interior of the shape.  This is only valid for predefined shape models. CAD-type models do not have a polarity. For these model types, [`M_ANY`](../../Reference/mod/MmodDefine.md) will be returned.

| Value | Description |
| --- | --- |
| *(see [`M_CIRCLE`](Reference/mod/MmodDefine.md))* |  |

---

### `M_HAS_DONT_CARE_MASK`

Inquires whether the model has an [`M_DONT_CARE`](../../Reference/mod/MmodMask.md) mask applied to it.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that an [`M_DONT_CARE`](../../Reference/mod/MmodMask.md) mask has not been applied to the model. |
| `M_TRUE` | Specifies that an [`M_DONT_CARE`](../../Reference/mod/MmodMask.md) mask has been applied to the model. |

---

### `M_HAS_FLAT_REGIONS_MASK`

Inquires whether the model has an [`M_FLAT_REGIONS`](../../Reference/mod/MmodMask.md) mask applied to it.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that an [`M_FLAT_REGIONS`](../../Reference/mod/MmodMask.md) mask has not been applied to the model. |
| `M_TRUE` | Specifies that an [`M_FLAT_REGIONS`](../../Reference/mod/MmodMask.md) mask has been applied to the model. |

---

### `M_HAS_WEIGHT_REGIONS_MASK`

Inquires whether the model has an [`M_WEIGHT_REGIONS`](../../Reference/mod/MmodMask.md) mask applied to it.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that an [`M_WEIGHT_REGIONS`](../../Reference/mod/MmodMask.md) mask has not been applied to the model. |
| `M_TRUE` | Specifies that an [`M_WEIGHT_REGIONS`](../../Reference/mod/MmodMask.md) mask has been applied to the model. |

---

### `M_HEIGHT`

Inquires the height of the shape in the model.  This is only valid for a model of type [`M_CROSS`](../../Reference/mod/MmodDefine.md), [`M_DIAMOND`](../../Reference/mod/MmodDefine.md), [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md), [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md), and [`M_TRIANGLE`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the shape, in model units. |

---

### `M_HORIZONTAL_THICKNESS`

Inquires the horizontal thickness of the cross in the model.  This is only valid for a model of type [`M_CROSS`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the horizontal thickness of the cross, in model units. |

---

### `M_INNER_RADIUS`

Inquires the inner radius of the ring in the model.  This is only valid for a model of type [`M_RING`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the inner radius of the ring, in model units. |

---

### `M_LENGTH`

Inquires the length of the segment in the model.  This is only valid for a model of type [`M_SEGMENT`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the length of the segment, in model units. |

---

### `M_MIN_SEPARATION_ANGLE`

Inquires the minimum angular separation required for two occurrences to be considered distinct matches. This value is an absolute angle value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Disables the minimum angle separation criteria. |
| `0.0 < Value <= 180.0` *(default)* |  |

---

### `M_MIN_SEPARATION_ASPECT_RATIO`

Inquires the minimum separation required in aspect ratios, for two occurrences to be considered distinct matches. This value is specified as an aspect ratio factor.  This inquire type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum aspect ratio separation criteria. |
| `1.0 < Value <= 4.0` *(default)* | Specifies the criteria for minimum separation of aspect ratios. |

---

### `M_MIN_SEPARATION_SCALE`

Inquires the minimum separation required in scale for two occurrences to be considered distinct matches. This value is a scale factor.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum scale separation criteria. |
| `1.0 < Value <= 4.0` *(default)* | Specifies the criteria for minimum separation in scale. |

---

### `M_MIN_SEPARATION_X`

Inquires the minimum separation required along the X-axis for two occurrences to be considered distinct matches. This value is a percentage of the model's width at [`M_SCALE`](../../Reference/mod/MmodInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum separation in X criteria. |
| `Value` *(default)* | Specifies the minimum separation as a percentage of the model's width at [`M_SCALE`](../../Reference/mod/MmodInquire.md). |

---

### `M_MIN_SEPARATION_Y`

Inquires the minimum separation required along the Y-axis for two occurrences to be considered distinct matches. This value is a percentage of the model's height at [`M_SCALE`](../../Reference/mod/MmodInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum separation in Y criteria. |
| `Value` *(default)* | Specifies the minimum separation as a percentage of the model's height at [`M_SCALE`](../../Reference/mod/MmodInquire.md). |

---

### `M_MIN_SIDE_COVERAGE`

Inquires the minimum coverage on each of the sides of a rectangular model. Note that at least a portion of each side must be visible.  This inquire type is only supported for a model in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 100.0` *(default)* | Specifies the minimum side coverage. |

---

### `M_MODEL_ASPECT_RATIO`

Inquires the nominal aspect ratio factor for the model.  This inquire type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For other types of Model Finder contexts, see [`M_ASPECT_RATIO`](../../Reference/mod/MmodInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCLE_ASPECT_RATIO` | Specifies the minimum possible value for this control type. |
| `Value > 0.0` *(default)* | Specifies the value of the nominal aspect ratio for the model. |

---

### `M_MODEL_ASPECT_RATIO_MAX_FACTOR`

Inquires the factor used to determine the upper limit (maximum permitted aspect ratio) of the model's aspect ratio.  This inquire type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies that the maximum factor for the aspect ratio of a model is infinite. |
| `Value >= 1.0` *(default)* | Specifies the factor that determines the maximum aspect ratio for a model. |

---

### `M_MODEL_ASPECT_RATIO_MIN_FACTOR`

Inquires he factor used to determine the lower limit (minimum permitted aspect ratio) of the model's aspect ratio.  This inquire type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCLE_ASPECT_RATIO` | Specifies the minimum factor for the aspect ratio of a model. |
| `0.0 < Value <= 1.0` *(default)* | Specifies the factor that determines the minimum aspect ratio for a model. |

---

### `M_MODEL_IMAGE_ORIGIN_X`

Inquires the X-coordinate of the model origin, relative to the top-left corner of the smallest buffer that can contain the model box (the model and its margins). The size of this buffer can be inquired using [`M_ALLOC_SIZE_X`](../../Reference/mod/MmodInquire.md) and [`M_ALLOC_SIZE_Y`](../../Reference/mod/MmodInquire.md).  This inquire type can be useful if you want to draw a shape at the center of the model origin (the center of the predefined-shape model).  To inquire the X-coordinate of the reference axis origin relative to the model origin, use [`M_REFERENCE_X`](../../Reference/mod/MmodInquire.md).  > **Note:** Note that the model origin for predefined-shape models is the center of the shape.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the X-coordinate of the model origin, in pixels. |

---

### `M_MODEL_IMAGE_ORIGIN_Y`

Inquires the Y-coordinate of the model origin, relative to the top-left corner of the smallest buffer that can contain the model box (the model and its margins). The size of this buffer can be inquired using [`M_ALLOC_SIZE_X`](../../Reference/mod/MmodInquire.md) and [`M_ALLOC_SIZE_Y`](../../Reference/mod/MmodInquire.md).  This inquire type can be useful if you want to draw a shape at the center of the model origin (the center of the predefined-shape model).  To inquire the Y-coordinate of the reference axis origin relative to the model origin, use [`M_REFERENCE_X`](../../Reference/mod/MmodInquire.md).  > **Note:** Note that the model origin for predefined-shape models is the center of the shape.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the Y-coordinate of the model origin, in pixels. |

---

### `M_MODEL_TYPE`

Inquires the type of model.

| Value | Description |
| --- | --- |
| `M_DXF_FILE` | Specifies a model defined from the entities in the specified CAD DXF file. |

---

### `M_NUMBER_OF_CHAINED_EDGELS`

Inquires the total number of chained edgels in the active edges of the model.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of chained edgels. |

---

### `M_ORIGINAL_X`

Inquires the X-coordinate of the model's reference axis origin, relative to the top-left corner of the model source image. This is only valid for image-type and Edge Finder-type models.  To inquire the X-coordinate of the reference axis origin relative to the model origin, use [`M_REFERENCE_X`](../../Reference/mod/MmodInquire.md).  > **Note:** Note that the model origin for image-type and Edge Finder-type models is the center of the upper left pixel of the model image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the model's reference axis origin. |

---

### `M_ORIGINAL_Y`

Inquires the Y-coordinate of the model's reference axis origin, relative to the top-left corner of the model source image This is only valid for image-type and Edge Finder-type models.  To inquire the Y-coordinate of the reference axis origin relative to the model origin, use [`M_REFERENCE_Y`](../../Reference/mod/MmodInquire.md).  > **Note:** Note that the model origin for image-type and Edge Finder-type models is the center of the upper left pixel of the model image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the model's reference axis origin. |

---

### `M_OUTER_RADIUS`

Inquires the outer radius of the ring in the model.  This is only valid for a model of type [`M_RING`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the outer radius of the ring, in model units. |

---

### `M_PIXEL_SCALE`

Inquires the pixel scale of the model, if it is a synthetic model.  [`M_PIXEL_SCALE`](../../Reference/mod/MmodInquire.md) is used for the match if the target is not calibrated; if the target is calibrated, this value is not used for the match nor for the returned results. Regardless if calibrated, this value will affect[`M_ALLOC_SIZE_X`](../../Reference/mod/MmodInquire.md) and [`M_ALLOC_SIZE_Y`](../../Reference/mod/MmodInquire.md) and it will also be used for draw operations.  This is only valid for synthetic models.  This inquire type is not supported for models defined in an[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these types of contexts, model units are interpreted to be in the units of the target. If the target is calibrated, the units are interpreted to be in calibrated units. If the target is not, the units are interpreted to be in pixel units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value of the pixel scale. |
| `Value > 0.0` | Specifies the pixel scale. |

---

### `M_POLARITY`

Inquires the expected polarity of occurrences relative to the model.  This inquire type is not supported for a model defined in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_ANY` | Specifies that the occurrences can be a mixture of polarities. |
| `M_REVERSE` | Specifies that the polarity of occurrences is the reverse of that of the model. |
| `M_SAME` | Specifies that the polarity of occurrences is the same as that of the model. |
| `M_SAME_OR_REVERSE` | Specifies that the polarity of occurrences can be either the same or the reverse of that of the model. |

---

### `M_POSITION_DELTA_NEG_X`

Inquires the position range, in the negative X-direction relative to the nominal search position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies the position range as the entire image plane in the negative X-direction. |
| `Value >= 0.0` *(default)* | Specifies the position range's negative X-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_DELTA_NEG_Y`

Inquires the position range, in the negative Y-direction relative to the nominal search position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies the position range as the entire image plane in the negative Y-direction. |
| `Value >= 0.0` *(default)* | Specifies the position range's negative Y-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_DELTA_POS_X`

Inquires the position range, in the positive X-direction relative to the nominal search position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies the position range as the entire image plane in the positive X-direction. |
| `Value >= 0.0` | Specifies the position range's positive X-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_DELTA_POS_Y`

Inquires the position range, in the positive Y-direction relative to the nominal search position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies the position range as the entire image plane in the positive Y-direction. |
| `Value >= 0.0` | Specifies the position range's positive Y-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_X`

Inquires the nominal search position for the X-coordinate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies all X-coordinates. |
| `Value` | Specifies the nominal search position, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the position in the units of the target's coordinate system. |

---

### `M_POSITION_Y`

Inquires the nominal search position for the Y-coordinate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies all Y-coordinates. |
| `Value` | Specifies the nominal search position, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the position in the units of the target's coordinate system. |

---

### `M_RADIUS`

Inquires the radius of the circle in the model.  This is only valid for a model of type [`M_CIRCLE`](../../Reference/mod/MmodDefine.md).  This inquire type is not supported for a model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the radius of the circle, in model units. |

---

### `M_REFERENCE_ANGLE`

Inquires the reference axis angle of the model.  By default, for an ellipse model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the reference axis is aligned with the principal axis of the model (the width).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_REFERENCE_X`

Inquires the X-coordinate, in pixels, of the reference axis origin of the model, relative to the model origin.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default X-offset will be used. |
| `Value` | Specifies the X-offset value. |

---

### `M_REFERENCE_Y`

Inquires the Y-coordinate, in pixels, of the reference axis origin of the model, relative to the model origin.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default Y-offset will be used. |
| `Value` | Specifies the Y-offset value. |

---

### `M_SAGITTA_TOLERANCE`

Inquires the tolerance for finding deformed circles (allowable radii variation) for an[`M_CIRCLE`](../../Reference/mod/MmodDefine.md) or an [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md) type of model.  This inquire type is only supported for [`M_CIRCLE`](../../Reference/mod/MmodDefine.md) or [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md) types of models in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tolerance as a percentage of the allowable radii variation given the other Model Finder constraints. |

---

### `M_SCALE`

Inquires the nominal search scale.  For a model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, scale calculations are taken with respect to the model's width. For a model defined in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, scale calculations are taken with respect to the model's length.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the value of the nominal search scale. |

---

### `M_SCALE_MAX_FACTOR`

Inquires the factor used to determine the upper limit (maximum permitted scale) of the scale range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies that the upper limit of the scale range is infinite. |
| `1.0 <= Value <= 2.0` *(default)* | Specifies the factor that determines the maximum scale of the occurrence for a model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `Value >= 1.0` | Specifies the factor that determines the maximum scale of the occurrence for a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |

---

### `M_SCALE_MIN_FACTOR`

Inquires the factor used to determine the lower limit (minimum permitted scale) of the scale range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 1.0` *(default)* | Specifies the factor that determines the minimum scale of the occurrence for a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `0.5 <= Value <= 1.0` | Specifies the factor that determines the minimum scale of the occurrence for a model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |

---

### `M_SEARCH_ASPECT_RATIO_CONSTRAINT`

Inquires whether to constrain candidates to the nearest bound of the aspect ratio range if the candidate is outside of the range.  This inquire type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that for a candidate to be considered an occurrence, it must have an aspect ratio within the defined aspect ratio range. |
| `M_ENABLE` *(default)* | Specifies to consider candidates with an aspect ratio that falls outside of the specified aspect ratio range, but constrain their fit to the closest bound of the aspect ratio range, and reduce their score accordingly. |

---

### `M_SEGMENT_CONSISTENT_GRADIENT`

Inquires whether the gradients of the segment model occurrences need to be consistent.  This inquire type is only supported for a model in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the segment model occurrences can have the same gradients or reverse gradients. |
| `M_ENABLE` *(default)* | Specifies that the gradients of the segment model occurrences must be consistent. |

---

### `M_USER_LABEL`

Inquires the user-defined label.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NO_LABEL` *(default)* | Specifies that no user label is associated with the model. |
| `Value` | Specifies the user label of the model. |

---

### `M_VALID`

Inquires whether the model is valid or not.  Note that a synthetic model is invalid if `_size of the model box _ * [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) * [`M_SCALE`](../../Reference/mod/MmodControl.md)` is greater than 1024 pixels in the X-direction or the Y-direction.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the model is not valid. |
| `M_TRUE` | Specifies that the model is valid. |

---

### `M_VERTICAL_THICKNESS`

Inquires the vertical thickness of the cross in the model.  This is only valid for a model of type [`M_CROSS`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the vertical thickness of the cross, in model units. |

---

### `M_WIDTH`

Inquires the width of the shape in the model.  This is only valid for a model of type [`M_CROSS`](../../Reference/mod/MmodDefine.md), [`M_DIAMOND`](../../Reference/mod/MmodDefine.md), [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md), [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md), [`M_SQUARE`](../../Reference/mod/MmodDefine.md), and [`M_TRIANGLE`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the shape, in model units. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### For inquiring about a model using its user label

For model inquiries using the model's user label l(that is, when the [`Index`](../../Reference/mod/MmodInquire.md) parameter is set to the user label of a model), the [`InquireType`](../../Reference/mod/MmodInquire.md) parameter can be set to the following:

---

### `M_INDEX_FROM_LABEL`

Inquires the model index associated with a model user label if the user label is used. Pass the user label as the [`Index`](../../Reference/mod/MmodInquire.md) parameter.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the user label is invalid. This is returned if the specified model label is not associated with a model. |
| `Value` | Specifies the model index that is associated with the specified user label. |

### For a result buffer

For result buffer inquiries, the [`InquireType`](../../Reference/mod/MmodInquire.md) parameter can be set to one of the following:

---

### `M_HAS_TARGET_EDGES_SAVED`

Inquires whether target edges have been saved.  [`M_SAVE_TARGET_EDGES`](../../Reference/mod/MmodControl.md) must be set to [`M_ENABLE`](../../Reference/mod/MmodControl.md) to save target edges.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that target edges have not been saved. |
| `M_TRUE` | Specifies that target edges have been saved. |

---

### `M_MOD_DEFINE_COMPATIBLE`

Inquires whether the information necessary to define a model from an occurrence will be saved in the result buffer.  This inquire type is not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to save any information. |
| `M_ENABLE` | Specifies to save all the necessary information. |

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

---

### `M_RESULT_TYPE`

Inquires the type of result buffer allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of context. |
| `M_SHAPE_CIRCLE` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md)type of context. |
| `M_SHAPE_ELLIPSE` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md)type of context. |
| `M_SHAPE_RECTANGLE` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md)type of context. |
| `M_SHAPE_SEGMENT` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md)type of context. |
| `M_GEOMETRIC` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) type of context. |
| `M_GEOMETRIC_CONTROLLED` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of context. |

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

This inquire type is not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

This inquire type is only valid for synthetic models.

The model box is defined by the bounding box of the model's active edges plus the specified margins. You can inquire the margins using [`M_BOX_MARGIN_...`](../../Reference/mod/MmodInquire.md).

This inquire type is not supported for a model defined in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

This inquire type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

This inquire type is only supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.
