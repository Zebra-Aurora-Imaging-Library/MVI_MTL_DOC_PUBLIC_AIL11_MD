---
doctype: Reference
module: ocr
function: MocrInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrInquire"
---

# MocrInquire

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

> Inquire about OCR font context or an OCR result buffer setting.

## Syntax

```c
AIL_INT MocrInquire(
    AIL_ID    FontContextOrResultOcrId,  //in
    AIL_INT64 InquireType,               //in
    void *    UserVarPtr                 //out
)
```

## Description

This function inquires about an OCR font context or an OCR result buffer setting.

Note that for an OCR result buffer, this function only retrieves information about result buffer settings (set with [`MocrControl`](../../Reference/ocr/MocrControl.md), for example). To retrieve results from the OCR result buffer, use [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md).

## Parameters

### `FontContextOrResultOcrId` *(in, AIL_ID)*

Specifies the identifier of the OCR font context or the OCR result buffer from which to read information.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MocrInquire`](../../Reference/ocr/MocrInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For OCR settings

For an OCR font context, the [`InquireType`](../../Reference/ocr/MocrInquire.md) parameter can be set to one of the following:

---

### `M_BLANK_CHARACTERS`

Inquires whether the space between characters in the target image is read. Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that blank spaces will not be read from the target image into the result string. |
| `M_ENABLE` | Specifies that blank spaces will be read from the target image into the result string. |

---

### `M_BROKEN_CHAR`

Inquires the capacity to read/verify broken characters.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that, during the read/verify, OCR should not try to compensate for broken characters. |
| `M_ENABLE` | Specifies that broken characters should be identifies as possible characters. |

---

### `M_CHAR_ACCEPTANCE`

Inquires the acceptance level used to determine a successful match between the font and the characters found within the target image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level for a target character. |

---

### `M_CHAR_CELL_SIZE_X`

Inquires the font's character cell width.

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the width, in pixels. |

---

### `M_CHAR_CELL_SIZE_Y`

Inquires the font's character cell height.

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the height, in pixels. |

---

### `M_CHAR_IN_FONT`

Inquires the string of ASCII characters associated with the character representations in the font.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the parameter is ignored. |
| `"StringOfCharacters"` | Specifies the string with the list of ASCII characters. |

---

### `M_CHAR_INVALID`

Inquires the symbol for the unrecognized characters within the string, or strings.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies that no special character will replace unrecognized characters. |
| `1 <= Value <= 255` | Specifies the character that will replace unrecognized characters. |

---

### `M_CHAR_NUMBER`

Inquires the number of characters that can be stored in the font of the OCR font context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of characters. |

---

### `M_CHAR_NUMBER_IN_FONT`

Inquires the number of characters that are actually stored in the OCR font context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of characters. |

---

### `M_CHAR_OFFSET_X`

Inquires the font's character offset along the X-axis.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset along the X-axis, in pixels. |

---

### `M_CHAR_OFFSET_Y`

Inquires the font's character offset along the Y-axis.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset along the Y-axis, in pixels. |

---

### `M_CHAR_POSITION_VARIATION_X`

Inquires the amount by which the position of the characters in the target image can vary along the X-axis. This value is expressed in pixels of the character, not of the target image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the position tolerance, in pixels of the character, not of the target image. |

---

### `M_CHAR_POSITION_VARIATION_Y`

Inquires the amount by which the position of the characters in the target image can vary along the Y-axis. This value is expressed in pixels of the character, not of the target image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the position tolerance, in pixels of the character, not of the target image. |

---

### `M_CHAR_SIZE_X`

Inquires the width of the font's characters.

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the width, in pixels. |

---

### `M_CHAR_SIZE_Y`

Inquires the height of the font's characters.

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the height, in pixels. |

---

### `M_CHAR_THICKNESS`

Inquires the average thickness of the font's characters.

| Value | Description |
| --- | --- |
| `Value` | Specifies the average thickness, in pixels. |

---

### `M_CONSTRAINT + n`

Inquires the constraint string for the _n_ <sup>th</sup> position in the read/verified null-terminated string.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no constraint. |
| `"StringOfCharacters"` | Specifies the string with the list of ASCII characters. |

---

### `M_CONSTRAINT_TYPE + n`

Inquires the constraint type for the _n_ <sup>th</sup> character in the string, as set by [`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that all characters present in the font are valid. |
| `M_DIGIT` | Specifies that only characters "0" to "9" (ASCII codes 48 to 57) are valid. |
| `M_LETTER` | Specifies that only characters "A" to "Z" and "a" to "z" (ASCII codes 65 to 90 and 97 to 122) are valid. |
| `M_LOWERCASE` | Specifies that only characters "a" to "z" (ASCII codes 97 to 122) are valid. |
| `M_UPPERCASE` | Specifies that only characters "A" to "Z" (ASCII codes 65 to 90) are valid. |

