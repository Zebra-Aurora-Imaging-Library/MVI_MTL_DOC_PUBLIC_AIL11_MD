---
doctype: Reference
module: agm
function: MagmTrain
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmTrain"
---

# MagmTrain

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

> Train a composite-definition model that is in a train AGM context.

## Syntax

```c
void MagmTrain(
    AIL_ID         TrainContextId,  //in
    const AIL_ID * ImageArrayPtr,   //in
    AIL_INT        NumImages,       //in
    AIL_ID         TrainResultId,   //out
    AIL_INT64      ControlFlag      //in
)
```

## Description

This function trains a composite-definition model that is in a train AGM context and stores the result in a train AGM result buffer. The trained composite-definition model is constructed from training images with regions labeled as positive or negative. During training, the model learns what is a favorable feature of an occurrence and what is not. Retrieve results about the training operation using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md). Note, the train AGM context must be preprocessed (using [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md)) before calling this function. Once trained, you copy the resulting trained composite-definition model to a find AGM context using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md).

The training process involves using model appearances and background samples to construct a model that is good at finding your model, especially in cluttered target images. These areas must be labeled in the training images using regions of interest (ROIs) containing blue/red rectangles. Blue rectangles define positive model locations, while red rectangles define negative model locations. Negative model locations include clutter in the background and areas in an image that resemble your model but aren't.

You should use blue rectangles to specify appearances of the model that you want to find. [`MagmTrain`](../../Reference/agm/MagmTrain.md) can only train one model; therefore, each blue rectangle must identify an occurrence of the same model. Note that model appearances don't need to be in the same location in the [`MagmFind`](../../Reference/agm/MagmFind.md) target images.

If you enabled the automatic labeling of negative model locations ([`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) set to [`M_AUTO`](../../Reference/agm/MagmControl.md)), you must only label the positive model appearances; [`MagmTrain`](../../Reference/agm/MagmTrain.md) will automatically identify and label the background areas of your training images. In this case, a minimum of one training image must have a blue ROI associated with it. The ROI can be composed of several non-contiguous and/or overlapping areas, but it must contain at least one blue rectangle and cannot contain any red rectangles. Note that there can be training images with no ROI (no rectangles).

If you did not enable the automatic labeling of negative model locations, you must manually label both the model appearances and background samples before calling [`MagmTrain`](../../Reference/agm/MagmTrain.md). In this case, the set of training images must contain red and blue rectangles, associated to the images using ROIs. You should use red rectangles to specify clutter in the background. If there is an area in an image that resembles your model but isn't, you can use red rectangles to prevent [`MagmFind`](../../Reference/agm/MagmFind.md) from returning that area as a possible occurrence. A minimum of one training image must have an ROI associated with it. The ROI can be composed of several non-contiguous and/or overlapping areas, but it must contain at least one red rectangle and/or one blue rectangle. Considering all the rectangles across all training images, there must be a minimum of one red rectangle and one blue rectangle. Note that there can be training images with only red rectangles, only blue rectangles, or no rectangles. The training images with no rectangles are not used during training.

Note that all rectangles must be the same size.

You can use the _AgmLabelingTool_ example, accessible from the Example Launcher, to define images with appropriate regions of interest.

Use [`MagmControl`](../../Reference/agm/MagmControl.md) to configure individual model settings before calling this function. For example, you can set [`M_EMPTY_REGIONS_MODE`](../../Reference/agm/MagmControl.md) to [`M_AUTO`](../../Reference/agm/MagmControl.md) to take into consideration, when training the model, the regions of the model appearances that do not contain edges.

You must call this function before calling [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md). You cannot copy the model to a find AGM context unless it is successfully trained. To check the training status of the model, use [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_STATUS`](../../Reference/agm/MagmGetResult.md).

## Parameters

### `TrainContextId` *(in, AIL_ID)*

Specifies the identifier of the train AGM context with which to train the composite-definition model. The train AGM context must have been previously allocated on the required system using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_TRAIN`](../../Reference/agm/MagmAlloc.md). It must also contain a composite-definition model, added using [`MagmDefine`](../../Reference/agm/MagmDefine.md) with [`M_COMPOSITE`](../../Reference/agm/MagmDefine.md).

### `ImageArrayPtr` *(in, *AIL_ID)*

Specifies either the address of the array containing the buffer identifiers of the training images or specifies the address of the variable containing the identifier of the image buffer container. The image buffers must be 1-band, 8-bit unsigned buffers. The minimum and maximum training image sizes are 6x6 and 32768x32768 pixels, respectively.

### `NumImages` *(in, AIL_INT)*

Specifies the size of the specified image buffer array. When an image buffer container is specified, you must set [`NumImages`](../../Reference/agm/MagmTrain.md) to 1.

*For specifying the number of training images*

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the size of the array passed to [`ImageArrayPtr`](../../Reference/agm/MagmTrain.md). |

### `TrainResultId` *(out, AIL_ID)*

Specifies the train AGM result buffer identifier. The result buffer must have been allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
