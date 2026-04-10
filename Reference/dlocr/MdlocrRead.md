---
doctype: Reference
module: dlocr
function: MdlocrRead
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrRead"
---

# MdlocrRead

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

> Read strings from a target image.

## Syntax

```c
void MdlocrRead(
    AIL_ID    ContextDlocrId,    //in
    AIL_ID    TargetImageBufId,  //in
    AIL_ID    ResultDlocrId,     //out
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function uses a Deep Learning OCR read context to read one or more strings from a target image. Results are stored in a Deep Learning OCR read result buffer. To get the results, use [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md).

By default, [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) reads all text in an image within the default character height range. You can control global read settings using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md). You only need to add a string model if you want to restrict the strings that are found and read. [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md) allows you to define the string model so that attributes are automatically set from a resulting string found using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md). If you only want to restrict the number of characters in the string or define a string model using an anchor, you can use [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md). Regardless of how you add a model, you can always constrain the characters that appear at certain positions using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) and adjust general string model settings using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md).

Requirements for reading strings depend on the settings of the context and if used, string model-specific positional constraints and settings. To control these settings, use [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md), [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md), and [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md). [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) takes into account all settings.

Before performing a read operation, the Deep Learning OCR context must be in a preprocessed state ([`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md)).

To read a string, it must not only meet the context's requirements, but it must also follow the basic rules of a string. It must, for example, be a linear sequence of characters.

Strings are read randomly in an image. A string's position is the position of the center of the bounding box of its first character.

Before preprocessing, you must manually set [`M_TARGET_MAX_SIZE_X`](../../Reference/dlocr/MdlocrControl.md) and [`M_TARGET_MAX_SIZE_Y`](../../Reference/dlocr/MdlocrControl.md), generally to the size of the target image. You can limit the read operation to a region of the target image buffer, using a rectangular ROI set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). To increase performance, set [`M_TARGET_MAX_SIZE_X`](../../Reference/dlocr/MdlocrControl.md) and [`M_TARGET_MAX_SIZE_Y`](../../Reference/dlocr/MdlocrControl.md) to the size of the ROI.

If wanted strings have a known prefix or suffix, set [`M_TEXT_ANCHOR_MODE`](../../Reference/dlocr/MdlocrControlStringModel.md) to [`M_TEXT_PREFIX`](../../Reference/dlocr/MdlocrControlStringModel.md) or [`M_TEXT_SUFFIX`](../../Reference/dlocr/MdlocrControlStringModel.md) respectively within your string model.

If the target image is not calibrated, results are calculated in the pixel coordinate system (pixel units). If the target image is calibrated, results are calculated in the world coordinate system (real-world units), but you can retrieve them in either world or pixel units. To specify this, call [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md).

In the presence of distortion, some results are meaningless when converted from real-world to pixel units (such as angle and scale). For example, if a string appears warped in the target image, but the camera calibration context compensates for this, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system. If complex distortions exist, correct the image before using it with Deep Learning OCR.

The read operation can occasionally take an unexpectedly long time to calculate. Use [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md)with [`M_TIMEOUT`](../../Reference/dlocr/MdlocrControl.md) to set a maximum read time.

To improve the performance of your Deep Learning OCR read context, you can add valid read results to a Deep Learning OCR dataset, using [`MdlocrAddImageToDataset`](../../Reference/dlocr/MdlocrAddImageToDataset.md). You can use this dataset to fine-tune a Deep Learning OCR read context on images from your particular application, using [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md).

## Parameters

### `ContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context to use for the read operation. The context must have been previously allocated on the required system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md), and preprocessed using [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).

### `TargetImageBufId` *(in, AIL_ID)*

Specifies the identifier of the target image buffer which contains the strings to read. The target image buffer must be a 1-band, 8-bit unsigned buffer. Its maximum size is 65536x65536 pixels, and its expeced maximum size should be set using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_TARGET_MAX_SIZE_X`](../../Reference/dlocr/MdlocrControl.md) and [`M_TARGET_MAX_SIZE_Y`](../../Reference/dlocr/MdlocrControl.md).

### `ResultDlocrId` *(out, AIL_ID)*

Specifies the identifier of the Deep Learning OCR result buffer in which to write the results of the read operation. The Deep Learning OCR result buffer must have been previously allocated on the required system using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies the string reading mode.

*For specifying the string matching mode*

| Value | Description |
| --- | --- |
| `M_MODEL_BASED` | Specifies that strings are read using the string models added to the Deep Learning OCR context. Only strings that meet the string model settings and positional constraints are read. |
| `M_READ_ALL` | Specifies to read all strings regardless of the string models added to the Deep Learning OCR context. |
