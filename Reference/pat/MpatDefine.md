---
doctype: Reference
module: pat
function: MpatDefine
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / pat / MpatDefine"
---

# MpatDefine

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

> Add one or more models to, or delete a model from, a Pattern Matching context.

## Syntax

```c
void MpatDefine(
    AIL_ID     ContextPatId,   //out
    AIL_INT64  ModelType,      //in
    AIL_ID     SrcImageBufId,  //in
    AIL_DOUBLE Param1,         //in
    AIL_DOUBLE Param2,         //in
    AIL_DOUBLE Param3,         //in
    AIL_DOUBLE Param4,         //in
    AIL_INT64  ControlFlag     //in
)
```

## Description

This function allows you to add one or more models at a time to, or delete a model from, a Pattern Matching context. When defining a search model, you are specifying the area of an image to use as a pattern to find within a target image, using [`MpatFind`](../../Reference/pat/MpatFind.md). When a call to [`MpatFind`](../../Reference/pat/MpatFind.md) is made, all models that are part of the specified context (and that have [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_NUMBER`](../../Reference/pat/MpatControl.md) set to a value greater than zero) are searched for simultaneously in the target. Note, when a model is added or deleted, the Pattern Matching context must be preprocessed again, using [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md).

The search is performed according to the general search settings specified in the Pattern Matching context, as well as the individual model search settings (depending on the [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) value specified). Specify both the context and individual model search settings using [`MpatControl`](../../Reference/pat/MpatControl.md).

When adding a model to the context, a number is assigned to the new model. The context starts numbering the models at 0, and each subsequent model added to the Pattern Matching context is given a sequential index number, in the order that you add the models. When deleting a model, all models with a higher index shift down one.

## Parameters

### `ContextPatId` *(out, AIL_ID)*

Specifies the Pattern Matching context to which to add, or from which to delete, the model. The Pattern Matching context must have been previously allocated on the required system using [`MpatAlloc`](../../Reference/pat/MpatAlloc.md).

### `ModelType` *(in, AIL_INT64)*

Specifies the type of the model to define when adding models to the context, or specifies to delete a model from the Pattern Matching context.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer from which to define the models, or the Pattern Matching context from which to copy a model.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen. When not used, set this parameter to `M_DEFAULT`.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen. When not used, set this parameter to `M_DEFAULT`.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen. When not used, set this parameter to `M_DEFAULT`.

### `Param4` *(in, AIL_DOUBLE)*

Specifies an attribute of the model to add. Its definition is dependent on the model type chosen. When not used, set this parameter to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the speed of the allocation or the interpolation mode to use, depending on the model type chosen. When not used, set this parameter to `M_DEFAULT`.

## Parameter Associations

### For defining models to add to specified Pattern Matching context

To add a model to the Pattern Matching context, the [`ModelType`](../../Reference/pat/MpatDefine.md), [`Param1`](../../Reference/pat/MpatDefine.md), [`Param2`](../../Reference/pat/MpatDefine.md), [`Param3`](../../Reference/pat/MpatDefine.md), and [`ControlFlag`](../../Reference/pat/MpatDefine.md) parameters can be set to the following values:  Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/pat/MpatDefine.md). Note, if unused, set [`SrcImageBufId`](../../Reference/pat/MpatDefine.md)to [`M_NULL`](../../Reference/pat/MpatDefine.md).

---

### `M_AUTO_MODEL`

Defines one or more models automatically of the specified dimensions from the most-suitable distinct areas found, in the source image. If none are found, no model is allocated and an error is reported. This is useful when you want to perform whole image alignment, for which allocation of a distinct model is essential. It can take several seconds to find the best models (more for large or small images).  To be effective, the source image should be a typical target image.  You can only define an[`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md) that respect the following condition: `(_maxvalue_ <sup>2</sup> *`   [`Param3`](../../Reference/pat/MpatDefine.md) * [`Param4`](../../Reference/pat/MpatDefine.md))`&lt; 2<sup>63</sup>` where _maxvalue_ is the maximum pixel value (typically, 255) in the target image and the model. This restriction is imposed to avoid overflows in the internal 64-bit accumulators.  The total area of the model must be greater or equal to 4 pixels ([`Param3`](../../Reference/pat/MpatDefine.md) * [`Param4`](../../Reference/pat/MpatDefine.md) >= 4).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Selects models from anywhere in the whole width of the image. |
| `Value >= 0` | Specifies the expected maximum pixel displacement in the horizontal direction, as an _integer_. |
| `M_DEFAULT` | Selects models from anywhere in the whole height of the image. |
| `Value >= 0` | Specifies the expected maximum pixel displacement in the vertical direction, as an _integer_. |
| `M_DEFAULT` |  |
| `M_BEST` | Specifies that the search should favor high precision over speed. You should typically use this setting if the model source image is small. |
| `M_FAST` *(default)* | Specifies that the search should favor speed over high precision. You should typically use this setting if your model source image is large. |

