---
doctype: Reference
module: dmr
function: MdmrInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrInquire"
---

# MdmrInquire

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

> Inquire about a SureDotOCR context or result buffer setting.

## Syntax

```c
AIL_INT MdmrInquire(
    AIL_ID    ContextOrResultDmrId,  //in
    AIL_INT64 InquireType,           //in
    void *    UserVarPtr             //out
)
```

## Description

This function inquires about a setting of a SureDotOCR context or result buffer. Such settings are typically specified with [`MdmrControl`](../../Reference/dmr/MdmrControl.md).

If the inquired setting is set to [`M_DEFAULT`](../../Reference/dmr/MdmrControl.md) (for example, in [`MdmrControl`](../../Reference/dmr/MdmrControl.md)), [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) will return [`M_DEFAULT`](../../Reference/dmr/MdmrInquire.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/dmr/MdmrInquire.md) to the [`InquireType`](../../Reference/dmr/MdmrInquire.md) parameter.

## Parameters

### `ContextOrResultDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context or result buffer about which to inquire. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md). The result buffer must have been previously allocated on the system using [`MdmrAllocResult`](../../Reference/dmr/MdmrAllocResult.md).

### `InquireType` *(in, AIL_INT64)*

Specifies the setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a SureDotOCR context

To inquire about the SureDotOCR context specified with the [`ContextOrResultDmrId`](../../Reference/dmr/MdmrInquire.md) parameter, set the [`InquireType`](../../Reference/dmr/MdmrInquire.md) parameter to one of the values below. SureDotOCR can only read strings that adhere to these settings (they apply to all string models in the context).

---

### `M_DOT_DIAMETER`

Inquires the diameter of the dots that make up the characters in the strings.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the diameter, in pixels. |
| `M_INVALID` | Specifies an invalid diameter. Set the diameter to a valid value, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md). |

---

### `M_DOT_DIAMETER_SPREAD`

Inquires the range of possible dot diameter deviations, in pixels, centered at the[`M_DOT_DIAMETER`](../../Reference/dmr/MdmrInquire.md). In other words, the maximum allowed dot diameter deviation over and below the specified dot diameter is half this value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the spread, in pixels. |

---

### `M_DOT_DIAMETER_SPREAD_MODE`

Inquires whether the range of possible dot diameter deviations is enabled, or not.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that range of dot diameter deviations is disabled. |
| `M_ENABLE` | Specifies that range of dot diameter deviations is enabled. |

---

### `M_DOT_DIAMETER_STEP`

Inquires the step that will be used inside the range of dot diameter deviation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the step, in pixels. |

---

### `M_FOREGROUND_VALUE`

Inquires the foreground value of the characters in the strings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FOREGROUND_BLACK` *(default)* | Specifies that strings with characters darker than the background will be read. |
| `M_FOREGROUND_WHITE` | Specifies that strings with characters lighter than the background will be read. |

---

### `M_ITALIC_ANGLE`

Inquires the italic angle at which to read the characters in the strings. [`M_ITALIC_ANGLE`](../../Reference/dmr/MdmrInquire.md)only has an effect if [`M_ITALIC_ANGLE_MODE`](../../Reference/dmr/MdmrInquire.md)is set to [`M_ANGLE`](../../Reference/dmr/MdmrInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-90.0 <= Value <= 90.0` *(default)* | Specifies the italic angle, in degrees. |

---

### `M_ITALIC_ANGLE_MODE`

Inquires whether the italic angle is detected automatically or explicitly specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrInquire.md). |
| `M_ANGLE` | Specifies to use the italic angle specified with [`M_ITALIC_ANGLE`](../../Reference/dmr/MdmrInquire.md). |
| `M_AUTO` | Specifies that the italic angle is detected automatically. |

---

### `M_ITALIC_PITCH`

Inquires the distance between successive dot centers, in the italic angle direction, at which to read the characters in the strings. [`M_ITALIC_PITCH`](../../Reference/dmr/MdmrInquire.md)only has an effect if [`M_ITALIC_PITCH_MODE`](../../Reference/dmr/MdmrInquire.md)is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the pitch, in pixels. |

---

### `M_ITALIC_PITCH_MODE`

