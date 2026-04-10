---
doctype: Reference
module: mod
function: MmodControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / mod / MmodControl"
---

# MmodControl

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

> Control a Model Finder context, individual model setting, or Model Finder result buffer setting.

## Syntax

```c
void MmodControl(
    AIL_ID     ContextOrResultId,  //out
    AIL_INT    Index,              //in
    AIL_INT64  ControlType,        //in
    AIL_DOUBLE ControlValue        //in
)
```

## Description

This function allows you to control a setting of a Model Finder context, one (or all) of the models contained therein, or a Model Finder result buffer. These settings control the execution of [`MmodFind`](../../Reference/mod/MmodFind.md) operations. Most of the control type settings can be inquired using [`MmodInquire`](../../Reference/mod/MmodInquire.md).

The units in which you should provide values (for example, position and width), depend on the model and context that you are using. In general, provide values as follows:

- For a non-synthetic model, provide values in the pixel coordinate system, even if the target is calibrated. The only exception is when specifying angles (for example, [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md)); in these cases, you must specify the angles in the relative coordinate system if the target is calibrated.
- For a synthetic model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context, provide values in the user-defined units for the model; their relationship to pixels is defined using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md). If the target is not calibrated, [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) is used for the match. If the target is calibrated, model units are used; note that these should be the same as the calibrated units. In a few cases, instead of providing values in model units, you must provide values in pixel units (for example, [`M_POSITION_...`](../../Reference/mod/MmodControl.md)); these cases are clearly indicated.
- For a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) context, provide values in model units, which in this case are the same as the target units. If the target is calibrated, the model's settings will be interpreted in world units. If the target is not calibrated, the model's settings will be interpreted in pixel units.

For each model defined in your Model Finder context, you can restrict the region in which to find the reference position of the model (the search region) to a rectangular subsection of your target image; this is useful if you know that occurrences can only be found in one region of the target image. There are three methods to do so:

- You can manually define the search region using successive calls to [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md) set to [`M_ENABLE`](../../Reference/mod/MmodControl.md), [`M_POSITION_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_X`](../../Reference/mod/MmodControl.md), and [`M_POSITION_DELTA_POS_Y`](../../Reference/mod/MmodControl.md). For models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type Model Finder contexts, the [`M_ANGLE_REGION`](../../Reference/mod/MmodControl.md) control type is also available, which allows you to set your rectangular search region at a non-zero angle.
- You can call [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md) set to [`M_ENABLE`](../../Reference/mod/MmodControl.md), [`M_SEARCH_POSITION_FROM_GRAPHIC_LIST`](../../Reference/mod/MmodControl.md), and the identifier of a 2D graphics list. The 2D graphics list must contain one graphic of type rectangle, and have its input units set to pixels; Aurora Imaging Library will use its position and dimensions to define the search region. Note that for models in [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type Model Finder contexts, the rectangular graphic must be set with no angle, but for models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type contexts, it can be set at any angle.
- For models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type Model Finder contexts, you can call [`MmodFind`](../../Reference/mod/MmodFind.md) directly with a target image buffer that is associated with a rectangular region of interest at any angle.

Changing certain control type settings might require preprocessing of the Model Finder context again, using [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md). To know if the Model Finder context needs to be preprocessed, call [`MmodInquire`](../../Reference/mod/MmodInquire.md) with the [`M_PREPROCESSED`](../../Reference/mod/MmodInquire.md) inquire type.

You can automatically define an image-type model that is based on specified control settings using this function with [`M_AUTO_DEFINE`](../../Reference/mod/MmodControl.md). To have specified settings taken into account during model definition, you must allocate an empty image-type model using [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md), set the appropriate control types using [`MmodControl`](../../Reference/mod/MmodControl.md), and then call [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md). [`M_AUTO_DEFINE`](../../Reference/mod/MmodControl.md) defines a unique model by searching the model source image for unique geometric features that match the settings specified using [`MmodControl`](../../Reference/mod/MmodControl.md). When the most suitable geometric features are found, the function automatically defines a model from those features. If none are found, no model is allocated and an error is reported. It can take several seconds to find the best model (or more for large or small images). To be useful, the model source image should be a typical target image; otherwise, the selected area for the model might never be present in the target image. For more information on defining an image-type model automatically, see [Image-type models](../../UserGuide/C08_Geometric_Model_Finder/S05_Defining_and_adding_models_to_your_model_finder_context.md).

## Parameters

### `ContextOrResultId` *(out, AIL_ID)*

Specifies the Model Finder context or Model Finder result buffer whose settings you want to modify. The Model Finder context or result buffer must have been previously allocated on the system using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) or [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md), respectively.

### `Index` *(in, AIL_INT)*

Specifies that the Model Finder context, an individual model, or a Model Finder result buffer is controlled. Set this parameter to one of the following values:

*For specifying a context, model, or result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default.

If a Model Finder context is specified, same as [`M_ALL`](../../Reference/mod/MmodControl.md).

If a Model Finder result buffer is specified, same as [`M_GENERAL`](../../Reference/mod/MmodControl.md). |
| `M_ALL` | Applies the specified control setting to all models, if a Model Finder context is specified.

All the models in the Model Finder context must support the specified control type, otherwise, an error is returned. |
| `M_CONTEXT` | Controls a general setting of a specified Model Finder context, if one is specified. |
| `M_GENERAL` | Controls a general setting of a Model Finder result buffer when [`ContextOrResultId`](../../Reference/mod/MmodControl.md) is a result buffer. |
| `0 <= Value < M_NUMBER_MODELS` | Specifies the index of the individual model to control, if a Model Finder context is specified.

