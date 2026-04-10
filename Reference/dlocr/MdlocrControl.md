---
doctype: Reference
module: dlocr
function: MdlocrControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrControl"
---

# MdlocrControl

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

> Control a Deep Learning OCR context, result buffer, or dataset.

## Syntax

```c
void MdlocrControl(
    AIL_ID     ContextOrResultDlocrId,  //out
    AIL_INT64  ControlType,             //in
    AIL_DOUBLE ControlValue             //in
)
```

## Description

This function allows you to control a Deep Learning OCR context, a Deep Learning OCR result buffer, or a Deep Learning OCR dataset. To inquire about settings in a context, result buffer, or a dataset, call [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md). To get results from a result buffer, call [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md).

To control the string model-specific settings and positional constraints held in a Deep Learning OCR read context, call [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) and [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md), respectively.

You must preprocess the Deep Learning OCR context after you have finished adjusting context settings and before calling [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) or [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md). To know if a context needs to be preprocessed, call [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_PREPROCESSED`](../../Reference/dlocr/MdlocrInquire.md).

## Parameters

### `ContextOrResultDlocrId` *(out, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context, result buffer, or dataset to control. The context must have been previously allocated on the system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md). The result buffer must have been previously allocated on the system using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md). The dataset must have been previously allocated on the system using [`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md).

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### For controlling a Deep Learning OCR read context setting

The following [`ControlType`](../../Reference/dlocr/MdlocrControl.md) and corresponding [`ControlValue`](../../Reference/dlocr/MdlocrControl.md) parameter settings are used to control a setting of the Deep Learning OCR read context specified with the [`ContextOrResultDlocrId`](../../Reference/dlocr/MdlocrControl.md) parameter.

---

### `M_ACCEPTED_CHARS`

