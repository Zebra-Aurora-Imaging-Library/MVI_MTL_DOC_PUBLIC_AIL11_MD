---
doctype: Reference
module: dlocr
function: MdlocrGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrGetResult"
---

# MdlocrGetResult

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

> Get results from a Deep Learning OCR result buffer.

## Syntax

```c
void MdlocrGetResult(
    AIL_ID    ResultDlocrId,      //in
    AIL_INT64 StringIndex,        //in
    AIL_INT64 CharPositionIndex,  //in
    AIL_INT64 ResultType,         //in
    void *    ResultArrayPtr      //out
)
```

## Description

This function retrieves results of the specified type from a Deep Learning OCR result buffer. You can retrieve global results, string results, or character results. Results are available after calling [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).

If your target image ([`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md)) was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the center of upper-left pixel in the target image.

If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md) control type set to [`M_PIXEL`](../../Reference/dlocr/MdlocrControl.md). However, if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md) to [`M_WORLD`](../../Reference/dlocr/MdlocrControl.md) without specifying a calibrated image in which to calculate the results, [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md) generates an error.

Positional and dimensional results take into account the angle at which the result was found. The following example shows how the result for the bottom-left corner of the string box ([`M_STRING_BOX_BL_X`](../../Reference/dlocr/MdlocrGetResult.md) and [`M_STRING_BOX_BL_Y`](../../Reference/dlocr/MdlocrGetResult.md)) depends on the location and angle at which the string was read.

*[Image: MdmrResultStringBoxPosition.png]*

To remove all results from a result buffer, call [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md)with [`M_RESET`](../../Reference/dlocr/MdlocrControl.md). This does not delete the result buffer identifier, as is the case with [`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md).

## Parameters

### `ResultDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR result buffer from which to retrieve results.

### `StringIndex` *(in, AIL_INT64)*

Specifies the string (one or all) for which to get results, or specifies to get global results. Set this parameter to one of the values below.

*For specifying the string for which to get results, or for specifying global results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to retrieve results for all strings in the result buffer. To retrieve the number of strings in the result buffer, use [`M_STRING_NUMBER`](../../Reference/dlocr/MdlocrGetResult.md). |
| `M_GENERAL` *(default)* | Specifies to retrieve global results. These results are related to the result buffer itself. |
| `0 <= Value < M_STRING_NUMBER` | Specifies the index of the string for which to retrieve results. The index must be less than the total number of strings in the result buffer ([`M_STRING_NUMBER`](../../Reference/dlocr/MdlocrGetResult.md)). |

### `CharPositionIndex` *(in, AIL_INT64)*

Specifies the character (one or all) for which to get results, or specifies to get global or string results. Set this parameter to one of the values below.

*For specifying the character for which to get results, or for specifying global or string results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INDEX_IN_STRING` | Specifies to retrieve a character result by indicating the character's index, according to the string model definition. The [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter must be set to a string (one or all).

> **Note:** Note that indices don't include any substring set as a text anchor (using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_TEXT_ANCHOR_MODE`](../../Reference/dlocr/MdlocrControlStringModel.md)) since these characters are not returned. |
| `M_GENERAL` *(default)* | Specifies to retrieve global results or string results.

