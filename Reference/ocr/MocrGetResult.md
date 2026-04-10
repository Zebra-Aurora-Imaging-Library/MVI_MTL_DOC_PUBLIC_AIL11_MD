---
doctype: Reference
module: ocr
function: MocrGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrGetResult"
---

# MocrGetResult

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

> Get the specified type of result(s) from an OCR result buffer.

## Syntax

```c
void MocrGetResult(
    AIL_ID    ResultOcrId,  //in
    AIL_INT64 ResultType,   //in
    void *    ResultPtr     //out
)
```

## Description

This function retrieves the result(s) of the specified type from an OCR result buffer. The information can be retrieved as a text, a string or a character. A text contains one or more strings. A string contains one or more characters. Results are only available after calling [`MocrReadString`](../../Reference/ocr/MocrReadString.md) or [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md).

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

> **Note:** If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/ocr/MocrControl.md) control type set to [`M_PIXEL`](../../Reference/ocr/MocrControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/ocr/MocrControl.md) to [`M_WORLD`](../../Reference/ocr/MocrControl.md) without specifying a calibrated image in which to calculate the results, [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md) will generate an error.

Note that, by default, the first string found is returned as a result. To get the result for a specific string or for all strings from a result buffer, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SELECT_STRING`](../../Reference/ocr/MocrControl.md).

## Parameters

### `ResultOcrId` *(in, AIL_ID)*

Specifies the identifier of the OCR result buffer from which to retrieve results.

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result(s) to retrieve. The following values are available to retrieve results from a read or a verify operation, unless otherwise specified.

### `ResultPtr` *(out, *void)*

Specifies the address in which to write the results. Each line of text found within the target image is read as a separate string. If there is only one line of text, there is only one string. Only the results from the first string or selected strings ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SELECT_STRING`](../../Reference/ocr/MocrControl.md)) are obtained.

## Parameter Associations

### For all strings

To retrieve a result for all strings, the [`ResultType`](../../Reference/ocr/MocrGetResult.md) parameter can be set to one of the following:

---

### `M_NB_STRING`

Retrieves the number of strings read by [`MocrReadString`](../../Reference/ocr/MocrReadString.md).  If no strings are found, 0 is returned.

---

### `M_TEXT`

Retrieves the entire text. This includes all available strings, their separators, and the terminating null character ("\0").  Note that a string separator is inserted between the different strings when getting all strings simultaneously. The actual string separator can be controlled using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TEXT_STRING_SEPARATOR`](../../Reference/ocr/MocrControl.md). Refer to [`M_STRING`](../../Reference/ocr/MocrGetResult.md) to retrieve the strings without separators.

---

### `M_TEXT_SCORE`

Retrieves the match score for the entire text as determined during the read/verify operation.

---

### `M_THRESHOLD`

Retrieves the value used to binarize the target image. Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, or when using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context if your target image has multiple lines, or its string angle is not 0.0°.

### For retrieving one or more strings

The following values are available to retrieve one or more strings, unless otherwise specified. To specify the number of strings to retrieve, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SELECT_STRING`](../../Reference/ocr/MocrControl.md). To retrieve a result for a string or all strings, the [`ResultType`](../../Reference/ocr/MocrGetResult.md) parameter can be set to one of the following:

---

### `M_STRING`

Retrieves the null-terminated string(s) found during the read/verify operation. By default, any unrecognized character in the string will be replaced by the most-likely character, even if the match score for that character is lower than the specified character acceptance level ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_ACCEPTANCE`](../../Reference/ocr/MocrControl.md)). You can replace all unrecognized characters in the string with an invalid character symbol by calling [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_INVALID`](../../Reference/ocr/MocrControl.md).  Note that no string separator is inserted between the different strings when getting all strings simultaneously. Refer to [`M_TEXT`](../../Reference/ocr/MocrGetResult.md) to retrieve the strings with separators.

---

### `M_STRING_ANGLE`

Retrieves the search angle of the string(s), in degrees, relative to the output coordinate system specified using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/ocr/MocrControl.md). For multiple strings, the search angle of each string is returned. Note that all strings are searched for at the same angle, so all returned values will be equivalent.  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

---

### `M_STRING_CHAR_NUMBER`

Retrieves the number of characters in the string(s) found during the read/verify operation, not including the terminating null character ("\0") itself; this means it returns the number of characters with results. For multiple strings, the length of each null-terminated string, not including the terminating null character ("\0") itself, is returned.

---

### `M_STRING_SCORE`

Retrieves the score calculated during the read/verify operation for the entire string(s). For multiple strings, the calculated score for each string is returned.

---

### `M_STRING_VALID_FLAG`

Retrieves the result of the read/verify operation as a flag that denotes the validity of the string(s). The flag's validity status is determined by comparing the string's read score with the string's acceptance level ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_ACCEPTANCE`](../../Reference/ocr/MocrControl.md)). For multiple strings, a validity flag for each string is returned.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the string's score is below the string acceptance level. |
| `M_TRUE` | Specifies that the string's score is above the string acceptance level. |

### Combination Constants — For retrieving the string length (including the terminating null character).

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string length (including the terminating null character).

#### `M_STRING_SIZE`

Retrieves the length of one or more strings, including the terminating null character ("\0"). In the case of [`M_TEXT`](../../Reference/ocr/MocrGetResult.md), it also includes string separators. This combination constant is useful to establish the required array size to retrieve the string.

### Combination Constants — For specifying to get the total number of characters for all strings

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether to get the total number of characters for all strings, instead of one value per string.

#### `M_SUM`

Specifies to return the total number of characters for all strings, instead of one value per string. Note that the terminating null character ("\0") is ignored by this combination constant.  Note that this value cannot be combined with [`M_TEXT`](../../Reference/ocr/MocrGetResult.md).

### For individual characters

The following values are available to retrieve the results for individual characters in one or more strings, unless otherwise specified. To specify the number of strings to retrieve, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SELECT_STRING`](../../Reference/ocr/MocrControl.md). To retrieve a result for the individual characters of one or more strings, the [`ResultType`](../../Reference/ocr/MocrGetResult.md) parameter can be set to one of the following:

---

### `M_CHAR_POSITION_X`

Retrieves the X-position of each individual character in the string or strings.

---

### `M_CHAR_POSITION_Y`

Retrieves the Y-position of each individual character in the string or strings.

---

### `M_CHAR_SCORE`

Retrieves the match score of each individual character within the string or strings.

---

### `M_CHAR_SIZE_X`

Retrieves the cell width of each character within the string or strings.

---

### `M_CHAR_SIZE_Y`

Retrieves the cell height of each character within the string or strings.

---

### `M_CHAR_SPACING`

Retrieves the spacing between each pair of characters in the selected string,or strings. Note that the spacing of the last character in each string is **M_NULL**.

---

### `M_CHAR_VALID_FLAG`

Retrieves the result of the read/verify operation as flags that denote the validity of each character within the string or strings. Each flag's validity status is determined by comparing the character's read score with the character's acceptance level ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_ACCEPTANCE`](../../Reference/ocr/MocrControl.md)).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the character's score is below the character acceptance level. |
| `M_TRUE` | Specifies that the character's score is above the character acceptance level. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

#### `M_TYPE_TEXT_CHAR`

Cast the requested results to an _AIL_TEXT_CHAR_.