Inquires whether the distance between successive dot centers, in the italic angle direction, is detected automatically or explicitly specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrInquire.md). |
| `M_AUTO` | Specifies to automatically establish the distance between dot centers in the italic angle direction. |
| `M_USER_DEFINED` | Specifies to use the value set with [`M_ITALIC_PITCH`](../../Reference/dmr/MdmrInquire.md). |

---

### `M_MAX_INTENSITY`

Inquires the maximum pixel intensity of a character's dots.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 255.0` *(default)* | Specifies the maximum pixel intensity. |

---

### `M_MAX_INTENSITY_MODE`

Inquires how to establish the maximum pixel intensity of a character's dots.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the maximum pixel intensity of a character's dots. |
| `M_USER_DEFINED` | Specifies to use the maximum pixel intensity value set with [`M_MAX_INTENSITY`](../../Reference/dmr/MdmrInquire.md). |

---

### `M_MIN_CONTRAST`

Inquires the minimum contrast between a character and its background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 255.0` *(default)* | Specifies the minimum contrast. |

---

### `M_MIN_CONTRAST_MODE`

Inquires how to establish the minimum contrast between a character and its background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the minimum contrast of characters. |
| `M_USER_DEFINED` | Specifies to use the minimum contrast value set with [`M_MIN_CONTRAST`](../../Reference/dmr/MdmrInquire.md). |

---

### `M_MIN_INTENSITY`

Inquires the minimum pixel intensity of a character's dots.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 255.0` *(default)* | Specifies the minimum pixel intensity. |

---

### `M_MIN_INTENSITY_MODE`

Inquires how to establish the minimum pixel intensity of a character's dots.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the minimum pixel intensity of character dots. |
| `M_USER_DEFINED` | Specifies to use the minimum pixel intensity value set with [`M_MIN_INTENSITY`](../../Reference/dmr/MdmrInquire.md). |

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_NUMBER_OF_FONTS`

Inquires the number of fonts in the context. To add and remove fonts, use [`MdmrControl`](../../Reference/dmr/MdmrControl.md). To import fonts, use [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of fonts. |

---

### `M_NUMBER_OF_STRING_MODELS`

Inquires the number of string models in the context. To add and remove string models, use [`MdmrControl`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of string models. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the context was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `Aurora Imaging Library system identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_PREPROCESSED`

Inquires whether the context is preprocessed. The context must be preprocessed, using [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md), before calling [`MdmrRead`](../../Reference/dmr/MdmrRead.md). After certain modifications to the context, as can be done with[`MdmrControl`](../../Reference/dmr/MdmrControl.md), [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md), or [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

---

### `M_SPACE_SIZE_MAX`

Inquires the value with which to determine the maximum width of a space character.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 3.0. |
| `Value > 0.0` | Specifies the maximum width value. |

---

### `M_SPACE_SIZE_MAX_MODE`

Inquires how to determine the maximum width of a space character.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CHAR_WIDTH_FACTOR` *(default)* | Specifies that the maximum width of a space character is determined by multiplying the [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrInquire.md) value by the string's maximum character width, in pixels. |
| `M_DISABLE` | Specifies no maximum space width. |

---

### `M_SPACE_SIZE_MIN`

Inquires the value with which to determine the minimum width of a space character.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 1.0. |
| `Value > 0.0` | Specifies the minimum width value. |

---

### `M_SPACE_SIZE_MIN_MODE`

Inquires how to determine the minimum width of a space character.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CHAR_WIDTH_FACTOR` *(default)* | Specifies that the minimum width of a space character is determined by multiplying the [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrInquire.md) value by the string's maximum character width, in pixels. |
| `M_DISABLE` | Specifies no minimum space width. |

---

### `M_STRING_ANGLE`

Inquires the angle at which to read a string.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_REGION` | Specifies that the angle is set to the angle of the target image's ROI. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies a fixed angle at which to read a string, in degrees. |

---

### `M_STRING_ANGLE_INPUT_UNITS`