[`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md) retrieves global results when the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter is set to [`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md). [`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md) retrieves string results when the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter is set to a string (one or all). Note that indices don't include any substring set as a text anchor (using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with[`M_TEXT_ANCHOR_MODE`](../../Reference/dlocr/MdlocrControlStringModel.md)). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write results.

## Parameter Associations

### For retrieving global results

When retrieving global results, set the [`ResultType`](../../Reference/dlocr/MdlocrGetResult.md) parameter to the value below. In this case, set both the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) and [`CharPositionIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameters to either [`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md) or [`M_DEFAULT`](../../Reference/dlocr/MdlocrGetResult.md).

---

### `M_STATUS`

Retrieves information regarding the state of the read operation.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the read operation completed successfully. It is possible to read 0 strings ([`M_STRING_NUMBER`](../../Reference/dlocr/MdlocrGetResult.md)), and have a successfully completed read operation. |
| `M_CURRENTLY_READING` | Specifies that the read operation is currently ongoing. You can only get this status if you are retrieving it from another thread. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred. |
| `M_NOT_ENOUGH_MEMORY` | Specifies that a memory allocation error occurred while reading. |
| `M_READ_NOT_PERFORMED` | Specifies that the read operation has not been performed. This is the initial status. |
| `M_STOPPED_BY_REQUEST` | Specifies that the read operation was explicitly stopped ([`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_STOP_READ`](../../Reference/dlocr/MdlocrControl.md)). |
| `M_TIMEOUT_REACHED` | Specifies that the read operation ended because the timeout limit was reached ([`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_TIMEOUT`](../../Reference/dlocr/MdlocrControl.md)). |

---

### `M_STRING_NUMBER`

Retrieves the total number of strings read. This is the total number of strings read and stored in the result buffer.

### For retrieving string results

When retrieving string results, set the [`ResultType`](../../Reference/dlocr/MdlocrGetResult.md) parameter to one of the values below. In this case, set the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter to the index of the string for which to get the result or [`M_ALL`](../../Reference/dlocr/MdlocrGetResult.md) (unless otherwise specified), and set the [`CharPositionIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter to either [`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md) or [`M_DEFAULT`](../../Reference/dlocr/MdlocrGetResult.md).

---

### `M_INTERCHAR_MAX_WIDTH`

Retrieves the maximum inter-character width in the string that was read. The width is relative to output coordinate system specified with [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the inter-character width. |

---

### `M_SKIPPED_POSITIONS`

Retrieves the indices of the positions that were skipped in the string model. This array will only be filled if [`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md) is different from [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md); if they are equal, it means that no positions were skipped.  > **Note:** Note that indices don't include any substring set as a text anchor (using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_TEXT_ANCHOR_MODE`](../../Reference/dlocr/MdlocrControlStringModel.md)) since these characters are not returned.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the index of the position in the model that was skipped. |

---

### `M_STRING`

Retrieves the string that was read. This information is returned as a regular string. It includes the name of all the characters in the string and the terminating null character. It does not include any text anchors set using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md).

---

### `M_STRING_CHAR_NUMBER`

Retrieves the total number of characters read. This number does not count the terminating null character.

---

### `M_STRING_MODEL_INDEX`

Retrieves the index of the string model, in the Deep Learning OCR context, if one was used to read the string.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that no string model exists in the context. |
| `Value >= 0` | Specifies the index of the string model used to read the string. |

---

### `M_STRING_MODEL_LABEL`

Retrieves the label of the string model, in the Deep Learning OCR context, if one was used to read the string.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that no string model exists in the context. |
| `Value >= 0` | Specifies the label of the string model used to read the string. |

---

### `M_STRING_SCORE`

Retrieves the string score calculated during the read operation. The string score is the average score of the characters in the string.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the string score. |

### For retrieving global or string results

When retrieving global or string results (one or all), set the [`ResultType`](../../Reference/dlocr/MdlocrGetResult.md) parameter to one of the values below. In this case, set the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter to one or all strings, [`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md), or [`M_DEFAULT`](../../Reference/dlocr/MdlocrGetResult.md), and set the [`CharPositionIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter to either [`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md) or [`M_DEFAULT`](../../Reference/dlocr/MdlocrGetResult.md).  > **Note:** Note when the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter is set to either [`M_GENERAL`](../../Reference/dlocr/MdlocrGetResult.md) or [`M_DEFAULT`](../../Reference/dlocr/MdlocrGetResult.md), the result type will be in reference to the region that encompasses all strings, as opposed to the region of the specific string(s).

---

### `M_STRING_ANGLE`

Retrieves the angle of the string's bounding box, or the bounding box encompassing all strings, in degrees. The angle is measured at the box's base and is relative to the output coordinate system ([`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md)with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md)).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise, from the positive X-axis towards the negative Y-axis of the image. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 360.0` | Specifies the string score. |

---

### `M_STRING_BOX_BL_X`

Retrieves the X-coordinate of the bottom-left corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_BL_Y`