Sets the list of accepted characters. The characters in this list should be among those supported by the context type; use [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_SUPPORTED_CHARS`](../../Reference/dlocr/MdlocrInquire.md) to establish the list of supported characters.  > **Note:** Note that calls to [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md) are not accumulative. To get the list of characters in a predefined list, set [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md) to the required predefined list, and then inquire what characters this corresponds to using [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with[`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT_ACCEPTED_CHARS` *(default)* | Specifies the list of all letters (upper and lowercase) and digits. |
| `M_DIGITS_CHARS` | Specifies the list of all numeric digits. |
| `M_LETTERS_CHARS` | Specifies the list of all upper and lowercase letters. |
| `M_LETTERS_LOWERCASE_CHARS` | Specifies the list of all lowercase letters. |
| `M_LETTERS_UPPERCASE_CHARS` | Specifies the list of all uppercase letters. |
| `"Accepted characters"` | Specifies the list of accepted characters. |

---

### `M_CHAR_ACCEPTANCE`

Sets the acceptance level for the character score. To be accepted as a read result, the character's score must be greater than or equal to this value.  The character's score quantifies the confidence that the Deep Learning OCR classifier has correctly predicted the character. Character scores can be retrieved, using [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md) with [`M_CHAR_SCORE`](../../Reference/dlocr/MdlocrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the minimum score. |

---

### `M_CHAR_MAX_ASPECT_RATIO`

Sets the expected aspect ratio of characters to be read.  > **Note:** This should be set when working with very wide or very narrow fonts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the aspect ratio (width/height). |

---

### `M_DETECTION_ANGLE`

Sets the angle by which the image must be internally rotated to be able to read strings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_REGION` | Specifies to use the angle of the region of interest. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. The angle should be specified in the counterclockwise direction. |

---

### `M_DETECTION_CHAR_HEIGHT_MAX`

Sets the maximum height of an uppercase character that can be read.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= M_DETECTION_CHAR_HEIGHT_MIN` *(default)* | Specifies the maximum character height, in pixels. |

---

### `M_DETECTION_CHAR_HEIGHT_MIN`

Sets the minimum height of an uppercase character that can be read. Note, setting this to a low value negatively impacts performance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `6.0 <= Value <= M_DETECTION_CHAR_HEIGHT_MAX` *(default)* | Specifies the minimum character height, in pixels. |

---

### `M_INTERCHAR_MAX_WIDTH`

Sets the maximum gap size between the last character of a string and the first character of another. Gaps larger than this will be read as separate strings. Adjusting this setting allows you to split and merge strings on the same line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the maximum character gap, in pixels. |

---

### `M_PRESEARCH_ANGLE_RANGE`

Sets the range of angles at which to presearch. Note, this setting only has an impact if [`M_PRESEARCH_STRATEGY`](../../Reference/dlocr/MdlocrControl.md) is set to [`M_PRESEARCH_NET1_V1`](../../Reference/dlocr/MdlocrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NARROW_RANGE` *(default)* | Specifies to use a narrow range of [-90,90] degrees around the detection angle. |
| `M_WIDE_RANGE` | Specifies to use a wide range of [-180,180] degrees around the detection angle. This can decrease the speed of read operations. |

---

### `M_PRESEARCH_STRATEGY`

Sets which presearch algorithm to use before other [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NO_PRESEARCH` *(default)* | Specifies to not use the AI presearch strategy. |
| `M_PRESEARCH_NET1_V1` | Specifies to use the AI presearch strategy. The AI presearch is designed to accelerate calls to [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) in large images containing mostly 'background' and few characters. The AI presearch also allows text to be detected at a wider angle than what would typically be possible with without the presearch. |

---

### `M_STRING_NUMBER`

Sets the total number of strings to read in the image. For values other than [`M_INFINITE`](../../Reference/dlocr/MdlocrControl.md), using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) with [`M_MODEL_BASED`](../../Reference/dlocr/MdlocrRead.md) will attempt to read exactly that number of strings while respecting [`M_MIN_NUMBER_OF_OCCURRENCES`](../../Reference/dlocr/MdlocrControlStringModel.md)and [`M_MAX_NUMBER_OF_OCCURRENCES`](../../Reference/dlocr/MdlocrControlStringModel.md) of each string model. If [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md)does not read enough strings, nothing will be read. Once the number of string occurrences for the context has been reached, the read operation will stop, regardless of whether the actual number set for any individual model has been found.  > **Note:** Note, this setting is ignored when performing an [`M_READ_ALL`](../../Reference/dlocr/MdlocrRead.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no limit. |
| `Value > 0` | Specifies the number of strings to read in an image. |

---

### `M_TARGET_MAX_SIZE_X`

Sets the maximum width of a target image buffer used for a Deep Learning OCR function. Note that when working with an image buffer with an ROI, [`M_TARGET_MAX_SIZE_X`](../../Reference/dlocr/MdlocrControl.md) refers to the width of the ROI rather than the width of the image.  Note, setting this much larger than the target image can reduce performance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INVALID` *(default)* | Specifies that the width is invalid. An error is generated if a Deep Learning OCR function is called on a context with [`M_TARGET_MAX_SIZE_X`](../../Reference/dlocr/MdlocrControl.md)set to [`M_INVALID`](../../Reference/dlocr/MdlocrControl.md). |
| `Value > 0` | Specifies the maximum width of the buffer, in pixels. |

---

### `M_TARGET_MAX_SIZE_Y`

Sets the maximum height of a target image buffer used for a Deep Learning OCR function. Note that when working with an image buffer with an ROI, [`M_TARGET_MAX_SIZE_Y`](../../Reference/dlocr/MdlocrControl.md) refers to the height of the ROI rather than the height of the image.  Note, setting this much larger than the target image can reduce performance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INVALID` *(default)* | Specifies that the height is invalid. An error is generated if a Deep Learning OCR function is called on a context with [`M_TARGET_MAX_SIZE_Y`](../../Reference/dlocr/MdlocrControl.md)set to [`M_INVALID`](../../Reference/dlocr/MdlocrControl.md). |
| `Value > 0` | Specifies the maximum height of the buffer, in pixels. |

---

### `M_TIMEOUT`

Sets the maximum read time. No results are returned if the read operation times out.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies an infinite amount of read time. |
| `Value > 0.0` *(default)* | Specifies the maximum read time, in msec. |

### For controlling the predict engine

The following [`ControlType`](../../Reference/dlocr/MdlocrControl.md) and corresponding [`ControlValue`](../../Reference/dlocr/MdlocrControl.md) parameter settings are used to control the predict engine.

---

### `M_PREDICT_ENGINE`

Sets the predict engine to use. You can get predict engine information by calling[`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_PREDICT_ENGINE_...`](../../Reference/dlocr/MdlocrInquire.md).  You must have installed the appropriate [Aurora Imaging Library add-ons and updates](../../UserGuide/C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md) to perform GPU accelerated prediction using CUDA or OpenVINO.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default predict engine, set using the **Aurora Imaging Configurator** utility. |
| `0 <= Value < M_NUMBER_OF_PREDICT_ENGINES` | Specifies the index of the predict engine to use. |

### For controlling a Deep Learning OCR read result buffer

The following [`ControlType`](../../Reference/dlocr/MdlocrControl.md) and corresponding [`ControlValue`](../../Reference/dlocr/MdlocrControl.md) parameter settings are used to control the Deep Learning OCR read result buffer specified with the [`ContextOrResultDlocrId`](../../Reference/dlocr/MdlocrControl.md) parameter.

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md) generates an error if the result was not calculated on a calibrated image. |

---

### `M_STOP_READ`

Stops the current read operation. You can only perform this operation from another thread of higher priority. No results are returned if you stop the read operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

### For controlling a Deep Learning OCR fine-tuning context

The following [`ControlType`](../../Reference/dlocr/MdlocrControl.md) and corresponding [`ControlValue`](../../Reference/dlocr/MdlocrControl.md) parameter settings are used to control the Deep Learning OCR fine-tuning context specified with the [`ContextOrResultDlocrId`](../../Reference/dlocr/MdlocrControl.md) parameter.

---

### `M_FINETUNING_LEVEL`

Sets the fine-tuning level. The fine-tuning level determines how much the fine-tuning context will adapt to the dataset.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
| `M_HIGH` | Specifies a high fine-tuning level. |
| `M_LOW` *(default)* | Specifies a low fine-tuning level. |
| `M_MEDIUM` | Specifies a medium fine-tuning level. |
| `M_VERY_HIGH` | Specifies a very high fine-tuning level. |
| `M_VERY_LOW` | Specifies a very low fine-tuning level. |

---

### `M_MINI_BATCH_SIZE`

Sets the number of images in the fine-tuning dataset that can be processed at the same time. Note that higher values will use more memory and lower values will increase the speed of fine-tuning.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of images in the mini-batch. |

### For controlling a Deep Learning OCR fine-tuning result buffer

The following [`ControlType`](../../Reference/dlocr/MdlocrControl.md) and corresponding [`ControlValue`](../../Reference/dlocr/MdlocrControl.md) parameter settings are used to control the Deep Learning OCR fine-tuning result buffer specified with the [`ContextOrResultDlocrId`](../../Reference/dlocr/MdlocrControl.md) parameter.

---

### `M_STOP_FINETUNING`

Stops the current fine-tuning operation. You can only perform this operation from another thread of higher priority. No results are returned if you stop the fine-tuning operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

### For controlling a Deep Learning OCR read result buffer, fine-tuning result buffer, or dataset

The following [`ControlType`](../../Reference/dlocr/MdlocrControl.md) and corresponding [`ControlValue`](../../Reference/dlocr/MdlocrControl.md) parameter setting is used to control the Deep Learning OCR read result buffer, fine-tuning result buffer, or dataset specified with the [`ContextOrResultDlocrId`](../../Reference/dlocr/MdlocrControl.md) parameter.

---

### `M_RESET`

Removes all images and results from the Deep Learning OCR read result buffer, fine-tuning result buffer, or dataset. This does not delete the object's identifier, as is the case with [`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
