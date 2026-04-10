---
doctype: Reference
module: agm
function: MagmDefine
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmDefine"
---

# MagmDefine

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

> Add a model to, or delete a model from, an AGM context.

## Syntax

```c
AIL_INT64 MagmDefine(
    AIL_ID    ContextId,   //out
    AIL_INT64 Operation,   //in
    AIL_INT64 SourceType,  //in
    AIL_INT64 ModelType,   //in
    AIL_INT64 Param1,      //in
    AIL_INT64 ControlFlag  //in
)
```

## Description

This function allows you to add a model to, or delete a model from, a find AGM context or a train AGM context. Note, when a model is added or deleted, the AGM context must be preprocessed again, using [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md). You can only define one model per AGM context. When adding a model to the context, it is assigned an index of 0.

Note that a composite-definition model must be trained before performing an [`MagmFind`](../../Reference/agm/MagmFind.md) operation. This means you cannot add a composite-definition model to a find AGM context using [`MagmDefine`](../../Reference/agm/MagmDefine.md). You must add it to a train AGM context using [`MagmDefine`](../../Reference/agm/MagmDefine.md), train the model using [`MagmTrain`](../../Reference/agm/MagmTrain.md), and then copy the trained model to a find AGM context using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md).

The [`MagmFind`](../../Reference/agm/MagmFind.md) and [`MagmTrain`](../../Reference/agm/MagmTrain.md) operations are performed according to the individual model settings specified using [`MagmControl`](../../Reference/agm/MagmControl.md).

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the identifier of the AGM context in which to add or delete a model. The AGM context must have been previously allocated on the required system using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md) or [`M_GLOBAL_EDGE_BASED_TRAIN`](../../Reference/agm/MagmAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies whether to add or remove a model.

*Specifies the operation*

| Value | Description |
| --- | --- |
| `M_ADD` | Specifies to add a model to the specified AGM context. |
| `M_DELETE` | Specifies to delete a model from the specified AGM context. |

### `SourceType` *(in, AIL_INT64)*

Specifies the type of source from which to define the model.

### `ModelType` *(in, AIL_INT64)*

Specifies the type of model to define when adding models to the AGM context.

### `Param1` *(in, AIL_INT64)*

Specifies an attribute of the model.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For adding a model to the AGM context

To add a model to an AGM context, the [`ModelType`](../../Reference/agm/MagmDefine.md), [`SourceType`](../../Reference/agm/MagmDefine.md), and [`Param1`](../../Reference/agm/MagmDefine.md) parameters can be set to the following values. The [`Operation`](../../Reference/agm/MagmDefine.md) parameter must be set to [`M_ADD`](../../Reference/agm/MagmDefine.md).

---

### `M_COMPOSITE`

Specifies to add a composite-definition model. A composite-definition model uses training images, labeled with model appearances and background areas that can be incorrectly misconstrued as occurrences, to construct the best possible model.  A composite-definition model can be used to improve results if a previous [`MagmFind`](../../Reference/agm/MagmFind.md) operation with a single-definition model found false occurrences. Training a composite-definition model, using [`MagmTrain`](../../Reference/agm/MagmTrain.md), identifies possible backgrounds, which can result in a model that can better discriminate true occurrences from false occurrences, especially in cluttered target images.  Since a composite-definition model requires training before it can be used to search for occurrences, this type of model can only be added to a train AGM context.

#### `M_DEFAULT`

Specifies the default behavior.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this parameter is unused. You do not specify a model image because the model is built from training images, using [`MagmTrain`](../../Reference/agm/MagmTrain.md). |

---

### `M_SINGLE`

Specifies to add a single-definition model. A single-definition model can be defined from a CAD DXF file, Edge Finder result buffer, or a single source image. In the latter case, the entire image is used as the model or the model is extracted from a rotated or non-rotated region.  Since a single-definition model does not require training, this type of model can only be added to a find AGM context.

#### `M_DXF_FILE`

Specifies to define the model from the entities in the specified CAD DXF file. This function does not support all entities that are possible in a CAD DXF file. If there are unsupported entities in the file, they are ignored, and the rest of the entities are read. The supported entities are as follows: LINE, ARC, CIRCLE, ELLIPSE, INSERT, POLYLINE, and LWPOLYLINE (including bulge information). For INSERT entities, only single insertions are supported; multiple insertions using the MINSERT command are not supported. Extrusion is also not supported.  A model defined from a CAD DXF file has no polarity.

| Value | Description |
| --- | --- |
| `"DxfFilePath"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_).  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"`, for example: `"remote:///C:\mydirectory\myfile"`. |

#### `M_EDGE_RESULT`

Specifies to define the model from the results of an edge extraction performed using Edge Finder.  To define a model from an Edge Finder result buffer, the buffer must have been previously allocated using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md). [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have been already called. In addition, the Edge Finder result buffer must be compatible with the AGM context. Enable compatibility using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_AGM_COMPATIBLE`](../../Reference/edge/MedgeControl.md) prior to calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).  The model is defined to have all currently included edges in the result buffer.  In the case of an Edge Finder-type model, the model image is the image from which the Edge Finder results were obtained.

| Value | Description |
| --- | --- |
| `Edge Finder result buffer identifier` | Specifies the identifier of the Edge Finder result buffer. The Edge Finder result buffer must be allocated on the same system as the AGM context. If it is not, an error will occur.  Note that because AGM does not support calibration, the Edge Finder result buffer must be uncalibrated or [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) must be set to [`M_PIXEL`](../../Reference/edge/MedgeControl.md). |

#### `M_IMAGE`

Specifies to define the model from the specified model source image. The entire image is used as the model.  To define the model from a region of the model source image, use [`M_IMAGE_REGION`](../../Reference/agm/MagmDefine.md) instead.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of the image buffer containing the model image. The image buffer must be a 1-band 8-bit unsigned buffer. The minimum and maximum model image sizes are 6x6 and 32768x32768 pixels, respectively.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.  Note that because AGM does not support calibration, the image buffer must be uncalibrated. |

#### `M_IMAGE_REGION`

Specifies to define the model from a region of the specified model source image. Only edgels inside the region will be part of the model. Note that the entire model source image contributes to determining the numerical threshold value used to extract edges from the region.  To extract the model from a rotated region in the model source image, pass a model source image that is associated to a rectangular region of interest (ROI) at the required angle.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of the image buffer from which to extract the model. The image buffer must be a 1-band 8-bit unsigned buffer and must have a (rotated or non-rotated) rectangular region of interest (ROI) associated with it, defined using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with a 2D graphics list and either [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) or [`M_NO_RASTERIZE`](../../Reference/buf/MbufSetRegion.md). An error will be generated if the ROI is only in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md)) or has a non-rectangular shape.  The minimum and maximum sizes are 6x6 and 32768x32768 pixels, respectively, for both the model source image and the ROI. Note that the model's size is determined by the size of the specified region. The center of the region must be inside the model source image.  Note that because AGM does not support calibration, the image buffer must be uncalibrated. |

### For removing a model from the AGM context

To delete a model from an AGM context, the [`ModelType`](../../Reference/agm/MagmDefine.md), [`SourceType`](../../Reference/agm/MagmDefine.md), and [`Param1`](../../Reference/agm/MagmDefine.md) parameters must be set to the following values. The [`Operation`](../../Reference/agm/MagmDefine.md) parameter must be set to [`M_DELETE`](../../Reference/agm/MagmDefine.md).

---

### `M_DEFAULT`

Specifies the default behavior.

#### `M_DEFAULT`

Specifies the default behavior.

| Value | Description |
| --- | --- |
| `M_AGM_MODEL_INDEX` | Specifies the model in the AGM context. |

## Return Value

**Type:** `AIL_INT64`

Reserved for future expansion and returns `M_NULL`.