Inquires the coordinate system that is used for [`M_STRING_ANGLE`](../../Reference/dmr/MdmrInquire.md). This is ignored when [`M_STRING_ANGLE`](../../Reference/dmr/MdmrInquire.md) is set to [`M_ACCORDING_TO_REGION`](../../Reference/dmr/MdmrInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. |

---

### `M_STRING_ANGLE_MODE`

Inquires whether the string angle is detected automatically or explicitly specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrInquire.md). |
| `M_ANGLE` | Specifies to use the angle specified with [`M_STRING_ANGLE`](../../Reference/dmr/MdmrInquire.md); the string will be read from left to right with the characters facing upward along the specified angle. |
| `M_AUTO` | Specifies that the string angle is detected automatically; [`M_STRING_ANGLE`](../../Reference/dmr/MdmrInquire.md) is ignored. |
| `M_ORIENTATION` | Specifies that the string will be read from left to right with the characters facing upward, or from right to left with the character facing downward. |

---

### `M_STRING_PARTIAL_CHAR_INVALID`

Inquires the single character string to replace invalid or unrecognized characters if [`M_STRING_PARTIAL_MODE`](../../Reference/dmr/MdmrInquire.md) is enabled.

| Value | Description |
| --- | --- |
| `"SingleCharacterString"` | Specifies the single character string to replace invalid or unrecognized characters. |

---

### `M_STRING_PARTIAL_MODE`

Inquires how partial strings are returned during the reading process.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that only complete strings will be returned. |
| `M_ENABLE` | Specifies that the best match for a partial string will be returned; unrecognized characters in the string will be replaced with[`M_STRING_PARTIAL_CHAR_INVALID`](../../Reference/dmr/MdmrInquire.md). |

---

### `M_STRING_PITCH`

Inquires the distance between successive dot centers, in the string angle direction, at which to read the characters in the strings. [`M_STRING_PITCH`](../../Reference/dmr/MdmrInquire.md) only has an effect if [`M_STRING_PITCH_MODE`](../../Reference/dmr/MdmrInquire.md)is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the pitch, in pixels. |

---

### `M_STRING_PITCH_MODE`

Inquires whether the distance between successive dot centers, in the string angle direction, is detected automatically or explicitly specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrInquire.md). |
| `M_AUTO` | Specifies to automatically establish the distance between dot centers in the string angle direction. |
| `M_USER_DEFINED` | Specifies to use the value set with [`M_STRING_PITCH`](../../Reference/dmr/MdmrInquire.md). |

---

### `M_TEXT_BLOCK_HEIGHT`

Inquires the height of the text block. This is the area that encloses all dot-matrix text in the image buffer (or in the buffer's associated region of interest).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the height, in pixels. |
| `M_INVALID` | Specifies an invalid height. Set the height to a valid value, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_TEXT_BLOCK_HEIGHT`](../../Reference/dmr/MdmrControl.md). |

---

### `M_TEXT_BLOCK_SIZE_MODE`

Inquires how to establish the size of the text block. This is the area that encloses all dot-matrix text in the image buffer (or in the buffer's associated region of interest).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED` *(default)* | Specifies that the size of the rectangular block is defined by the [`M_TEXT_BLOCK_HEIGHT`](../../Reference/dmr/MdmrInquire.md) and [`M_TEXT_BLOCK_WIDTH`](../../Reference/dmr/MdmrInquire.md) settings. |

---

### `M_TEXT_BLOCK_WIDTH`

Inquires the width of the text block. This is the area that encloses all dot-matrix text in the image buffer (or in the buffer's associated region of interest).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the width, in pixels. |
| `M_INVALID` | Specifies an invalid width. Set the width to a valid value, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_TEXT_BLOCK_WIDTH`](../../Reference/dmr/MdmrControl.md). |

---

### `M_TIMEOUT`

Inquires the maximum read time for [`MdmrRead`](../../Reference/dmr/MdmrRead.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies an infinite amount of read time. |
| `Value >= 0.0` *(default)* | Specifies the maximum read time, in msec. |

### Combination Constants — For retrieving the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string..

#### `M_STRING_SIZE`

Retrieves the number of characters in the string, including the terminating null character.

### For inquiring about a SureDotOCR result buffer

To inquire about the SureDotOCR result buffer specified with the [`ContextOrResultDmrId`](../../Reference/dmr/MdmrInquire.md) parameter, set the [`InquireType`](../../Reference/dmr/MdmrInquire.md) parameter to one of the values below.

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether to return results in pixels or world units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring if an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported for the SureDotOCR context currently being inquired.

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

### Combination Constants — For casting the result to a required data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
