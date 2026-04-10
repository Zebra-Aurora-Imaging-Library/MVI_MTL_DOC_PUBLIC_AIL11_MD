---
doctype: Reference
module: mod
function: MmodDefineFromFile
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodDefineFromFile"
---

# MmodDefineFromFile

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

> Defines a model from a file and adds it to a Model Finder context.

## Syntax

```c
void MmodDefineFromFile(
    AIL_ID             ContextModId,  //out
    AIL_INT64          FileType,      //in
    AIL_CONST_TEXT_PTR FileName,      //in
    AIL_INT64          ControlFlag    //in
)
```

## Description

This function defines a model from a file and adds it to a Model Finder context. Models defined from a file are CAD-type synthetic models; use [`MmodDefine`](../../Reference/mod/MmodDefine.md) to add other types of models to the Model Finder context. This function does not support an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

Note that the CAD DXF files must be in the R14 file format.

You must preprocess the Model Finder context after you add a model.

By default, synthetic models are defined to have a 10% margin around the bounding box of their active edges. When finding an occurrence, any extra edges found in this area will reduce the target score. You can change the size of the margins with the [`MmodControl`](../../Reference/mod/MmodControl.md)   [`M_BOX_MARGIN_...`](../../Reference/mod/MmodControl.md) control types.

When the model is added to the Model Finder context from a CAD DXF file, the model will retain the CAD-file's coordinate system (the origin and the axis). Specify the ratio between model units and pixel units using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md).

> **Note:** After you have specified your pixel scale and scale settings, you can verify if your synthetic models are valid, using [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_VALID`](../../Reference/mod/MmodInquire.md). Synthetic models can be invalid if their global size in X or Y (_size of the model box _ * [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) * [`M_SCALE`](../../Reference/mod/MmodControl.md)) is less than 16 or greater than 1024.

Note that if the model is used to search in a calibrated target, you must also associate the camera calibration context of the target with the model, using [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md). Aurora Imaging Library needs the camera calibration context for internal purposes at preprocessing time. Aurora Imaging Library will not use the camera calibration context to compensate for incongruencies between the model and the target, nor will Aurora Imaging Library map the model's coordinate system to that of the target.

If the target is calibrated, the model units should be the same as the calibrated units. If the target is calibrated, [`M_PIXEL_SCALE`](../../Reference/mod/MmodControl.md) will be used for draw operations, but not for the match nor the returned results.

Most CAD DXF files are oriented so that the Y-axis is positive going up, whereas the coordinate system Aurora Imaging Library uses is oriented with the Y-axis positive going down. So if you take the edge coordinates of an object from a CAD DXF file and put them in an image, the imaged object will look flipped when compared to the original object. This is also the case for a model defined from a CAD DXF file. This means that, unless the object is symmetrical, the match will not be made. You can use [`M_CAD_Y_AXIS`](../../Reference/mod/MmodControl.md) to set the Y-axis in the appropriate direction.

To delete a model from a Model Finder context, use [`MmodDefine`](../../Reference/mod/MmodDefine.md) with [`M_DELETE`](../../Reference/mod/MmodDefine.md).

## Parameters

### `ContextModId` *(out, AIL_ID)*

Specifies the identifier of the Model Finder context. For this parameter, you can only specify [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder contexts.

### `FileType` *(in, AIL_INT64)*

Specifies the type of file from which to define the model.

*For specifying the type of file*

| Value | Description |
| --- | --- |
| `M_DXF_FILE` | Defines the model from the entities in the specified CAD DXF file. This function only supports version 14 2D CAD DXF files. This function does not support all entities that are possible in a CAD DXF file. If there are unsupported entities in the file, they are ignored, and the rest of the entities are read. The supported entities are as follows: LINE, POLYLINE, LWPOLYLINE, CIRCLE, ARC, ELLIPSE, INSERT, and BLOCK.

Note that for the LWPOLYLINE entities, the bulge option is ignored by Aurora Imaging Library.

A model defined from a CAD DXF file has no polarity. |

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to define the model. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_).

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"`, for example: `"remote:///C:\mydirectory\myfile"`. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. Set this parameter to `M_DEFAULT`.