---

### `M_REGULAR_MODEL`

Defines a regular model, from the specified area, in the source image. You can use this type of model when dealing with images containing a mix of models on an inconsistent background, such as an image of loose nuts and bolts lying on a metal sheet.  If the occurrence can appear at an angle, specify the angular range in which it can appear, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md). If the occurrence is expected to be found always with the same background, use the [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) combination constant.  To extract the model from a rotated region in the model's source image, pass a source image that is associated with a rectangular region of interest (ROI) at the required angle. You can associate an ROI to your source image using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).  You can only define an [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) model that respects the following condition: `(_maxvalue_ <sup>2</sup> *`   [`Param3`](../../Reference/pat/MpatDefine.md) * [`Param4`](../../Reference/pat/MpatDefine.md))`&lt; 2<sup>63</sup>`, where _maxvalue_ is the maximum pixel value (typically, 255) in the target image and the model. This restriction is imposed to avoid overflows in the internal 64-bit accumulators.  The total area of the model must be greater or equal to 4 pixels ([`Param3`](../../Reference/pat/MpatDefine.md) * [`Param4`](../../Reference/pat/MpatDefine.md) >= 4).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the X-coordinate of the model origin is 0. Alternatively, if the model's source image is associated with an ROI, it specifies to ignore this parameter. |
| `0 <= Value <= ImageSizeX` | Specifies the X-coordinate of model origin in the source image, in pixels (as an _integer_). |
| `M_DEFAULT` | Specifies that the Y-coordinate of the model origin is 0. Alternatively, if the model's source image is associated with an ROI, it specifies to ignore this parameter. |
| `0 <= Value <= ImageSizeY` | Specifies the Y-coordinate of model origin in the source image, in pixels (as an _integer_). |
| `M_DEFAULT` | Specifies that the width of the model is the distance from the origin of the model to the edge of the image in the X-direction. Alternatively, if the model's source image is associated with an ROI, it specifies to ignore this parameter. |
| `1 <= Value <= ImageSizeX` | Specifies the width of the model in the model source image, in pixels (as an _integer_). |
| `M_DEFAULT` | Specifies that the height of the model is the distance from the origin of the model to the edge of the image in the Y-direction. Alternatively, if the model's source image is associated with an ROI, it specifies to ignore this parameter. |
| `1 <= Value <= ImageSizeY` | Specifies the height of the model in the model source image, in pixels (as an _integer_). |

### Combination Constants — For extracting the circular overscan data

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to extract the model as well as a circular area around it.

| Value | Description |
| --- | --- |
| `M_CIRCULAR_OVERSCAN` | Specifies that the model is extracted from the source image with the circular overscan data.  The circular overscan data is determined by rotating the model, about its center. The circular overscan data of an [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) type model is used if you search for an occurrence of the model at an angle.  Angular search is fastest when performed with an [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) model; however, [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) should only be used when the region around the model is consistent, for example, when searching for a chip in the image of an integrated circuit.  When preprocessing an [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) model for which an angular search range has been specified, a set of models is extracted from rotated versions of the [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) model, defining models that would appear upright if the target image were rotated. For this type of model, a larger region than the one defined is extracted from the source image so as to allow creation of models at different angles.  The model must not be extracted from a region too close to the edge of the source image. In addition, the [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) model should be complex enough so that if models are created from it at the required angles, they are representative of the pattern being sought.  For a model defined to include circular overscan data, you cannot define "don't care" regions ([`MpatMask`](../../Reference/pat/MpatMask.md)). |

### Combination Constants — For use with the auto model context

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the number of models to allocate from the same image.

| Value | Description |
| --- | --- |
| `M_MULTIPLE + n` | Allocates several models from the same source image. Set _n_ to the number of models to define.  For example, set [`ControlFlag`](../../Reference/pat/MpatDefine.md) to [`M_BEST`](../../Reference/pat/MpatDefine.md)+[`M_MULTIPLE`](../../Reference/pat/MpatDefine.md)+ 4 to define the four most distinct models available in your model source image. Search models can overlap within the image by half the size of the model (in X and Y). |

### For deleting the specified model from the Pattern Matching context

When deleting a model from the specified Pattern Matching context, use the following value:  Any unused parameters should be set to [`M_DEFAULT`](../../Reference/pat/MpatDefine.md). Note, if unused, set [`SrcImageBufId`](../../Reference/pat/MpatDefine.md)to [`M_NULL`](../../Reference/pat/MpatDefine.md).

---

### `M_DELETE`

Specifies to remove the specified model from the Pattern Matching context, specified using the [`Param1`](../../Reference/pat/MpatDefine.md) parameter. When deleting a model, all models with a higher index shift down one.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to delete all models from the Pattern Matching context. |
| `Value >= 0` | Specifies the index of the model to delete. |