---

### `M_CONTEXT`

Inquires the type of the OCR font context.

| Value | Description |
| --- | --- |
| `M_CONSTRAINED` | Specifies an OCR font context that works well with degraded target images. |
| `M_GENERAL` | Specifies an OCR font context that works well with clean target images. |

---

### `M_EXTRA_CHARACTERS`

Inquires whether the string is still identified and read when the image contains more characters than otherwise expected.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the read/verify operation should not try to compensate for extra characters. |
| `M_ENABLE` | Specifies that the read/verify operation should try to compensate for extra characters. |

---

### `M_FONT_INIT_FLAG`

Inquires the initialization flag passed at font allocation.

| Value | Description |
| --- | --- |
| `M_FOREGROUND_BLACK` | Specifies that the characters to read or verify are darker than the background. |
| `M_FOREGROUND_WHITE` | Specifies that the characters to read or verify are brighter than the background. |

---

### `M_FONT_TYPE`

Inquires the font type of the OCR font context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SEMI_M12_92` | Specifies a font respecting the SEMI M12-92 standard as the type of font. |
| `M_SEMI_M13_88` | Specifies a font respecting the SEMI M13-88 standard as the type of font. |
| `M_USER_DEFINED` *(default)* | Specifies a general, user-defined type of font. |
| `M_CONSTRAINED` *(default)* | Specifies an OCR font context that works well with degraded target images and requires more information about the target string, but provides a more robust search. |
| `M_GENERAL` | Specifies an OCR font context that works well with clean target images. |

---

### `M_FOREGROUND_VALUE`

Inquires the foreground value of the font.

| Value | Description |
| --- | --- |
| `M_FOREGROUND_BLACK` | Specifies that the characters to read or verify are darker than the background. |
| `M_FOREGROUND_WHITE` | Specifies that the characters to read or verify are brighter than the background. |

---

### `M_MORPHOLOGIC_FILTERING`

Inquires the number of iterations of morphological filtering.

| Value | Description |
| --- | --- |
| `0 <= Value <= 100` *(default)* | Specifies the number of iterations. |

---

### `M_PREPROCESSED`

Inquires whether preprocessing is necessary. The font context must be preprocessed (using [`MocrPreprocess`](../../Reference/ocr/MocrPreprocess.md)) before calling [`MocrReadString`](../../Reference/ocr/MocrReadString.md) or [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md). After some of the target constraints or font-specific controls are changed, this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that preprocessing is necessary. |
| `M_TRUE` | Specifies that preprocessing is not necessary. |

---

### `M_SKIP_STRING_LOCATION`

Inquires whether the string location step will be skipped or not.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that the step will not be skipped. |
| `M_ENABLE` | Specifies that the step will be skipped. |

---

### `M_SPEED`

Inquires the algorithm's search speed.

| Value | Description |
| --- | --- |
| `M_HIGH` | Specifies a high speed. |
| `M_LOW` | Specifies a low speed. |
| `M_MEDIUM` *(default)* | Specifies a medium speed. |
| `M_VERY_HIGH` | Specifies a very high speed. |
| `M_VERY_LOW` | Specifies a very low speed. |

---

### `M_STRING_ACCEPTANCE`

Inquires the acceptance level used to determine a successful match between the font and the read/verified string.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level for a string. |

---

### `M_STRING_ANGLE`

Inquires the angle at which the string is expected to be found.

| Value | Description |
| --- | --- |
| `M_ACCORDING_TO_REGION` *(default)* | Specifies to use the angle of the rectangular ROI (set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)) associated with the target image buffer. |
| `0.0 <= Value <= 360.0` | Specifies the angle, in degrees. |

---

### `M_STRING_ANGLE_DELTA_NEG`

Inquires the possible angle variation in a clockwise rotation, relative to [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the possible clockwise angle variation, in degrees. |

---

### `M_STRING_ANGLE_DELTA_POS`

Inquires the possible angle variation in a counter-clockwise rotation, relative to [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the possible counter-clockwise angle variation, in degrees. |

---

### `M_STRING_ANGLE_INTERPOLATION_MODE`

Inquires the interpolation mode to use when reading/verifying a string at an angle.

| Value | Description |
| --- | --- |
| `M_BICUBIC` | Specifies bicubic interpolation. |
| `M_BILINEAR` *(default)* | Specifies bilinear interpolation. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. |

---

### `M_STRING_CHAR_NUMBER`

Inquires the length of the string to read/verify from the target image.

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the length of the string is unknown. |
| `Value <= M_STRING_SIZE_MAX` | Specifies the string length to read/verify. |

---

### `M_STRING_NUMBER`

Inquires the number of strings to read from the target image.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that the number of strings in the target image should be determined automatically. |
| `Value` *(default)* | Specifies the number of lines of text in the target image. |

---

### `M_STRING_SIZE_MAX`

Inquires the maximum string length that will be read/verified using the OCR font context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum string length. |

---

### `M_TARGET_CHAR_SIZE_X`

Inquires the width of the expected target characters.

| Value | Description |
| --- | --- |
| `M_SAME` | Specifies that the character width is found automatically. |
| `Value > 1` | Specifies the character width in pixels (with subpixel accuracy). |

---

### `M_TARGET_CHAR_SIZE_Y`

Inquires the height of the expected target characters.

| Value | Description |
| --- | --- |
| `M_SAME` | Specifies that the character height is found automatically. |
| `Value > 1` | Specifies the character height in pixels (with subpixel accuracy). |

---

### `M_TARGET_CHAR_SPACING`

Inquires the expected amount of space between characters in the string.

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the inter-character spacing is unknown and not the same between the characters. |
| `M_SAME` | Specifies that the inter-character spacing is the same throughout the target string. |
| `Value >= 2` | Specifies the inter-character spacing, in pixels, with subpixel accuracy. |

---

### `M_TEXT_STRING_SEPARATOR`

Inquires the ASCII code of the character to be used as a string separator within the text read.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the ASCII code of the character. |

---

### `M_THICKEN_CHAR`

Inquires the number of times a character should be thickened.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 100` *(default)* | Specifies the number of times a character should be thickened. |

