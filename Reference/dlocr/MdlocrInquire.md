---
doctype: Reference
module: dlocr
function: MdlocrInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrInquire"
---

# MdlocrInquire

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

> Inquire about a Deep Learning OCR context, result buffer, or dataset setting.

## Syntax

```c
AIL_INT64 MdlocrInquire(
    AIL_ID    DlocrId,      //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function inquires about a setting of a Deep Learning OCR context, result buffer, or dataset. Such settings are typically specified with [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md).

## Parameters

### `DlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context, result buffer, or dataset about which to inquire. The context must have been previously allocated on the system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md). The result buffer must have been previously allocated on the system using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md). The dataset must have been previously allocated on the system using [`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md).

### `InquireType` *(in, AIL_INT64)*

Specifies the setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a Deep Learning OCR context or result buffer

To inquire about the Deep Learning OCR read context, finetuning context, read result buffer, or finetuning result buffer specified with the [`DlocrId`](../../Reference/dlocr/MdlocrInquire.md) parameter, set the [`InquireType`](../../Reference/dlocr/MdlocrInquire.md) parameter to one of the values below.

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context or result buffer are modified.  Although you cannot identify the modification counter's contents, you can compare the counter's value throughout your application to know if the context or result buffer has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the context or result buffer was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For inquiring about a Deep Learning OCR Read or Finetuning Context

To inquire about the Deep Learning OCR read or finetuning context specified with the [`DlocrId`](../../Reference/dlocr/MdlocrInquire.md) parameter, set the [`InquireType`](../../Reference/dlocr/MdlocrInquire.md) parameter to one of the values below.

---

### `M_CONTEXT_TYPE`

Inquires the type of context.

| Value | Description |
| --- | --- |
| `M_OCR_NET1_BALANCED_V1` | Specifies a Deep Learning OCR read context with a balanced deep learning model based on the NET1 architecture. |
| `M_OCR_NET1_EXTENDED_V1` | Specifies a Deep Learning OCR read context with the most robust deep learning model based on the NET1 architecture. |
| `M_OCR_NET1_FAST_V1` | Specifies a Deep Learning OCR read context with the fastest deep learning model based on the NET1 architecture. |
| `M_OCR_NET2_TINY_V1` | Specifies a Deep Learning OCR read context with the tiny deep learning model based on the NET2 architecture. |
| `M_OCR_NET2_FINETUNE_CONTEXT` | Specifies a Deep Learning OCR fine-tuning context. |

---

### `M_PREPROCESSED`

Inquires whether the context is preprocessed.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

### For inquiring about a Deep Learning OCR Read Context

To inquire about the Deep Learning OCR read context specified with the [`DlocrId`](../../Reference/dlocr/MdlocrInquire.md) parameter, set the [`InquireType`](../../Reference/dlocr/MdlocrInquire.md) parameter to one of the values below.

---

### `M_ACCEPTED_CHARS`

Inquires the list of accepted characters. The characters in this list should be included in [`M_SUPPORTED_CHARS`](../../Reference/dlocr/MdlocrInquire.md).

| Value | Description |
| --- | --- |
| `"Accepted characters"` | Specifies the list of accepted characters. |

---

### `M_CHAR_ACCEPTANCE`

Inquires he minimum score that a matched character must have to be considered valid.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the minimum score. |

---

### `M_CHAR_MAX_ASPECT_RATIO`

Inquires the maximum aspect ratio of an uppercase character.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the aspect ratio (width/height). |

---

### `M_DETECTION_ANGLE`

Inquires the angle by which the image must be internally rotated to be able to read strings.

| Value | Description |
| --- | --- |
| `M_ACCORDING_TO_REGION` | Specifies to use the angle of the region of interest. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_DETECTION_CHAR_HEIGHT_MAX`

Inquires the maximum height of an uppercase character that can be read.

| Value | Description |
| --- | --- |
| `Value >= M_DETECTION_CHAR_HEIGHT_MIN` *(default)* | Specifies the maximum character height, in pixels. |

---

### `M_DETECTION_CHAR_HEIGHT_MIN`

Inquires the minimum height of an uppercase character that can be read.

| Value | Description |
| --- | --- |
| `6.0 <= Value <= M_DETECTION_CHAR_HEIGHT_MAX` *(default)* | Specifies the minimum character height, in pixels. |

---

### `M_INTERCHAR_MAX_WIDTH`

Inquires the maximum gap size between the last character of a string and the first character of another. Gaps larger than this will be read as separate strings.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the maximum character gap, in pixels. |

---

### `M_NUMBER_OF_STRING_MODELS`

Inquires the number of string models in the context. To add and remove string models, use [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md) or [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of string models. |

---

### `M_PRESEARCH_ANGLE_RANGE`

Inquires the range of angles at which to presearch. Note, this setting only has an impact if [`M_PRESEARCH_STRATEGY`](../../Reference/dlocr/MdlocrInquire.md) is set to [`M_PRESEARCH_NET1_V1`](../../Reference/dlocr/MdlocrInquire.md).

| Value | Description |
| --- | --- |
| `M_NARROW_RANGE` *(default)* | Specifies to use a narrow range of [-90,90] degrees around the detection angle. |
| `M_WIDE_RANGE` | Specifies to use a wide range of [-180,180] degrees around the detection angle. |

---

### `M_PRESEARCH_STRATEGY`

Inquires which presearch algorithm to use before other [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) operations.

| Value | Description |
| --- | --- |
| `M_NO_PRESEARCH` *(default)* | Specifies to not use the AI presearch strategy. |
| `M_PRESEARCH_NET1_V1` | Specifies to use the AI presearch strategy. |

---

### `M_STRING_NUMBER`

Inquires the maximum total number of strings to read in the image.

| Value | Description |
| --- | --- |
| `M_INFINITE` *(default)* | Specifies that there is no limit. |
| `Value > 0` | Specifies the number of strings to read in an image. |

---

### `M_SUPPORTED_CHARS`

Inquires the list of supported characters. This is the list of characters on which the classifier was trained.

| Value | Description |
| --- | --- |
| `"Supported characters"` | Specifies the list of supported characters. |

---

### `M_TARGET_MAX_SIZE_X`

Inquires the maximum width of a target image buffer used for Deep Leaning OCR functions. When using ROIs, this can be set to the width of the ROI to optimize the operation.

| Value | Description |
| --- | --- |
| `M_INVALID` *(default)* | Specifies that the width is invalid. |
| `Value > 0` | Specifies the maximum width of the buffer, in pixels. |

---

### `M_TARGET_MAX_SIZE_Y`

Inquires the maximum height of a target image buffer used for Deep Learning OCR functions. When using ROIs, this can be set to the height of the ROI to optimize the operation.

| Value | Description |
| --- | --- |
| `M_INVALID` *(default)* | Specifies that the height is invalid. |
| `Value > 0` | Specifies the maximum height of the buffer, in pixels. |

---

### `M_TIMEOUT`

Inquires the maximum read time.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies an infinite amount of read time. |
| `Value > 0.0` *(default)* | Specifies the maximum read time, in msec. |

### For inquiring about the predict engine

To inquire about the predict engine of the Deep Learning OCR context specified with the [`DlocrId`](../../Reference/dlocr/MdlocrInquire.md) parameter, set the [`InquireType`](../../Reference/dlocr/MdlocrInquire.md) parameter to one of the following values.

---

### `M_NUMBER_OF_PREDICT_ENGINES`

Inquires the number of predict engines detected. You can inquire the specifics of each predict engine using [`M_PREDICT_ENGINE_PROVIDER`](../../Reference/class/MclassInquire.md), [`M_PREDICT_ENGINE_DESCRIPTION`](../../Reference/class/MclassInquire.md) and [`M_PREDICT_ENGINE_PRECISION`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of predict engines. |

---

### `M_PREDICT_ENGINE_DESCRIPTION`

Inquires the description of the predict engine being used. You can inquire the predict engine being used with [`M_PREDICT_ENGINE_USED`](../../Reference/dlocr/MdlocrInquire.md).

| Value | Description |
| --- | --- |
| `"Description"` | Specifies the text description of the engine type seen by the execution provider. |

---

### `M_PREDICT_ENGINE_PRECISION`

Inquires the precision of the predict engine being used. You can inquire the predict engine being used with [`M_PREDICT_ENGINE_USED`](../../Reference/dlocr/MdlocrInquire.md).

| Value | Description |
| --- | --- |
| `M_FP16` | Specifies that the precision used by the predict engine is floating-point 16-bit. |
| `M_FP32` | Specifies that the precision used by the predict engine is floating-point 32-bit. |

---

### `M_PREDICT_ENGINE_PROVIDER`

Inquires the provider of the predict engine being used. You can inquire the predict engine being used with [`M_PREDICT_ENGINE_USED`](../../Reference/dlocr/MdlocrInquire.md).  You must have installed the appropriate [Aurora Imaging Library add-ons and updates](../../UserGuide/C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md) to perform GPU accelerated prediction using CUDA or OpenVINO.

| Value | Description |
| --- | --- |
| `M_CUDA` | Specifies that the predict engine provider is CUDA. |
| `M_DEFAULT_CPU` | Specifies the default predict engine provider. |
| `M_OPENVINO` | Specifies that the predict engine provider is OpenVINO. |

---

### `M_PREDICT_ENGINE_USED`

Inquires which predict engine is used by the specified context. This value should match the value set using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_PREDICT_ENGINE`](../../Reference/class/MclassControl.md) unless the order of the predict engines is modified. This can happen if the environment running Aurora Imaging Library is modified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the predict engine being used is the predict engine that was selected in the **Aurora Imaging Configurator** utility. |
| `M_INVALID` | Specifies that the predict engine is not found. |
| `0 <= Value < M_NUMBER_OF_PREDICT_ENGINES` | Specifies the index of the predict engine used. |

### For inquiring about a Deep Learning OCR fine-tuning context

To inquire about the Deep Learning OCR fine-tuning context specified with the [`DlocrId`](../../Reference/dlocr/MdlocrInquire.md) parameter, set the [`InquireType`](../../Reference/dlocr/MdlocrInquire.md) parameter to one of the values below.

---

### `M_FINETUNING_LEVEL`

Inquires the fine-tuning level. The fine-tuning level determines how much the fine-tuning context will adapt to the dataset.

| Value | Description |
| --- | --- |
| `M_HIGH` | Specifies a high fine-tuning level. |
| `M_LOW` *(default)* | Specifies a low fine-tuning level. |
| `M_MEDIUM` | Specifies a medium fine-tuning level. |
| `M_VERY_HIGH` | Specifies a very high fine-tuning level. |
| `M_VERY_LOW` | Specifies a very low fine-tuning level. |

---

### `M_MINI_BATCH_SIZE`

Inquires the number of images in the fine-tuning dataset that can be processed at the same time. Note that higher values will use more memory and lower values will increase the speed of fine-tuning.

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the number of images in the mini-batch. |

### For inquiring about a Deep learning OCR read result buffer

To inquire about the Deep Learning OCR read result buffer specified with the [`DlocrId`](../../Reference/dlocr/MdlocrInquire.md) parameter, set the [`InquireType`](../../Reference/dlocr/MdlocrInquire.md) parameter to one of the values below.

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether to return results in pixels or world units.

| Value | Description |
| --- | --- |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

### For inquiring about a Deep learning OCR dataset

To inquire about the Deep Learning OCR dataset specified with the [`DlocrId`](../../Reference/dlocr/MdlocrInquire.md) parameter, set the [`InquireType`](../../Reference/dlocr/MdlocrInquire.md) parameter to one of the values below.

---

### `M_NUMBER_OF_ENTRIES`

Inquires the number of entries in a dataset. An entry contains an image and its associated results.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of entries in the dataset. |

### Combination Constants — For inquiring the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character.

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring if an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported for the Deep Learning OCR context currently being inquired.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

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

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
