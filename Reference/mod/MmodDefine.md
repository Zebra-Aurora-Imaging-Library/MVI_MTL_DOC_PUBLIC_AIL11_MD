---
doctype: Reference
module: mod
function: MmodDefine
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodDefine"
---

# MmodDefine

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

> Add a model to, or delete a model from, a Model Finder context.

## Syntax

```c
void MmodDefine(
    AIL_ID     ContextId,  //out
    AIL_INT64  ModelType,  //in
    AIL_DOUBLE Param1,     //in
    AIL_DOUBLE Param2,     //in
    AIL_DOUBLE Param3,     //in
    AIL_DOUBLE Param4,     //in
    AIL_DOUBLE Param5      //in
)
```

## Description

This function allows you to add a model to, or delete a model from, a Model Finder context. To an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, you can add a model from an image, from a result buffer, from two other models, or you can add a synthetic model. Image-type, result-type, merge-type, and synthetic models can be mixed in the same Model Finder context. To an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, you can only add a single synthetic model of the same type (for example, to an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of context, you can only add a single synthetic [`M_CIRCLE`](../../Reference/mod/MmodDefine.md) model).

There are two types of image-type models: models for which you manually define their location in the model source image ([`M_IMAGE`](../../Reference/mod/MmodDefine.md)), and models that Aurora Imaging Library automatically defines from the model source image ([`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md)). [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md) defines a unique model by searching the model source image for unique geometric features. [`MmodDefine`](../../Reference/mod/MmodDefine.md) can define an automatic model based on default settings. To have specified settings taken into account during model definition, you must allocate an empty image-type model using [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md), set the appropriate control types using [`MmodControl`](../../Reference/mod/MmodControl.md), and then call [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_AUTO_DEFINE`](../../Reference/mod/MmodDefine.md). It can take several seconds to find the best model (or more for large or small images). To be useful, the model source image should be a typical target image; otherwise, the selected area for the model might never be present in the target image. For more information on defining a model automatically, see [Image-type models](../../UserGuide/C08_Geometric_Model_Finder/S05_Defining_and_adding_models_to_your_model_finder_context.md).

There are two types of result-type models: models defined from an Edge Finder result buffer (Edge Finder-type models), and models defined from a Model Finder result buffer (Model Finder-type model).

Merge-type models are composed of the edges of two different models. When you define a merge-type model, the two models from which it is merged are not affected.

There are two types of synthetic models: models of a predefined shape (predefined-shape models) and models defined from a CAD DXF file (CAD-file models). Use [`MmodDefineFromFile`](../../Reference/mod/MmodDefineFromFile.md) to define a CAD-file model.

By default, synthetic models are defined to have a 10% margin around the bounding box of their active edges. For a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, this margin is used for drawing. For a model in an[`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context, any extra edges found in this area, when finding an occurrence, will reduce the target score. Regardless of the context, you can change the size of the margins with the [`MmodControl`](../../Reference/mod/MmodControl.md)   [`M_BOX_MARGIN_...`](../../Reference/mod/MmodControl.md) control types.

How you specify the dimensions of synthetic models depends on the type of context:

- For an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the dimensions of synthetic models in user-defined units. Then, specify the ratio between model units and pixel units using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md). If the target is not calibrated, then [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) is used for the match. If the target is calibrated, the model units should be the same as the calibrated units. If the target is calibrated, [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) will be used for draw operations, but not for the match nor the returned results. If you are searching for a synthetic model in a calibrated target, you must also associate the camera calibration context of the target with the model, using [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md). Aurora Imaging Library needs the camera calibration context for internal purposes at preprocessing time. Aurora Imaging Library will not use the camera calibration context to compensate for incongruencies between the model and the target, nor will Aurora Imaging Library map the model's coordinate system to that of the target.
  > **Note:** After you have specified your pixel scale and scale settings, you can verify if your synthetic models are valid, using [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_VALID`](../../Reference/mod/MmodInquire.md). Synthetic models can be invalid if their global size in X or Y (_size of the model box _ * [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) * [`M_SCALE`](../../Reference/mod/MmodControl.md)) is less than 16 or greater than 1024.
- For an[`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, specify the dimensions of the synthetic models in the units of the target; [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) and [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md) are not used.

If the image or the result buffer used to define the model is calibrated, then that camera calibration context is automatically associated with the model. Models can use different camera calibration contexts within the same Model Finder context, provided all camera calibrations map to the same world. All image-type or result-type models must be either calibrated or not.

When a call to [`MmodFind`](../../Reference/mod/MmodFind.md) is made, all models that are part of the specified context are searched for simultaneously in the target.

Note, when a model is added or deleted, the Model Finder context must be preprocessed again, using [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md).

The search is performed according to the general search settings specified in the Model Finder context ([`M_CONTEXT`](../../Reference/mod/MmodControl.md)), as well as the individual model search settings. Both the context and individual model search settings can be specified using [`MmodControl`](../../Reference/mod/MmodControl.md).

The model index starts at 0, and each subsequent model added to the Model Finder context is given a sequential index number, in the order that the model was added. If a model is deleted, all entries with higher indices are shifted down one.

For more information on defining models, see [Guidelines for choosing models](../../UserGuide/C08_Geometric_Model_Finder/S06_Guidelines_for_choosing_models.md).

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the Model Finder context to which to add, or from which to delete, the model. The Model Finder context must have been previously allocated on the required system using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md).

### `ModelType` *(in, AIL_INT64)*

Specifies the type of model to define when adding models to the context, or specifies to delete a model from the Model Finder context.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen.

### `Param4` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen.

### `Param5` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen.

## Parameter Associations

### For adding a model to the context

To add a model to the context, the [`ModelType`](../../Reference/mod/MmodDefine.md), [`Param1`](../../Reference/mod/MmodDefine.md), [`Param2`](../../Reference/mod/MmodDefine.md), [`Param3`](../../Reference/mod/MmodDefine.md), [`Param4`](../../Reference/mod/MmodDefine.md), and [`Param5`](../../Reference/mod/MmodDefine.md) parameters can be set to the following values. Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/mod/MmodDefine.md).

---

### `M_AUTO_DEFINE`

Defines a unique model whose features are extracted from an automatically selected region of the model source image.  To define a model automatically based on specified control settings, call this function with [`Param1`](../../Reference/mod/MmodDefine.md) set to [`M_NULL`](../../Reference/mod/MmodDefine.md). Then, specify control settings using [`MmodControl`](../../Reference/mod/MmodControl.md). Once the controls are set, define the model using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_AUTO_DEFINE`](../../Reference/mod/MmodControl.md).  To define a model automatically based on default control settings, call this function with [`Param1`](../../Reference/mod/MmodDefine.md) set to the identifier of the model source image.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that an empty model should be defined. |
| `Source image identifier` | Sets the identifier of the image buffer (model source image) from which to extract a unique model. The image buffer must be a 1-band 8-bit unsigned buffer. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `M_DEFAULT` | Selects a model from anywhere in the whole width of the image. |
| `Value` | Specifies the maximum expected pixel displacement in the horizontal direction. |
| `M_DEFAULT` | Selects a model from anywhere in the whole height of the image. |
| `Value` | Specifies the maximum expected pixel displacement in the verical direction. |

---

### `M_CIRCLE`

Specifies a predefined circle as the model.  *[Image: DefineCircle.png]*  This type of model is only supported for an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md), [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_ANY`](../../Reference/mod/MmodDefine.md).  For an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_FOREGROUND_BLACK`](../../Reference/mod/MmodDefine.md). |
| `M_ANY` | Specifies that the model has no specific polarity.  > **Note:** This setting is not supported for an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

