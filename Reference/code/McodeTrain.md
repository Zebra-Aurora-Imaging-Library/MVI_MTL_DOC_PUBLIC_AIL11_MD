---
doctype: Reference
module: code
function: McodeTrain
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeTrain"
---

# McodeTrain

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

> Train the control type settings of a code context and its code models so that they are optimized for the specified set of sample images.

## Syntax

```c
void McodeTrain(
    AIL_ID         ContextCodeId,     //in
    AIL_INT        NumImages,         //in
    const AIL_ID * ImageArrayPtr,     //in
    AIL_INT64      ControlFlag,       //in
    AIL_ID         TrainResultCodeId  //out
)
```

## Description

This function trains the settings of the selected control types of a code context and its code models so that the settings are optimized for performing an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation on the specified set of sample images. The function saves the results in the specified code train result buffer; the specified code context is not directly modified.

Activate the required control types for training using [`McodeControl`](../../Reference/code/McodeControl.md) with the combination constant [`M_TRAIN`](../../Reference/code/McodeControl.md) or with [`M_SET_TRAINING_STATE_ALL`](../../Reference/code/McodeControl.md). The following control types are supported for training:

- [`M_CELL_NUMBER_...`](../../Reference/code/McodeControl.md).
- [`M_CELL_SIZE_...`](../../Reference/code/McodeControl.md).
- [`M_CODE_FLIP`](../../Reference/code/McodeControl.md).
- [`M_DATAMATRIX_SHAPE`](../../Reference/code/McodeControl.md).
- [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md).
- [`M_DOT_SIZE_...`](../../Reference/code/McodeControl.md).
- [`M_DOT_SPACING_...`](../../Reference/code/McodeControl.md).
- [`M_ENCODING`](../../Reference/code/McodeControl.md).
- [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md).
- [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md).
- [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md).
- [`M_NUMBER`](../../Reference/code/McodeControl.md).
- [`M_SEARCH_ANGLE...`](../../Reference/code/McodeControl.md).
- [`M_SPEED`](../../Reference/code/McodeControl.md).
- [`M_THRESHOLD_...`](../../Reference/code/McodeControl.md).
- [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md).

You can limit the code occurrences used for training to those within specific regions of the training images. If the training images have a rectangular region of interest (ROI), defined using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), [`McodeTrain`](../../Reference/code/McodeTrain.md) only uses occurrences within those regions to establish the recommended settings. The regions must be defined in vector format from a 2D graphics list ([`M_VECTOR`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error will be generated if any of the images have an ROI that is only in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md)) or is a non-rectangular shape.

This function writes the recommended settings of the activated control types to the specified code train result buffer. You can reconfigure any code context with these results using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESET_FROM_TRAINED_RESULTS`](../../Reference/code/McodeControl.md). This control type discards any existing code models from the context, and then adds the code models used for training to the context. If a control type was not trained, it is set to its value used during training; if a control type was trained, it is set to its trained value. Prior to using [`M_RESET_FROM_TRAINED_RESULTS`](../../Reference/code/McodeControl.md), you should ensure that the train operation was successful using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STATUS`](../../Reference/code/McodeGetResult.md). If required, you can retrieve the recommended setting for a specific control type using [`McodeGetResult`](../../Reference/code/McodeGetResult.md). This is especially useful if you want to reconfigure the code context that was used during training. In this case, you can use the retrieved values to selectively change the control types of the context to their trained value. Note that some control types are inter-dependent; changing the setting of one control type to its trained value might not be optimal if the settings of other control types are not also changed. This is especially true for control types that are automatically activated for training if another control type is activated (for example, [`M_CELL_NUMBER_...`](../../Reference/code/McodeControl.md)).

You can retrieve or draw the results of the read operation that [`McodeTrain`](../../Reference/code/McodeTrain.md) internally performed on each training image. You can use this extra information to complement the status result. First, call [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_CODE_RESULT_ID`](../../Reference/code/McodeGetResult.md). This returns the identifiers of the internal code read result buffers; there is one per training image. You can then pass one of these result buffer identifiers to [`McodeGetResult`](../../Reference/code/McodeGetResult.md), and retrieve any type of result available for an[`McodeRead`](../../Reference/code/McodeRead.md) operation. You can also pass one of the result buffer identifiers to [`McodeDraw`](../../Reference/code/McodeDraw.md) to visualize the results for a specific training image.

The recommended control type settings for a code context and its code models are based on the code occurrences found in all the training images. [`McodeTrain`](../../Reference/code/McodeTrain.md) must find at least one occurrence of one of the code models among all the sample images; otherwise, [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with[`M_STATUS`](../../Reference/code/McodeGetResult.md) returns [`M_STATUS_TRAIN_FAILED`](../../Reference/code/McodeGetResult.md). You can retrieve the buffer identifiers of images in which no code occurrence was found using [`M_FAILED_IMAGES_ID`](../../Reference/code/McodeGetResult.md).

## Parameters

### `ContextCodeId` *(in, AIL_ID)*

Specifies the identifier of the code context to train.

### `NumImages` *(in, AIL_INT)*

Specifies the number of training images passed to [`ImageArrayPtr`](../../Reference/code/McodeTrain.md).

*For specifying the number of training images*

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of images passed to [`ImageArrayPtr`](../../Reference/code/McodeTrain.md). |

### `ImageArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the buffer identifiers of the training images. The image buffers must be 1-band, 8-bit unsigned buffers.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `TrainResultCodeId` *(out, AIL_ID)*

Specifies the train result buffer identifier. The result buffer must have been allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_TRAIN_RESULT`](../../Reference/code/McodeAllocResult.md).