---

### `M_THRESHOLD`

Inquires the threshold value used to internally binarize the target image.

| Value | Description |
| --- | --- |
| `M_AUTO` | Specifies  an automatically computed threshold value. |
| `Value` *(default)* | Specifies the threshold value. |

---

### `M_TOUCHING_CHAR`

Inquires whether the capability to read characters that touch is enabled or not.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the identification of touching characters. |
| `M_ENABLE` | Specifies to enable the identification of touching characters. |

### Combination Constants — Returns the length of the constraint string

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the length of the constraint string.

#### `M_STRING_SIZE`

Retrieves the length of the constraint string, including the terminating null character ("\0").

### Combination Constants — Returns the letter case for the constraint

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the letter case for the constraint.

| Value | Description |
| --- | --- |
| `M_LOWERCASE` | Specifies that only characters "a" to "z" (ASCII codes 97 to 122) are valid. |
| `M_UPPERCASE` | Specifies that only characters "A" to "Z" (ASCII codes 65 to 90) are valid. |

### Combination Constants — Returns the context type

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to determine the OCR font context type.

| Value | Description |
| --- | --- |
| `M_CONSTRAINED` *(default)* | Specifies an OCR font context that works well with degraded target images and requires more information about the target string, but provides a more robust search. |
| `M_GENERAL` | Specifies an OCR font context that works well with clean target images. |

### Combination Constants — For

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the returned string is sorted in ascending ASCII order.

| Value | Description |
| --- | --- |
| `M_SORT` | Sorts the returned string in ascending ASCII order. |

### For specifying the result for

For an OCR result buffer, the [`InquireType`](../../Reference/ocr/MocrInquire.md) parameter can be set to one of the following:

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

---

### `M_SELECT_STRING`

Inquires the index of the string, selected from a multiple line text, for which to get the results with [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md).

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that all strings are selected. |
| `0 <= Value < M_STRING_NUMBER` *(default)* | Specifies a specific string. |

### For inquiring about the system

To inquire about the system on which the OCR font context or result buffer is allocated, set the [`InquireType`](../../Reference/ocr/MocrInquire.md) parameter to the value below.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the OCR font context or result buffer is allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

#### `M_TYPE_AIL_UINT8`

Casts the requested information to an _AIL_UINT8_.

#### `M_TYPE_TEXT_CHAR`

Casts the requested information to an _AIL_TEXT_CHAR_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