---

### `M_CROSS`

Specifies a predefined cross as the model.  *[Image: DefineCross.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the model has no specific polarity. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

---

### `M_DIAMOND`

Specifies a predefined diamond as the model.  *[Image: DefineDiamond.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ANY`](../../Reference/mod/MmodDefine.md). |
| `M_ANY` | Specifies that the model has no specific polarity. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

---

### `M_EDGE_RESULT`

Defines a model from the results of an edge extraction performed using Edge Finder.  To define a model from an Edge Finder result buffer, the buffer must have been previously allocated using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md). [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have been already called. In addition, the Edge Finder result buffer must be compatible with the Model Finder context. Enable compatibility using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_MODEL_FINDER_COMPATIBLE`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).  The model is defined to have all currently included edges in the result buffer.  In the case of an Edge Finder-type model, the model source image is the image from which the Edge Finder results were obtained.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the X-offset of the model origin is the origin of the model source image. The X-offset is 0. |
| `Value` | Specifies the X-offset of the model origin in the model source image, in pixels. |
| `M_DEFAULT` | Specifies that the Y-offset of the model origin is the origin of the model source image. The Y-offset is 0. |
| `Value` | Specifies the Y-offset of the model origin in the model source image, in pixels. |
| `M_DEFAULT` | Specifies that the width of the model is the width of the model source image. |
| `16.0 <= Value <= 1024.0` | Specifies the width of the model (in X), in pixels. |
| `M_DEFAULT` | Specifies that the height of the model is the height of the model source image. |
| `16.0 <= Value <= 1024.0` | Specifies the height of the model (in Y), in pixels. |

---

### `M_ELLIPSE`

Specifies a predefined ellipse as the model.  *[Image: DefineEllipse.png]*  > **Note:** Note that when specifying this type of model using an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the width of the model must be larger than or equal to the height.  This type of model is only supported for an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md), [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_ANY`](../../Reference/mod/MmodDefine.md).  For an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_FOREGROUND_BLACK`](../../Reference/mod/MmodDefine.md). |
| `M_ANY` | Specifies that the model has no specific polarity.  > **Note:** This setting is not supported for an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