User labels cannot be used as indices; to retrieve the index of a model from its user label, use [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_INDEX_FROM_LABEL`](../../Reference/mod/MmodInquire.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For the context and models

The following [`ControlType`](../../Reference/mod/MmodControl.md) and corresponding [`ControlValue`](../../Reference/mod/MmodControl.md) settings can be specified for both the Model Finder context ([`M_CONTEXT`](../../Reference/mod/MmodControl.md)) and each individual model (or all models) in the Model Finder context:

---

### `M_NUMBER`

Sets the number of occurrences for which to search.  For [`M_CONTEXT`](../../Reference/mod/MmodControl.md), this control type sets the maximum number of all model occurrences for which to search in the target.  For a model, this control type sets the maximum number of occurrences of the specified model, for which to search in the target.  Once the number of occurrences for the context, with scores and target scores greater than or equal to their certainty levels, has been reached, the search will stop, regardless of whether the actual number set for any individual model has been found.  To further illustrate the relationship between the [`M_NUMBER`](../../Reference/mod/MmodControl.md) setting for the context and that of the individual models, see [Expected number of occurrences](../../UserGuide/C08_Geometric_Model_Finder/S11_Customizing_search_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value.  For [`M_CONTEXT`](../../Reference/mod/MmodControl.md), the default is [`M_ALL`](../../Reference/mod/MmodControl.md).  For a model, the default value is 1. |
| `M_ALL` | Specifies to find all occurrences.  For [`M_CONTEXT`](../../Reference/mod/MmodControl.md), [`M_ALL`](../../Reference/mod/MmodControl.md) finds all the occurrences specified for each model in the context.  For a model, [`M_ALL`](../../Reference/mod/MmodControl.md) finds all the occurrences of the specified model.  Note that this setting can increase the search time; always set [`M_NUMBER`](../../Reference/mod/MmodControl.md) to a specific number whenever possible. |
| `Value > 0` | Specifies the number of occurrences.  For [`M_CONTEXT`](../../Reference/mod/MmodControl.md), this value specifies the number of occurrences, for all models within the context, to find in the target.  For a model, this value specifies the maximum number of occurrences of the specified model to find in the target. |

### For the context

The following [`ControlType`](../../Reference/mod/MmodControl.md) and corresponding [`ControlValue`](../../Reference/mod/MmodControl.md) settings can be specified for the Model Finder context ([`M_CONTEXT`](../../Reference/mod/MmodControl.md)):

---

### `M_ACCURACY`

Sets the accuracy with which to find occurrences. The precision achieved is dependent on the quality of the model and the target. In addition, positional accuracy is slightly affected by the setting of the [`M_SPEED`](../../Reference/mod/MmodControl.md).  Note that lower accuracy can speed up the [`MmodFind`](../../Reference/mod/MmodFind.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies high accuracy. |
| `M_LOW` | Specifies low accuracy. |
| `M_MEDIUM` *(default)* | Specifies medium accuracy. |

---

### `M_ACTIVE_EDGELS`

Sets the degree to which Aurora Imaging Library processes edgels in the target ([`MmodFind`](../../Reference/mod/MmodFind.md)) The target edgels that Aurora Imaging Library processes are considered active. By limiting the active edgels in the target, you can speed up the search. To draw the active edgels in the target, use [`MmodDraw`](../../Reference/mod/MmodDraw.md) with [`M_DRAW_ACTIVE_EDGELS`](../../Reference/mod/MmodDraw.md).  This control type is only supported for a geometric Model Finder context ([`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the degree to which Aurora Imaging Library processes target edgels, as a percentage. Lower settings typically result in fewer active edgels in the target and faster searches, although accuracy can be compromised. Conversely, higher settings result in more active edgels in the target and higher accuracy, although searches can take longer. |

---

### `M_ASPECT_RATIO`

Sets the aspect ratio to apply to the target before starting the search. When the target is not calibrated, this control type allows you to quickly compensate for aspect ratio distortion in the target, typically a side effect of the sampling rate used by the frame grabber. When the target is calibrated, this control type is ignored.  This control type is mainly useful when dealing with synthetic models since it does not compensate for aspect ratio distortion in the model.  When performing the search, the nominal search aspect ratio is taken into account horizontally, adjusting the target pixel width to equal the pixel height. As such, results will be returned for this corrected target coordinate system. However, the results are converted to the original target coordinate system if you draw them using [`MmodDraw`](../../Reference/mod/MmodDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.5 <= Value <= 2.0` *(default)* | Specifies the pixel width/pixel height of the target. |

---

### `M_DETAIL_LEVEL`

Sets the level of details to extract from model images and target images during edge extraction. The detail level determines what is considered an edge/background.  A higher detail level will include more edges than a lower detail level.  This setting is used when dealing with image-type models and/or image-type targets.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies a high level of detail. |
| `M_MEDIUM` *(default)* | Specifies a medium level of detail. |
| `M_VERY_HIGH` | Specifies a very high level of detail. |

---

### `M_EDGE_ACCURACY`

Controls the edge accuracy to be used when calculating the edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` *(default)* | Specifies a high level of edge accuracy. |
| `M_VERY_HIGH` | Specifies a very high level of edge accuracy. |

---

### `M_FIRST_LEVEL`

Sets the resolution level for the initial stage (lowest resolution) of the search.  Level 0 is the original target and each higher level is half the size (and resolution) of the previous one. If the specified level is not supported by the search algorithm, the highest possible level will be used.  A higher first level speeds up the initial search but might make it less reliable because the model might not retain enough distinctive features at a lower resolution.  [`M_FIRST_LEVEL`](../../Reference/mod/MmodControl.md) is for advanced users of the Model Finder module. The default setting usually provides the best results for the search operation.  > **Note:** Note that if you set a lower value for [`M_FIRST_LEVEL`](../../Reference/mod/MmodControl.md) than the one assigned to [`M_LAST_LEVEL`](../../Reference/mod/MmodControl.md), [`MmodControl`](../../Reference/mod/MmodControl.md) will override your settings and automatically assign [`M_FIRST_LEVEL`](../../Reference/mod/MmodControl.md) the same value as [`M_LAST_LEVEL`](../../Reference/mod/MmodControl.md). This implies that only one resolution level will be used for the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the first resolution is determined automatically. |
| `0 <= Value <= 7` | Specifies the resolution level. Only integer values are accepted. |

---

### `M_LAST_LEVEL`

Sets the resolution level for the final stage (the highest resolution) of the search.  Level 0 is the original target and each higher level is half the size (and resolution) of the previous one. If the specified level is not supported by the search algorithm, the highest possible level will be used.  A higher last level speeds up the initial search but might make it less reliable and less accurate because the model might not retain enough distinctive features at a low resolution.  [`M_LAST_LEVEL`](../../Reference/mod/MmodControl.md) is for advanced users of the Model Finder module. The default setting usually provides the best results for the search operation.  > **Note:** Note that if you set a higher value for [`M_LAST_LEVEL`](../../Reference/mod/MmodControl.md) than the one assigned to [`M_FIRST_LEVEL`](../../Reference/mod/MmodControl.md), [`MmodControl`](../../Reference/mod/MmodControl.md) will override your settings and automatically assign [`M_FIRST_LEVEL`](../../Reference/mod/MmodControl.md) the same value as [`M_LAST_LEVEL`](../../Reference/mod/MmodControl.md). This implies that only one resolution level will be used for the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the last resolution is determined automatically. |
| `0 <= Value <= 7` | Specifies the resolution level. Only integer values are accepted. |

---

### `M_RESOLUTION_COARSENESS_LEVEL`

Sets the resolution of edge approximation.  This control type is only valid for [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) Model finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the resolution. When set to 0.0, a very fine edge approximation is performed. When set to 100.0, a coarse edge approximation is performed. |

---

### `M_SAVE_TARGET_EDGES`

Sets whether to save the target edges in the result buffer. By enabling this control type, you can retrieve target chain information using [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) and you can draw target edges using [`MmodDraw`](../../Reference/mod/MmodDraw.md).  For an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of context, this control type must be enabled if you want to use the search results as a target for a subsequent search of a model defined in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that target edges cannot be saved. |
| `M_ENABLE` | Specifies that target edges can be saved. |

---

### `M_SEARCH_ANGLE_RANGE`

Sets whether to perform calculations specific to angular-range search strategies.  Typically, to search for models within their specified angular range ([`M_ANGLE`](../../Reference/mod/MmodControl.md)-[`M_ANGLE_DELTA_NEG`](../../Reference/mod/MmodControl.md) to [`M_ANGLE`](../../Reference/mod/MmodControl.md)+[`M_ANGLE_DELTA_POS`](../../Reference/mod/MmodControl.md)) in the target, calculations specific to angular-range search strategies should be enabled for the context. These calculations are not required to search for models at their nominal angle ([`M_ANGLE`](../../Reference/mod/MmodControl.md)). In addition, if you expect that the occurrences sought are close to their model's nominal angle, you can try disabling these calculations to see if Model Finder can still find the required occurrences. Disabling these calculations might speed up the search depending on the model and the target.  Note that [`M_SEARCH_ANGLE_RANGE`](../../Reference/mod/MmodControl.md) must be enabled to search for rotation-invariant non-synthetic models (for example, an image-type model of a circle).  Also note that candidates can only be returned as occurrences if found within the angular range specified for their model. Therefore, whether you enable or disable the calculation, you can restrict which candidates are returned as occurrences by narrowing the angular range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value.  The default is the same as [`M_ENABLE`](../../Reference/mod/MmodControl.md) if the Model Finder context uses a general geometric search algorithm.  The default is the same as [`M_DISABLE`](../../Reference/mod/MmodControl.md) if the Model Finder context uses a controlled geometric search algorithm. |
| `M_DISABLE` | Specifies that calculations specific to angular-range search strategies are disabled. |
| `M_ENABLE` | Specifies that calculations specific to angular-range search strategies are enabled. |

---

### `M_SEARCH_POSITION_RANGE`

Sets whether to perform calculations specific to position-range search strategies.  Typically, to search for models within their specified position range ([`M_POSITION_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_Y`](../../Reference/mod/MmodControl.md)) in the target, calculations specific to position-range search strategies should be enabled for the context. These calculations are not required to search for models at their nominal position ([`M_POSITION_X`](../../Reference/mod/MmodControl.md) and [`M_POSITION_Y`](../../Reference/mod/MmodControl.md)). In addition, if you expect that the occurrences sought are close to their model's nominal position, you can try disabling these calculations to see if Model Finder can still find the required occurrences. Disabling these calculations might speed up the search depending on the model and the target.  Note that candidates can only be returned as occurrences if found within the position range specified for their model. Therefore, whether you enable or disable the calculations, you can restrict which candidates are returned as occurrences by narrowing the position range.  Finally, if these calculations are disabled and no nominal position is specified ([`M_POSITION_X`](../../Reference/mod/MmodControl.md) and [`M_POSITION_Y`](../../Reference/mod/MmodControl.md) set to [`M_ALL`](../../Reference/mod/MmodControl.md)), no occurrences will be found and no errors will be returned.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that calculations specific to position-range search strategies are disabled. |
| `M_ENABLE` *(default)* | Specifies that calculations specific to position-range search strategies are enabled. |

---

### `M_SEARCH_SCALE_RANGE`

Sets whether to perform calculations specific to scale-range search strategies.  Typically, to search for models within their specified scale range ([`M_SCALE`](../../Reference/mod/MmodControl.md)x [`M_SCALE_MIN_FACTOR`](../../Reference/mod/MmodControl.md)) to ([`M_SCALE`](../../Reference/mod/MmodControl.md)x [`M_SCALE_MAX_FACTOR`](../../Reference/mod/MmodControl.md)) in the target, calculations specific to scale-range search strategies should be enabled for the context. These calculations are not required to search for models at their nominal scale ([`M_SCALE`](../../Reference/mod/MmodControl.md)). In addition, if you expect that the occurrences sought are close to their model's nominal scale, you can try disabling these calculations to see if Model Finder can still find the required occurrences. Disabling these calculations might speed up the search depending on the model and the target.  Note that candidates can only be returned as occurrences if found within the scale range specified for their model. Therefore, whether you enable or disable the calculation, you can restrict which candidates are returned as occurrences by narrowing the scale range.  This control type cannot be set to [`M_ENABLE`](../../Reference/mod/MmodControl.md) if the Model Finder context is set to [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that calculations specific to scale-range search strategies are disabled. |
| `M_ENABLE` | Specifies that calculations specific to scale-range search strategies are enabled.  Note that when these calculations are enabled, limiting the scale range to the minimum required generally decreases the search time. |

---

### `M_SHARED_EDGES`

Sets whether to enable sharing of edges between occurrences.  This control type is not supported for a model defined in an[`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md), an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md), or an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md)type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the sharing of edges. When disabled, edges that are part of more than one occurrence are considered as part of the occurrence with the greatest score only. |
| `M_ENABLE` | Specifies to enable the sharing of edges. |

---

### `M_SMOOTHNESS`

Sets the degree of smoothness (noise reduction) applied to the model images and target images during the edge extraction.  This setting is only used when dealing with image-type models or using image-type targets.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value applied to the images.  A setting of 0.0 indicates almost no noise reduction effect, while a setting of 100.0 indicates a very strong noise reduction effect. |

---

### `M_SPEED`

Sets the algorithm's search speed. Note that increasing the search speed can decrease the robustness and subpixel accuracy of the [`MmodFind`](../../Reference/mod/MmodFind.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies a high speed. |
| `M_LOW` | Specifies a low speed. |
| `M_MEDIUM` *(default)* | Specifies a medium speed. |
| `M_VERY_HIGH` | Specifies a very high speed. |

---

### `M_STOP_FIND`

Stops the current [`MmodFind`](../../Reference/mod/MmodFind.md) operation. You must call [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_STOP_FIND`](../../Reference/mod/MmodControl.md) from another thread (typically of higher priority). The number of occurrences found at that point will be returned.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_TARGET_CACHING`

Sets whether target caching is enabled. When target caching is enabled, the geometric representation of the target, generated by [`MmodFind`](../../Reference/mod/MmodFind.md), is kept in the result.  Subsequent calls to [`MmodFind`](../../Reference/mod/MmodFind.md) can reuse the representation, if they use the same result buffer and target. This increases the speed of the searches.  Note that the geometric representation generated with a given Model Finder context might not be compatible with another context. In such a case, the cache will not be used.  Also note that this constant cannot be set to [`M_ENABLE`](../../Reference/mod/MmodControl.md) if the Model Finder context is set to [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable target caching. Each call to [`MmodFind`](../../Reference/mod/MmodFind.md) will generate the geometric representation of the target. |
| `M_ENABLE` | Specifies to enable target caching in the result. |

---

### `M_TIMEOUT`

Sets the maximum search time for [`MmodFind`](../../Reference/mod/MmodFind.md), in msec. If a search times out, the number of occurrences found at that point will be returned.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies an infinite amount of search time. |
| `Value >= 0.0` *(default)* | Specifies the maximum search time, in msecs. |

### For one or all models

The following [`ControlType`](../../Reference/mod/MmodControl.md) and corresponding [`ControlValue`](../../Reference/mod/MmodControl.md) settings can be specified for each individual model or all models in the Model Finder context:

---

### `M_ACCEPTANCE`

Sets the acceptance level for the score. An occurrence will be returned only if the match score between the target and the model is greater than or equal to this level.  The score is a measure, as a percentage, of the presence and fit of the model's active edges in the occurrence. Using [`M_FIT_ERROR_WEIGHTING_FACTOR`](../../Reference/mod/MmodControl.md), you can specify how much the fit influences the score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. 100% indicates that for every active edge in the model, a corresponding edge must be found in the occurrence with a perfect fit (generally hard to obtain). |

---

### `M_ACCEPTANCE_TARGET`

Sets the acceptance level for the target score. An occurrence will be returned only if the target score between the target and the model is greater than or equal to this level.  The target score is a measure, as a percentage, of edges found in the occurrence that are not found in the original model, weighted by the deviation in position of the common edges. Using [`M_FIT_ERROR_WEIGHTING_FACTOR`](../../Reference/mod/MmodControl.md), you can specify how much the fit influences the target score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable target score, as a percentage. At 0.0%, any number of extra edges is tolerated (100% allows for no extra edges in the occurrence and the occurrence must have a perfect fit). |

---

### `M_ANGLE`

Sets the nominal search angle; this is the angle at which you expect to find the model's reference axis (specified using [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md)) in the target.  For a model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the model's reference axis is, by default, aligned with its principle axis (its width).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the nominal search angle, in degrees. |

---

### `M_ANGLE_DELTA_NEG`

Sets the lower limit of the angular range, relative to the nominal search angle ([`M_ANGLE`](../../Reference/mod/MmodControl.md)). Occurrences with angles outside the angular-range cannot be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the lower limit of the angular range, in degrees. |

---

### `M_ANGLE_DELTA_POS`

Sets the upper limit of the angular range, relative to the nominal search angle ([`M_ANGLE`](../../Reference/mod/MmodControl.md)). Occurrences with angles outside the angular-range cannot be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the upper limit of the angular range, in degrees. |

---

### `M_ANGLE_MULTIPLE_RANGE`

Sets whether to search for occurrences in multiple angular ranges. This allows you to search for occurrences at their nominal angle and at an angle perpendicular to the nominal search angle. For a model in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of context, this also allows you to search for occurrences with opposite segment gradient directions (for example, along an edge of a chessboard grid row).  If you only want to search for occurrences at the nominal angle, [`M_ANGLE`](../../Reference/mod/MmodControl.md), these calculations are not required.  This control type is only supported for a model in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that searching in multiple angular ranges is disabled.  This value is only supported for a model in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `M_STEP_90` | Specifies to search for models at angular ranges at 90 degree steps from the nominal search angle.  It specifies to search for models within the specified angular range ([`M_ANGLE`](../../Reference/mod/MmodControl.md)-[`M_ANGLE_DELTA_NEG`](../../Reference/mod/MmodControl.md) to [`M_ANGLE`](../../Reference/mod/MmodControl.md)+[`M_ANGLE_DELTA_POS`](../../Reference/mod/MmodControl.md)), and also within the angular range at all 90 degree rotations of the specified angular range (for example, [`M_ANGLE`](../../Reference/mod/MmodControl.md)+ 90 - [`M_ANGLE_DELTA_NEG`](../../Reference/mod/MmodControl.md) to [`M_ANGLE`](../../Reference/mod/MmodControl.md)+ 90 + [`M_ANGLE_DELTA_POS`](../../Reference/mod/MmodControl.md)) in the target. |
| `M_STEP_180` *(default)* | Specifies to search for models at a 180 degree step.  It specifies to search for models within the specified angular range ([`M_ANGLE`](../../Reference/mod/MmodControl.md)-[`M_ANGLE_DELTA_NEG`](../../Reference/mod/MmodControl.md) to [`M_ANGLE`](../../Reference/mod/MmodControl.md)+[`M_ANGLE_DELTA_POS`](../../Reference/mod/MmodControl.md)), and also within the angular range at a 180 degree rotation of the specified angular range ( [`M_ANGLE`](../../Reference/mod/MmodControl.md)+ 180 - [`M_ANGLE_DELTA_NEG`](../../Reference/mod/MmodControl.md) to [`M_ANGLE`](../../Reference/mod/MmodControl.md)+ 180 + [`M_ANGLE_DELTA_POS`](../../Reference/mod/MmodControl.md)) in the target.  Note that a rectangle occurrence found at an angle of [`M_ANGLE`](../../Reference/mod/MmodControl.md), and a rectangle occurrence found at an angle of [`M_ANGLE`](../../Reference/mod/MmodControl.md) + 180 degrees, are considered the same rectangle. |

---

### `M_ANGLE_REGION`

Sets the angle of the search region defined by the [`M_POSITION_...`](../../Reference/mod/MmodControl.md) control types.  Setting the angle of the search region does not set or affect the nominal search angle in any way; it only limits the location in which the reference position of the model can be found. If you expect to find occurrences at an angle, you must also set [`M_ANGLE`](../../Reference/mod/MmodControl.md). Likewise, searching within a region at an angle has no effect on the angle of a found occurrence's reference axis returned by [`MmodGetResult`](../../Reference/mod/MmodGetResult.md).  This control type is only valid for models in[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) contexts.  For models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) contexts, you can alternatively set the search region at an angle by specifying[`M_SEARCH_POSITION_FROM_GRAPHIC_LIST`](../../Reference/mod/MmodControl.md) and passing the identifier of a 2D graphics list containing a rotated rectangular graphic, or by passing a target image buffer associated with a rotated rectangular region of interest to [`MmodFind`](../../Reference/mod/MmodFind.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle of the search region, in degrees. |

---

### `M_ASSOCIATED_CALIBRATION`

Associates the specified camera calibration context with the specified model. Note that if you do not save, with the Model Finder context, the camera calibration contexts associated with the models ([`MmodSave`](../../Reference/mod/MmodSave.md) or [`MmodStream`](../../Reference/mod/MmodStream.md) with [`M_WITH_CALIBRATION`](../../Reference/mod/MmodSave.md)), you must re-associate each model with its camera calibration context after restoring the context ([`MmodRestore`](../../Reference/mod/MmodRestore.md), or [`MmodStream`](../../Reference/mod/MmodStream.md)).  In addition, for a synthetic model, you should use [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md) to associate the camera calibration context of a calibrated target with the model. Note that the camera calibration contexts associated with synthetic models are not saved or restored by [`MmodSave`](../../Reference/mod/MmodSave.md), [`MmodRestore`](../../Reference/mod/MmodRestore.md) or [`MmodStream`](../../Reference/mod/MmodStream.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Removes the association between the model and a camera calibration context. |
| `Calibration context identifier` | Specifies the camera calibration context to associate with the model. |

---

### `M_AUTO_DEFINE`

Defines a unique model from a source image, automatically. This control type is only supported for models of type [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md).

| Value | Description |
| --- | --- |
| `Source image identifier` | Specifies the identifier of the image buffer (model source image) from which to extract the model. The image buffer must be a 1-band 8-bit unsigned buffer. |

---

### `M_BOX_MARGIN_BOTTOM`

Sets a margin at the bottom of the bounding box of the model's active edges.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  The model box is used to compute the target score and to set the different masks of the model. If you change this value, all of this model's previous masks are discarded, since their size is no longer valid.  [`M_BOX_MARGIN_BOTTOM`](../../Reference/mod/MmodControl.md) is only available for synthetic models.  This control type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the bottom margin is always 10.0% of the height of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the height of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_BOX_MARGIN_LEFT`

Sets a margin at the left side of the bounding box of the model's active edges.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  The model box is used to compute the target score and to set the different masks of the model. If you change this value, all of this model's previous masks are discarded, since their size is no longer valid.  [`M_BOX_MARGIN_LEFT`](../../Reference/mod/MmodControl.md) is only available for synthetic models.  This control type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the left margin is always 10.0% of the width of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the width of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_BOX_MARGIN_RIGHT`

Sets a margin at the right side of the bounding box of the model's active edges.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  The model box is used to compute the target score and to set the different masks of the model. If you change this value, all of this model's previous masks are discarded, since their size is no longer valid.  [`M_BOX_MARGIN_RIGHT`](../../Reference/mod/MmodControl.md)is only available for synthetic models.  This control type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the right margin is always 10.0% of the width of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the width of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_BOX_MARGIN_TOP`

Sets a margin at the top of the bounding box of the model's active edges.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  The model box is used to compute the target score and to set the different masks of the model. If you change this value, all of this model's previous masks are discarded, since their size is no longer valid.  [`M_BOX_MARGIN_TOP`](../../Reference/mod/MmodControl.md) is only available for synthetic models.  This control type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these contexts, the top margin is always 10.0% of the height of the bounding box of the model's active edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the height of the bounding box of the model's active edges. |
| `Value >= 0.0` | Specifies the margin, in model units. |

---

### `M_CAD_Y_AXIS`

Sets the direction of the Y-axis for a model of type [`M_DXF_FILE`](../../Reference/mod/MmodDefineFromFile.md).  When the model is added to the Model Finder context from a CAD DXF file, the model will retain its coordinate system (the origin and the axis). Most CAD DXF files are oriented so that the Y-axis is positive going up, whereas the coordinate system Aurora Imaging Library uses is oriented with the Y-axis positive going down. So if you take the edge coordinates of an object from a CAD DXF file and put them in an image, the imaged object will look flipped when compared to the original object. This is also the case for a model defined from a CAD DXF file. This means that, unless the object is symmetrical, the match will not be made.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is [`M_FLIP`](../../Reference/mod/MmodControl.md) for a model of type [`M_DXF_FILE`](../../Reference/mod/MmodDefineFromFile.md). If the model is not of type [`M_DXF_FILE`](../../Reference/mod/MmodDefineFromFile.md), this control type must remain set to [`M_DEFAULT`](../../Reference/mod/MmodControl.md), otherwise an error is generated. |
| `M_FLIP` | Specifies to flip the Y-axis for the model so that the Y-axis is positive going down. |
| `M_NO_FLIP` | Specifies not to flip the Y-axis for the model. |

---

### `M_CERTAINTY`

Sets the certainty level for the score, as a percentage. If both the score and target scores are greater than or equal to their respective certainty levels, the occurrence is considered a match, without searching the rest of the target for better matches (provided the specified number of occurrences has been found).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the score, as a percentage.  If you set the certainty level too high (close to 100.0%), you slow down the search because you force the search algorithm to check the whole position range for the best possible match(es). A good certainty level is slightly lower than the expected score, so that the search can finish as soon as a match is found. However, if you set the certainty level too low, false matches might be found. |

---

### `M_CERTAINTY_TARGET`

Sets the certainty level for the target score. If both the score and target scores are greater than or equal to their respective certainty levels, the occurrence is considered a match, without searching the rest of the target for better matches (provided the specified number of occurrences has been found).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the target score, as a percentage. See the possible values of [`M_CERTAINTY`](../../Reference/mod/MmodControl.md) for more details. |

---

### `M_CORNER_RADIUS`

Sets the radius used to round all the corners of predefined-shaped models that have corners.  *[Image: DefineCornerRadius.png]*  This control type is only valid for models of type [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md), [`M_SQUARE`](../../Reference/mod/MmodDefine.md), [`M_DIAMOND`](../../Reference/mod/MmodDefine.md), [`M_TRIANGLE`](../../Reference/mod/MmodDefine.md), and [`M_CROSS`](../../Reference/mod/MmodDefine.md). Attempting to use this value for any other model type will generate an error.  This control type is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the radius, in model units. If you set the radius to 0.0, there will be no rounding. |

---

### `M_COVERAGE_MAX`

Specifies the maximum expected model coverage.  This control type is only supported for a model defined in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum expected model coverage. |

---

### `M_COVERAGE_MIN`

Specifies the minimum expected model coverage.  This control type is only supported for a model defined in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the minimum expected model coverage. |

---

### `M_DEVIATION_TOLERANCE`

Sets the tolerance for finding deformed rectangles or segments, for a model defined in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, respectively, given the other specified Model Finder constraints. This tolerance sets the allowable deformation of rectangles and line segments such that rectangles or segments that are not perfectly straight, or rectangles or segments that are broken up, would still be identified as potential occurrences.  This control type is only supported for a model in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tolerance as a percentage of the allowable deformation of a rectangle or segment, given the other Model Finder constraints.  A value of 0.0% means that the rectangular shape or segment being sought needs to be as close as possible to a perfect model. A value of 100.0% means that the algorithm has the maximum tolerance for finding deformed rectangles or segments. This could lead to finding distorted rectangular shapes or segments. |

---

### `M_ELLIPSE_TOLERANCE_TO_CIRCLE`

Sets the tolerance for approximating the aspect ratio of an ellipse occurrence to 1 (that is, reporting that the ellipse model matched a circle).  Internally, the full precision of the aspect ratio of an occurrence is used to find the best occurrences of an ellipse. When you retrieve the resulting aspect ratio and height of the ellipse occurrence(s), using [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_ASPECT_RATIO`](../../Reference/mod/MmodGetResult.md) and [`M_HEIGHT`](../../Reference/mod/MmodGetResult.md), the values are adjusted if the aspect ratio is close to 1 to account for pixelization and noise. If close enough to 1, the aspect ratio is returned as 1 and the height is returned as being the same as the width. You can use [`M_ELLIPSE_TOLERANCE_TO_CIRCLE`](../../Reference/mod/MmodControl.md) to control how close to 1 the aspect ratio must be for it to be rounded to 1 and for the height to be adjusted.  Aurora Imaging Library applies the tolerance to the aspect ratio between the ellipse's major and minor axis.  [`M_ELLIPSE_TOLERANCE_TO_CIRCLE`](../../Reference/mod/MmodControl.md) is only supported for a model defined in an[`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 0.5` *(default)* | Specifies the tolerance applied to the ellipse occurrence's aspect ratio. If [`M_ASPECT_RATIO`](../../Reference/mod/MmodControl.md)-1 is within the range (0,_Value_), then the ellipse is approximated as a circle. |

---

### `M_FIT_ERROR_WEIGHTING_FACTOR`

Sets the fit error weighting factor. This factor determines the contribution of the fit error in the score and target score calculation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the fit error weighting factor, as a percentage. The higher the percentage, the greater the contribution of the fit error in determining the score and target score. |

---

### `M_FIT_SCORE_MIN`

Sets the minimum expected occurrence fit score.  This control type is only supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum expected occurrence fit score. |

---

### `M_MIN_SEPARATION_ANGLE`

Sets the minimum angular separation required for two occurrences to be considered two distinct matches (two separate occurrences). Note, only one of the separation criteria needs to be met for occurrences to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Disables the minimum angle separation criteria. When disabled ([`M_DISABLE`](../../Reference/mod/MmodControl.md)), the angle is not a factor when determining if occurrences are distinct. |
| `0.0 < Value <= 180.0` *(default)* |  |

---

### `M_MIN_SEPARATION_ASPECT_RATIO`

Specifies the minimum separation required in aspect ratios, for two occurrences to be considered distinct matches. This value is specified as an aspect ratio factor. Note, only one of the separation criteria needs to be met for occurrences to be considered distinct.  This control type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum aspect ratio separation criteria. When disabled, the aspect ratio is not a factor when determining if occurrences are distinct. |
| `1.0 < Value <= 4.0` *(default)* | Specifies the criteria for minimum separation of aspect ratios. |

---

### `M_MIN_SEPARATION_SCALE`

Sets the minimum separation required in scale for two occurrences to be considered distinct matches (two separate occurrences). This value is specified as a scale factor. Note, only one of the separation criteria needs to be met for occurrences to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum scale separation criteria. When disabled, the scale is not a factor when determining if occurrences are distinct. |
| `1.0 < Value <= 4.0` *(default)* | Specifies the criteria for minimum separation in scale. |

---

### `M_MIN_SEPARATION_X`

Sets the minimum separation required along the X-axis for two occurrences to be considered distinct matches (two separate occurrences). Note, only one of the separation criteria needs to be met for occurrences to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum separation in X criteria. When disabled, separation along the X-axis is not a factor when determining if occurrences are distinct. |
| `Value` *(default)* | Specifies the minimum separation as a percentage of the model's width at [`M_SCALE`](../../Reference/mod/MmodControl.md). This value can be greater than 100%. |

---

### `M_MIN_SEPARATION_Y`

Sets the minimum separation required along the Y-axis for two occurrences to be considered distinct matches (two separate occurrences). Note, only one of the separation criteria needs to be met for occurrences to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the minimum separation in Y criteria. When disabled, separation along the Y-axis is not a factor when determining if occurrences are distinct. |
| `Value` *(default)* | Specifies the minimum separation as a percentage of the model's height at [`M_SCALE`](../../Reference/mod/MmodControl.md). This value can be greater than 100%. |

---

### `M_MIN_SIDE_COVERAGE`

Sets the minimum coverage on each of the sides of a rectangular model. Note that at least a portion of each side must be visible.  This control type is only supported for a model in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 100.0` *(default)* | Specifies the minimum side coverage. |

---

### `M_MODEL_ASPECT_RATIO`

Sets the nominal aspect ratio factor for the model. The nominal aspect ratio factor is inversely applied to the height of the model, leaving the model's width constant.  This control type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For other types of Model Finder contexts, see [`M_ASPECT_RATIO`](../../Reference/mod/MmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCLE_ASPECT_RATIO` | Specifies the minimum possible value for this control type. |
| `Value > 0.0` *(default)* | Specifies the value of the nominal aspect ratio for the model.  > **Note:** Note that the final aspect ratio must be greater than or equal to 1. That is to say that the width of the model divided by the height of the model, multiplied by the value set for this control type must be greater than or equal to 1. |

---

### `M_MODEL_ASPECT_RATIO_MAX_FACTOR`

Sets the factor used to determine the upper limit (maximum permitted aspect ratio) of the model's aspect ratio. [`M_MODEL_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) is multiplied by this factor. Occurrences with aspect ratios outside of this range cannot be returned as results.  This control type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies that the maximum factor for the aspect ratio of a model is infinite. |
| `Value >= 1.0` *(default)* | Specifies the factor that determines the maximum aspect ratio for a model. |

---

### `M_MODEL_ASPECT_RATIO_MIN_FACTOR`

Sets the factor used to determine the lower limit (minimum permitted aspect ratio) of the model's aspect ratio. [`M_MODEL_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) is multiplied by this factor. Occurrences with aspect ratios outside of this range cannot be returned as results.  This control type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCLE_ASPECT_RATIO` | Specifies the minimum factor for the aspect ratio of a model. |
| `0.0 < Value <= 1.0` *(default)* | Specifies the factor that determines the minimum aspect ratio for a model. |

---

### `M_PIXEL_SCALE`

Sets the pixel scale of the model, if it is a synthetic model. This value is the scale to apply to the model to go from model units to pixel units.  [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) is used for the match if the target is not calibrated; if the target is calibrated, this value is not used for the match nor for the returned results. Regardless if calibrated, this value will affect [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_ALLOC_SIZE_X`](../../Reference/mod/MmodInquire.md) and [`M_ALLOC_SIZE_Y`](../../Reference/mod/MmodInquire.md), and it will also be used for draw operations.  This is only valid for synthetic models.  This control type is not supported for a model defined in an[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For these types of contexts, model units are interpreted to be in the units of the target. If the target is calibrated, the units are interpreted to be in calibrated units. If the target is not, the units are interpreted to be in pixel units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value of the pixel scale.  If it is a calibrated model, the default is 1/(pixel size in X of the associated camera calibration).  If it is not a calibrated model, the default is 1.0. |
| `Value > 0.0` | Specifies the pixel scale.  Note that if the model is not a synthetic model, the only valid value is 1.0. Any other value will generate an error. |

---

### `M_POLARITY`

Sets the expected polarity of occurrences, compared to that of the model.  *[Image: PolaritiesCR.png]*  This control type is not supported for a model defined in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For a model defined in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_SAME`](../../Reference/mod/MmodControl.md).  For a model defined in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md), [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md), or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_SAME_OR_REVERSE`](../../Reference/mod/MmodControl.md). |
| `M_ANY` | Specifies that the occurrences can be a mixture of polarities. |
| `M_REVERSE` | Specifies that the polarity of occurrences is the reverse of that of the model. |
| `M_SAME` | Specifies that the polarity of occurrences is the same as that of the model. |
| `M_SAME_OR_REVERSE` | Specifies that the polarity of occurrences can be either the same or the reverse of that of the model. |

---

### `M_POSITION_DELTA_NEG_X`

Sets the valid position range in the negative X-direction, relative to the nominal position ([`M_POSITION_X`](../../Reference/mod/MmodControl.md) and [`M_POSITION_Y`](../../Reference/mod/MmodControl.md)).  The position range limits the region in which the position of a model occurrence can be found; position coordinates which fall outside this region cannot be returned as results ([`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_POSITION_X`](../../Reference/mod/MmodGetResult.md) and [`M_POSITION_Y`](../../Reference/mod/MmodGetResult.md)). Note that the position returned for an occurrence is determined by the model's reference axis position. The region defined by the position range can lie partially, or totally, outside the target.  Note that typically, to search for model occurrences within the position range, calculations specific to position-range search strategies should be enabled ([`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md)) for the context. When enabled, using a small position range generally decreases the search time, depending on the number of details present in the target. Always set the position range to the minimum required when speed is a consideration.  When [`M_POSITION_X`](../../Reference/mod/MmodControl.md) is set to [`M_ALL`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_X`](../../Reference/mod/MmodControl.md) will be treated as if set to [`M_INFINITE`](../../Reference/mod/MmodControl.md) regardless of its actual setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies the position range as the entire image plane in the negative X-direction. This means that any X-coordinate in the negative X-direction from the nominal position (even outside the target) can be returned as a result. |
| `Value >= 0.0` *(default)* | Specifies the position range's negative X-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_DELTA_NEG_Y`

Sets the valid position range in the negative Y-direction, relative to the nominal position ([`M_POSITION_X`](../../Reference/mod/MmodControl.md) and [`M_POSITION_Y`](../../Reference/mod/MmodControl.md)).  The position range limits the region in which the position of a model occurrence can be found; position coordinates which fall outside this region cannot be returned as results ([`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_POSITION_X`](../../Reference/mod/MmodGetResult.md) and [`M_POSITION_Y`](../../Reference/mod/MmodGetResult.md)). Note that the position returned for an occurrence is determined by the model's reference axis position. The region defined by the position range can lie partially, or totally, outside the target.  Note that typically, to search for model occurrences within the position range, calculations specific to position-range search strategies should be enabled ([`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md)) for the context. When enabled, using a small position range generally decreases the search time, depending on the number of details present in the target. Always set the position range to the minimum required when speed is a consideration.  When [`M_POSITION_Y`](../../Reference/mod/MmodControl.md) is set to [`M_ALL`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_Y`](../../Reference/mod/MmodControl.md) will be treated as if set to [`M_INFINITE`](../../Reference/mod/MmodControl.md) regardless of its actual setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies the position range as the entire image plane in the negative Y-direction. This means that any Y-coordinate in the negative Y-direction from the nominal position (even outside the target) can be returned as a result. |
| `Value >= 0.0` *(default)* | Specifies the position range's negative Y-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_DELTA_POS_X`

Sets the valid position range in the positive X-direction, relative to the nominal position ([`M_POSITION_X`](../../Reference/mod/MmodControl.md) and [`M_POSITION_Y`](../../Reference/mod/MmodControl.md)).  The position range limits the region in which the position of a model occurrence can be found; position coordinates which fall outside this region cannot be returned as results ([`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_POSITION_X`](../../Reference/mod/MmodGetResult.md) and [`M_POSITION_Y`](../../Reference/mod/MmodGetResult.md)). Note that the position returned for an occurrence is determined by the model's reference axis position. The region defined by the position range can lie partially, or totally, outside the target.  Note that typically, to search for model occurrences within the position range, calculations specific to position-range search strategies should be enabled ([`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md)) for the context. When enabled, using a small position range generally decreases the search time, depending on the number of details present in the target. Always set the position range to the minimum required when speed is a consideration.  When [`M_POSITION_X`](../../Reference/mod/MmodControl.md) is set to [`M_ALL`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_X`](../../Reference/mod/MmodControl.md) will be treated as if set to [`M_INFINITE`](../../Reference/mod/MmodControl.md) regardless of its actual setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies the position range as the entire image plane in the positive X-direction. This means that any X-coordinate in the positive X-direction from the nominal position (even outside the target) can be returned as a result. |
| `Value >= 0.0` | Specifies the position range's positive X-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_DELTA_POS_Y`

Sets the valid position range in the positive Y-direction, relative to the nominal position ([`M_POSITION_X`](../../Reference/mod/MmodControl.md) and [`M_POSITION_Y`](../../Reference/mod/MmodControl.md)).  The position range limits the region in which the position of a model occurrence can be found; position coordinates which fall outside this region cannot be returned as results ([`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_POSITION_X`](../../Reference/mod/MmodGetResult.md) and [`M_POSITION_Y`](../../Reference/mod/MmodGetResult.md)). Note that the position returned for an occurrence is determined by the model's reference axis position. The region defined by the position range can lie partially, or totally, outside the target.  Note that typically, to search for model occurrences within the position range, calculations specific to position-range search strategies should be enabled ([`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md)) for the context. When enabled, using a small position range generally decreases the search time, depending on the number of details present in the target. Always set the position range to the minimum required when speed is a consideration.  When [`M_POSITION_Y`](../../Reference/mod/MmodControl.md) is set to [`M_ALL`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_Y`](../../Reference/mod/MmodControl.md) will be treated as if set to [`M_INFINITE`](../../Reference/mod/MmodControl.md) regardless of its actual setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies the position range as the entire image plane in the positive Y-direction. This means that any Y-coordinate in the positive Y-axis from the nominal position (even outside the target) can be returned as a result. |
| `Value >= 0.0` | Specifies the position range's positive Y-offset, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the offset in the units of the target's coordinate system. |

---

### `M_POSITION_X`

Sets the nominal search position for the X-coordinate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies all X-coordinates. |
| `Value` | Specifies the nominal search position, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the position in the units of the target's coordinate system. |

---

### `M_POSITION_Y`

Sets the nominal search position for the Y-coordinate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies all Y-coordinates. |
| `Value` | Specifies the nominal search position, in pixels, and can be specified with subpixel accuracy; for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the position in the units of the target's coordinate system. |

---

### `M_REFERENCE_ANGLE`

Sets the angle of the reference axis for the model. Angle results are returned relative to this axis, rather than to the model source image axis. For example, you can define a model from a source image and then search the same image by passing the image buffer to [`MmodFind`](../../Reference/mod/MmodFind.md). If you then call [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_ANGLE`](../../Reference/mod/MmodGetResult.md) to retrieve the angle of the occurrence, which is identical in shape and orientation to the model, the returned angle of the occurrence will match [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md).  By default, for an ellipse model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the reference axis is aligned with the principal axis of the model (the width).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_REFERENCE_X`

Sets the X-offset of the origin of the model's reference axis, relative to the model origin. Position results return the X-coordinate of the model's reference axis origin transformed at the model occurrence. Position results are relative to the target origin.  Note that the reference axis origin need not be in the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default X-offset will be used.  For a synthetic model, the default value is 0.0. For other types of models, the default value is the center of the model. |
| `Value` | Specifies the X-offset value. The value is in pixels, and can be specified with subpixel accuracy. |

---

### `M_REFERENCE_Y`

Sets the Y-offset of the origin of the model's reference axis, relative to the model origin. Position results return the Y-coordinate of the model's reference axis origin transformed at the model occurrence. Position results are relative to the target origin.  Note that the reference axis origin need not be in the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default Y-offset will be used.  For a synthetic model, the default value is 0.0. For other types of models, the default value is the center of the model. |
| `Value` | Specifies the Y-offset value. The value is in pixels, and can be specified with subpixel accuracy. |

---

### `M_SAGITTA_TOLERANCE`

Sets the tolerance for finding deformed circles (allowable radii variation) for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, given the other specified Model Finder constraints.  This control type is only supported for a model in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tolerance as a percentage of the allowable radii variation given the other Model Finder constraints.  A value of 0.0% means that the circular shape being sought needs to be as close as possible to a perfect circle. A value of 100.0% means that the algorithm has the maximum tolerance for finding deformed circles. This could lead to finding elliptical shapes or circular type shapes with noise around the edges. |

---

### `M_SCALE`

Sets the nominal search scale.  For a model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, scale calculations are taken with respect to the model's width. The model's height is not taken into account. Use [`M_MODEL_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) to account for height differences. For example, if the model has a width of 4 and a height of 2 and the occurrence is expected to have a height of 2 and a width of 8, set [`M_SCALE`](../../Reference/mod/MmodControl.md) to 2 and [`M_MODEL_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) to 2. Recall that the width of a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, or the width of an occurrence of this type of model, is always measured along its principal axis.  For a model defined in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, scale calculations are taken with respect to the model's length.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the value of the nominal search scale.  For a synthetic model, the nominal scale can be any positive value. For all other models, the nominal scale must be from 0.5 to 2.0. |

---

### `M_SCALE_MAX_FACTOR`

Sets the factor used to determine the upper limit (maximum permitted scale) of the scale range. [`M_SCALE`](../../Reference/mod/MmodControl.md) is multiplied by this factor. Occurrences with scales outside the scale-range cannot be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies that the upper limit of the scale range is infinite.  > **Note:** This control value is only supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `1.0 <= Value <= 2.0` *(default)* | Specifies the factor that determines the maximum scale of the occurrence for a model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `Value >= 1.0` | Specifies the factor that determines the maximum scale of the occurrence for a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |

---

### `M_SCALE_MIN_FACTOR`

Sets the factor used to determine the lower limit (minimum permitted scale) of the scale range. [`M_SCALE`](../../Reference/mod/MmodControl.md) is multiplied by this factor. Occurrences with scales outside the scale-range cannot be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 1.0` *(default)* | Specifies the factor that determines the minimum scale of the occurrence for a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `0.5 <= Value <= 1.0` | Specifies the factor that determines the minimum scale of the occurrence for a model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |

---

### `M_SEARCH_ASPECT_RATIO_CONSTRAINT`

Sets whether to constrain candidates to the nearest bound of the aspect ratio range if the candidate is outside of the range. For example, if the maximum effective aspect ratio being sought is 2.0, and a candidate's aspect ratio is 2.3, and [`M_SEARCH_ASPECT_RATIO_CONSTRAINT`](../../Reference/mod/MmodControl.md) is set to [`M_ENABLE`](../../Reference/mod/MmodControl.md), the candidate can be identified as an occurrence, with a reduced score, while if [`M_SEARCH_ASPECT_RATIO_CONSTRAINT`](../../Reference/mod/MmodControl.md) is set to [`M_DISABLE`](../../Reference/mod/MmodControl.md), it is not.  This control type is only supported for a model in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that for a candidate to be considered an occurrence, it must have an aspect ratio within the defined aspect ratio range. Using this control value will limit the number of occurrences returned, but they will all conform to the defined aspect ratio range. |
| `M_ENABLE` *(default)* | Specifies to consider candidates with an aspect ratio that falls outside of the specified aspect ratio range, but constrain their fit to the closest bound of the aspect ratio range, and reduce their score accordingly. This allows for more potential occurrences to be returned. |

---

### `M_SEARCH_POSITION_FROM_GRAPHIC_LIST`

Specifies to use a search region defined from a 2D graphics list.  The 2D graphics list must contain a maximum of one graphic of type rectangle ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md), [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md), or [`MgraRectFill`](../../Reference/gra/MgraRectFill.md)) and have its input units set to pixels ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md) set to [`M_PIXEL`](../../Reference/gra/MgraControlList.md)).  For models in [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) contexts, the rectangular graphic in your 2D graphics list must be defined with an angle of 0 degrees. For models in [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) contexts, the rectangular graphic in your 2D graphics list can have any angle.  Using this value is effectively equivalent to making multiple calls to [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_POSITION_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_Y`](../../Reference/mod/MmodControl.md), and [`M_ANGLE_REGION`](../../Reference/mod/MmodControl.md).

| Value | Description |
| --- | --- |
| `2D graphics list identifier` | Specifies the identifier of the 2D graphics list containing the rectangular region that will be used as the model search region. |

---

### `M_SEGMENT_CONSISTENT_GRADIENT`

Sets whether the gradients of the segment model occurrences need to be consistent.  This control type is only supported for a model in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the segment model occurrences can have the same gradients or reverse gradients. |
| `M_ENABLE` *(default)* | Specifies that the gradients of the segment model occurrences must be consistent. |

---

### `M_USER_LABEL`

Sets a unique user-defined label for the specified model. This label can be used as a means of identifying your model, independently from its index, in the Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NO_LABEL` *(default)* | Specifies that no user label is associated with the model. |
| `Value` | Specifies the user label of the model. The value must be an integer value that is not associated as a label with any other model in the Model Finder context. Since the same label cannot be associated with multiple models, you cannot pass a user label if the [`Index`](../../Reference/mod/MmodControl.md) parameter of this function is set to [`M_ALL`](../../Reference/mod/MmodControl.md). |

### For a result buffer

The following [`ControlType`](../../Reference/mod/MmodControl.md) and corresponding [`ControlValue`](../../Reference/mod/MmodControl.md) settings can be specified for a Model Finder result buffer:

---

### `M_MOD_DEFINE_COMPATIBLE`

Saves all the necessary information in the result buffer to be able to define a model from a result occurrence ([`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_MOD_RESULT`](../../Reference/mod/MmodDefine.md)).  This control type is not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to save any information. |
| `M_ENABLE` | Specifies to save all the necessary information. |

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) generates an error if the result was not calculated on a calibrated image. |

This control type is not supported for an[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

DISTINCT OCCURRENCE = (Separation in X) OR (Separation in Y) OR (Separation in Angle) OR (Separation in Scale) OR (Separation in Aspect Ratio).

Note that typically, to search for model occurrences within the angular range, calculations specific to angular-range search strategies should be enabled ([`M_SEARCH_ANGLE_RANGE`](../../Reference/mod/MmodControl.md)) for the context. When enabled, the angular range should be used to cover an expected variance in angle. Note that the actual angle of the occurrence does not affect search speed. If you need to search for a model at discrete angles only (for example, at intervals of 90 degrees), it is typically more efficient to define several models with different expected angles, than to search through the full angular range.

Note that typically, to search for model occurrences within the scale range, calculations specific to scale-range search strategies should be enabled ([`M_SEARCH_SCALE_RANGE`](../../Reference/mod/MmodControl.md)) for the context. When enabled, the scale range should only cover the expected variance in scale to avoid slowing down the search and avoid finding unwanted occurrences. A search through a range of scales is performed in parallel, meaning that the actual scale of an occurrence has no bearing on which occurrence will be found first.

This control type is not supported for a model defined in an[`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

This control type is not supported for a model defined in an[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.