Retrieves the Y-coordinate of the bottom-left corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_BR_X`

Retrieves the X-coordinate of the bottom-right corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_BR_Y`

Retrieves the Y-coordinate of the bottom-right corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_UL_X`

Retrieves the X-coordinate of the upper-left corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_UL_Y`

Retrieves the Y-coordinate of the upper-left corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_UR_X`

Retrieves the X-coordinate of the upper-right corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_UR_Y`

Retrieves the Y-coordinate of the upper-right corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_HEIGHT`

Retrieves the height of the string's bounding box, or the bounding box encompassing all strings.

---

### `M_STRING_POSITION_X`

Retrieves the X-coordinate of the center of the bounding box of the string, or the bounding box encompassing all strings; this position is considered the position of the string, or bounding box of all strings.

---

### `M_STRING_POSITION_Y`

Retrieves the Y-coordinate of the center of the bounding box of the string, or the bounding box encompassing all strings; this position is considered the position of the string, or bounding box of all strings.

---

### `M_STRING_WIDTH`

Retrieves the width of the string's bounding box, or the bounding box encompassing all strings.

### For retrieving character results

When retrieving results for characters in a string, set the [`ResultType`](../../Reference/dlocr/MdlocrGetResult.md) parameter to one of the values below. In this case, set the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter to a string (one or all), and set the [`CharPositionIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter to a character (one or all using **M_INDEX_IN_STRING()**). Characters are ordered by their index position in their string.

---

### `M_CHAR_BOX_BL_X`

Retrieves the X-coordinate of the bottom-left corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_BL_Y`

Retrieves the Y-coordinate of the bottom-left corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_BR_X`

Retrieves the X-coordinate of the bottom-right corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_BR_Y`

Retrieves the Y-coordinate of the bottom-right corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_UL_X`

Retrieves the X-coordinate of the upper-left corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_UL_Y`

Retrieves the Y-coordinate of the upper-left corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_UR_X`

Retrieves the X-coordinate of the upper-right corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_UR_Y`

Retrieves the Y-coordinate of the upper-right corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_HEIGHT`

Retrieves the height of the character's bounding box.

---

### `M_CHAR_NAME`

Retrieves the name of the character in the string that was read. The name is returned as a regular string. It consists of the character name and the terminating null character.  > **Note:** Note that [`M_CHAR_NAME`](../../Reference/dlocr/MdlocrGetResult.md) cannot be used when the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter is set to [`M_ALL`](../../Reference/dlocr/MdlocrGetResult.md) or when the [`CharPositionIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter is set to **M_INDEX_IN_STRING()**.

---

### `M_CHAR_POSITION_X`

Retrieves the X-coordinate of the center of the bounding box of the character; this position is considered the position of the character.

---

### `M_CHAR_POSITION_Y`

Retrieves the Y-coordinate of the center of the bounding box of the character; this position is considered the position of the character.

---

### `M_CHAR_SCORE`

Retrieves the character's score. This quantifies the confidence that the Deep Learning OCR module has correctly predicted the character.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the character score. |

---

### `M_CHAR_WIDTH`

Retrieves the width of the character's bounding box.

### Combination Constants — For retrieving the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Retrieves the number of characters in the string, including the terminating null character.  For [`M_CHAR_NAME`](../../Reference/dlocr/MdlocrGetResult.md), the name of each letter at each position is itself a string, with its own terminating null character. You would need to sum them all to get the required array size for a string. For a specific string, [`M_STRING`](../../Reference/dlocr/MdlocrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dlocr/MdlocrGetResult.md) returns the number of characters in the string plus 1 for each character's terminating null character.

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

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

> **Note:** Note that this result type cannot be used when the [`StringIndex`](../../Reference/dlocr/MdlocrGetResult.md) parameter is set to [`M_ALL`](../../Reference/dlocr/MdlocrGetResult.md).