---

### `M_IMAGE`

Defines a model from the specified model source image. The default settings use the entire image as the model. For image-type models, the minimum and maximum model sizes are 16.0 x 16.0 and 4096.0 x 4096.0 pixels, respectively.  To extract the model from a rotated region in the model source image, pass a model source image that is associated to a rectangular region of interest (ROI) at the required angle. You can associate an ROI to your source image using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the X-offset of the model origin is the origin of the model source image, that is, the X-offset is 0. Alternatively, if the model source image is associated with an ROI, it specifies to ignore this parameter. |
| `Value` | Specifies the X-offset of the model origin in the source image, in pixels. |
| `M_DEFAULT` | Specifies that the Y-offset of the model origin is the origin of the model source image, that is, the Y-offset is 0. Alternatively, if the model source image is associated with an ROI, it specifies to ignore this parameter. |
| `Value` | Specifies the Y-offset of the model origin in the source image, in pixels. |
| `M_DEFAULT` | Specifies that the width of the model is the distance from the origin of the model to the edge of the image in the X-direction. Alternatively, if the model source image is associated with an ROI, it specifies to ignore this parameter. |
| `16.0 <= Value <= 4096.0` | Specifies the width of the model (in X), in pixels. |
| `M_DEFAULT` | Specifies that the height of the model is the distance from the origin of the model to the edge of the image in the Y-direction. Alternatively, if the model source image is associated with an ROI, it specifies to ignore this parameter. |
| `16.0 <= Value <= 4096.0` | Specifies the height of the model (in Y), in pixels. |

---

### `M_MERGE_MODEL`

Defines a model from two other models. These models have to be in the same Model Finder context.  The model is defined with the edges of the first model and the edges of the second model that are not equal to the edges of the first model.  The size of the merge-type model is the size of the first model. To include all of the edges of both models in the new model, set the first parameter to the index of the larger model.  If the models being merged are calibrated, the new merge-type model will be associated with the camera calibration context of the first model.

---

### `M_MOD_RESULT`

Defines a model from a Model Finder result buffer.  The model is defined with the edges of the target at the position of the specified occurrence.  To define a model from a Model Finder result buffer, enable [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_MOD_DEFINE_COMPATIBLE`](../../Reference/mod/MmodControl.md). In the case of a Model Finder-type model, the model source image is the target from which the results were obtained.

---

### `M_RECTANGLE`

Specifies a predefined rectangle as the model.  *[Image: DefineRectangle.png]*  If adding the model to an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context, you can round the corners of this model using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_CORNER_RADIUS`](../../Reference/mod/MmodControl.md).  This type of model is only supported for an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md), [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_ANY`](../../Reference/mod/MmodDefine.md).  For an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default value is [`M_FOREGROUND_BLACK`](../../Reference/mod/MmodDefine.md). |
| `M_ANY` | Specifies that the model has no specific polarity.  > **Note:** This setting is not supported for an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

---

### `M_RING`

Specifies a predefined ring as the model.  *[Image: DefineRing.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the model has no specific polarity. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

---

### `M_SEGMENT`

Specifies a predefined segment as the model.  *[Image: DefineSegment.png]*  This type of model is only supported for an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

---

### `M_SQUARE`

Specifies a predefined square as the model.  *[Image: DefineSquare.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the model has no specific polarity. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

---

### `M_TRIANGLE`

Specifies a predefined triangle as the model.  *[Image: DefineTriangle.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ANY`](../../Reference/mod/MmodDefine.md). |
| `M_ANY` | Specifies that the model has no specific polarity. |
| `M_FOREGROUND_BLACK` | Specifies that black is the foreground color of the model. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the model. |

### For removing a model from the context

The following [`ModelType`](../../Reference/mod/MmodDefine.md) parameter setting can be used to remove a model from the context.  Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/mod/MmodDefine.md).

---

### `M_DELETE`

Deletes the specified model from the context.

This type of model is not supported for [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder contexts.

You can round the corners of this model using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_CORNER_RADIUS`](../../Reference/mod/MmodControl.md).
