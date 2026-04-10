---
doctype: Reference
module: dmr
function: MdmrGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrGetResult"
---

# MdmrGetResult

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

> Get results from a SureDotOCR result buffer.

## Syntax

```c
void MdmrGetResult(
    AIL_ID    ResultDmrId,        //in
    AIL_INT64 StringIndex,        //in
    AIL_INT64 CharPositionIndex,  //in
    AIL_INT64 ResultType,         //in
    void *    ResultArrayPtr      //out
)
```

## Description

This function retrieves results of the specified type from a SureDotOCR result buffer. You can retrieve global results, string results, or character results. Results are available after calling [`MdmrRead`](../../Reference/dmr/MdmrRead.md).

If your target image ([`MdmrRead`](../../Reference/dmr/MdmrRead.md)) was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the center of top-left pixel in the target image.

If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/dmr/MdmrControl.md) control type set to [`M_PIXEL`](../../Reference/dmr/MdmrControl.md). However, if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/dmr/MdmrControl.md) to [`M_WORLD`](../../Reference/dmr/MdmrControl.md) without specifying a calibrated image in which to calculate the results, [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) generates an error.

Positional and dimensional results take into account the angle at which the result was found. The following example shows how the result for the bottom-left corner of the string box ([`M_STRING_BOX_BL_X`](../../Reference/dmr/MdmrGetResult.md) and [`M_STRING_BOX_BL_Y`](../../Reference/dmr/MdmrGetResult.md)) depends on the location and angle at which the string was read.

*[Image: MdmrResultStringBoxPosition.png]*

To remove all results from a result buffer, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md)with [`M_RESET`](../../Reference/dmr/MdmrControl.md). This does not delete the buffer identifier, as is the case with [`MdmrFree`](../../Reference/dmr/MdmrFree.md).

## Parameters

### `ResultDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR result buffer from which to retrieve results.

### `StringIndex` *(in, AIL_INT64)*

Specifies the string (one or all) for which to get results, or specifies to get global results. Set this parameter to one of the values below.

*For specifying the string for which to get results, or for specifying global results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to retrieve results for all strings in the result buffer. To retrieve the number of strings in the result buffer, use [`M_STRING_NUMBER`](../../Reference/dmr/MdmrGetResult.md). |
| `M_GENERAL` *(default)* | Specifies to retrieve global results. These results are related to the result buffer itself. |
| `0 <= Value < M_STRING_NUMBER` | Specifies the index of the string for which to retrieve results. The index must be less than the total number of strings in the result buffer ([`M_STRING_NUMBER`](../../Reference/dmr/MdmrGetResult.md)). |

### `CharPositionIndex` *(in, AIL_INT64)*

Specifies the character (one or all) for which to get results, or specifies to get global or string results. Set this parameter to one of the values below.

*For specifying the character for which to get results, or for specifying global or string results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INDEX_IN_FORMATTED_STRING` | Specifies to retrieve a character result by indicating the character's index, according to the target string format. The [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter must be set to a string (one or all).

If a space is read in the target string, it is indexed as a character result, even if you did not add a space to the string model as a permitted character constraint ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)). For example, if the string model definition specifies to read 4 capital letters ("LLLL"), and the target string read is "AB CD", **M_INDEX_IN_FORMATTED_STRING(3)**refers to 'C'. If the string model definition specifies to read 4 capital letters and 1 space ("LLSLL"), and the target string read is "AB CD", **M_INDEX_IN_FORMATTED_STRING(3)**also refers to 'C'. |
| `M_INDEX_IN_STRING` | Specifies to retrieve a character result by indicating the character's index, according to the string model definition. The [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter must be set to a string (one or all).

Spaces read in the target string can be indexed as a character result, but only if they correspond to a space permitted character constraint in the string model ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)). For example, if the string model definition specifies to read 4 capital letters ("LLLL"), and the target string read is "AB CD", **M_INDEX_IN_STRING(3)**refers to 'D'. If the string model definition specifies to read 4 capital letters and 1 space ("LLSLL"), and the target string read is "AB CD", **M_INDEX_IN_STRING(3)**refers to 'C'. |
| `M_GENERAL` *(default)* | Specifies to retrieve global results or string results.

[`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md)or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md) retrieves global results when the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter is set to [`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md) or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md). [`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md)or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md) retrieves string results when the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter is set to a string (one or all). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write results.

## Parameter Associations

### For retrieving global results

When retrieving global results, set the [`ResultType`](../../Reference/dmr/MdmrGetResult.md) parameter to the value below. In this case, set both the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) and [`CharPositionIndex`](../../Reference/dmr/MdmrGetResult.md) parameters to either [`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md) or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md).

---

### `M_STATUS`

Retrieves information regarding the state of the read operation.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the read operation completed successfully. It is possible to read 0 strings ([`M_STRING_NUMBER`](../../Reference/dmr/MdmrGetResult.md)), and have a successfully completed read operation. |
| `M_CURRENTLY_READING` | Specifies that the read operation is currently ongoing. You can only get this status if you are retrieving it from another thread. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred. |
| `M_NOT_ENOUGH_MEMORY` | Specifies that a memory allocation error occurred while reading. |
| `M_READ_NOT_PERFORMED` | Specifies that the read operation has not been performed. This is the initial status. |
| `M_STOPPED_BY_REQUEST` | Specifies that the read operation was explicitly stopped ([`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_STOP_READ`](../../Reference/dmr/MdmrControl.md)). |
| `M_TIMEOUT_REACHED` | Specifies that the read operation ended because the timeout limit was reached ([`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_TIMEOUT`](../../Reference/dmr/MdmrControl.md)). |

---

### `M_STRING_NUMBER`

Retrieves the total number of strings read. This is the total number of strings held by the result buffer.

### For retrieving string results

When retrieving string results, set the [`ResultType`](../../Reference/dmr/MdmrGetResult.md) parameter to one of the values below. In this case, set the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter to the index of the string for which to get the result or [`M_ALL`](../../Reference/dmr/MdmrGetResult.md) (unless otherwise specified), and set the [`CharPositionIndex`](../../Reference/dmr/MdmrGetResult.md) parameter to either [`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md) or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md).

---

### `M_FORMATTED_STRING`

Retrieves the string that was read, according to the target string format. This information is returned as a regular string. It includes the name of all the characters in the string and the terminating null character. It also includes spaces, if the target string has any that are greater than the minimum spacing ([`MdmrControl`](../../Reference/dmr/MdmrControl.md)with [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md)) and less than the maximum spacing ([`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md)).

---

### `M_FORMATTED_STRING_CHAR_INVALID_INDICES`

Retrieves the positions of invalid or unreadable characters in the formatted string.

---

### `M_FORMATTED_STRING_CHAR_NUMBER`

Retrieves the total number of characters read, according to the target string format. This number includes spaces, if any were read in the target string, even if you did not add spaces to the string model as permitted character constraints ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)). [`M_FORMATTED_STRING_CHAR_NUMBER`](../../Reference/dmr/MdmrGetResult.md) does not count the terminating null character.

---

### `M_SKIPPED_POSITIONS`

Retrieves the positions of the skipped characters; this result is only available when [`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) is less than [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md).

---

### `M_STRING`

Retrieves the string that was read, according to the string model definition. This information is returned as a regular string. It includes the name of all the characters in the string and the terminating null character. It also include spaces, if they correspond to a space permitted character constraint in the string model ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)).

---

### `M_STRING_CHAR_INVALID_INDICES`

Retrieves the positions of invalid or unreadable characters in the string.  To retrieve the positions of these characters in the formatted string, use [`M_FORMATTED_STRING_CHAR_INVALID_INDICES`](../../Reference/dmr/MdmrGetResult.md).

---

### `M_STRING_CHAR_NUMBER`

Retrieves the total number of characters read, according to the string model definition. This number can include spaces, but only if they correspond to a space permitted character constraint in the string model ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)). [`M_STRING_CHAR_NUMBER`](../../Reference/dmr/MdmrGetResult.md) does not count the terminating null character.

---

### `M_STRING_MODEL_INDEX`

Retrieves the index of the string model, in the SureDotOCR context, used to read the string.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the index of the string model used to read the string. |

---

### `M_STRING_MODEL_LABEL`

Retrieves the label of the string model, in the SureDotOCR context, used to read the string.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the index of the string model used to read the string. |

---

### `M_STRING_SCORE`

Retrieves the string score calculated during the read operation. The string score is the average score of the characters in the string. This score does not include the score of an [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) permitted character ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md)), if any were read in the string.

### For retrieving global or string results

When retrieving global or string results (one or all), set the [`ResultType`](../../Reference/dmr/MdmrGetResult.md) parameter to one of the values below. In this case, set the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter to one or all strings, [`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md) or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md), and set the [`CharPositionIndex`](../../Reference/dmr/MdmrGetResult.md) parameter to either [`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md) or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md).  > **Note:** Note when the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter is set to either [`M_GENERAL`](../../Reference/dmr/MdmrGetResult.md) or [`M_DEFAULT`](../../Reference/dmr/MdmrGetResult.md), the result type will be in reference to the region that encompasses all strings, as opposed to the region of the specific string(s).

---

### `M_ITALIC_CHAR_ANGLE`

Retrieves the italic angle at which characters in the string(s) are found.

---

### `M_ITALIC_PITCH`

Retrieves the distance between successive dot centers in the italic angle direction.

---

### `M_STRING_ANGLE`

Retrieves the angle of the string's bounding box, or the bounding box encompassing all strings, in degrees. The angle is measured at the box's base and is relative to the output coordinate system ([`MdmrControl`](../../Reference/dmr/MdmrControl.md)with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dmr/MdmrControl.md)).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise, from the positive X-axis towards the negative Y-axis of the image. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

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

Retrieves the X-coordinate of the top-left corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_UL_Y`

Retrieves the Y-coordinate of the top-left corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_UR_X`

Retrieves the X-coordinate of the top-right corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_BOX_UR_Y`

Retrieves the Y-coordinate of the top-right corner of the bounding box of the string, or the bounding box encompassing all strings, relative to the angle at which the string was found.

---

### `M_STRING_CHAR_ANGLE`

Retrieves the angle at which characters in the string(s) are found.

---

### `M_STRING_HEIGHT`

Retrieves the height of the string's bounding box, or the bounding box encompassing all strings.

---

### `M_STRING_PITCH`

Retrieves the distance between successive dot centers in the string angle direction.

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

When retrieving results for characters in a string, set the [`ResultType`](../../Reference/dmr/MdmrGetResult.md) parameter to one of the values below. In this case, set the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter to a string (one or all), and set the [`CharPositionIndex`](../../Reference/dmr/MdmrGetResult.md) parameter to a character (one or all). Characters are ordered by their index position in their string.  > **Note:** Note that [`M_CHAR_NAME`](../../Reference/dmr/MdmrGetResult.md) cannot be used when the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter is set to [`M_ALL`](../../Reference/dmr/MdmrGetResult.md).

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

Retrieves the X-coordinate of the top-left corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_UL_Y`

Retrieves the Y-coordinate of the top-left corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_UR_X`

Retrieves the X-coordinate of the top-right corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_BOX_UR_Y`

Retrieves the Y-coordinate of the top-right corner of the character's bounding box, relative to the angle at which the character was found.

---

### `M_CHAR_FONT_INDEX`

Retrieves the index of the font, in the SureDotOCR context, that contains the representation of the character that was read in the string.

---

### `M_CHAR_FONT_LABEL`

Retrieves the label of the font, in the SureDotOCR context, that contains the representation of the character that was read in the string.

---

### `M_CHAR_HEIGHT`

Retrieves the height of the character's bounding box.

---

### `M_CHAR_NAME`

Retrieves the name of the character in the string that was read. The name is returned as a regular string. It consists of the character name and the terminating null character. If a space was read, the name returned is an actual space (and the terminating null character).

---

### `M_CHAR_POSITION_X`

Retrieves the X-coordinate of the center of the bounding box of the character; this position is considered the position of the character.

---

### `M_CHAR_POSITION_Y`

Retrieves the Y-coordinate of the center of the bounding box of the character; this position is considered the position of the character.

---

### `M_CHAR_SCORE`

Retrieves the character's score. This quantifies the similarity between the character in the target and the character in the font.  If SureDotOCR reads an [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) permitted character ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md)), its character score is always 100%. This score does not influence the string score ([`M_STRING_SCORE`](../../Reference/dmr/MdmrGetResult.md)).

---

### `M_CHAR_WIDTH`

Retrieves the width of the character's bounding box.

### Combination Constants — For retrieving the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Retrieves the number of characters in the string, including the terminating null character. When specifying [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md), the number returned also include spaces, if any were read in the target string. When specifying [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md), the number returned can also include spaces, but only if they correspond to a space permitted character constraint in the string model. When specifying [`M_CHAR_NAME`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md), the number returned can include a space, if that was the character read.  For [`M_CHAR_NAME`](../../Reference/dmr/MdmrGetResult.md), the name of each letter at each position is itself a string, with its own terminating null character. You must sum them all to get the required array size for a string.

### Combination Constants — For explicitly retrieving character name information in hexadecimal 16-bit Unicode (UTF-16) format

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the hexadecimal 16-bit Unicode (UTF-16) character name information.

The character name of the letter A is "A" (its native name) or "\x0041", which is its representation in hexadecimal format beginning with "\x". Characters between 0 and 127 are considered Basic Latin. A character such as the smiley face (☺, or "\x263A" in hexadecimal) is beyond the Basic Latin range.  In a Unicode environment, the character representations are returned (for example, the smiley face), unless you explicitly specify one of the below combination values (for example, to obtain "\x263A" instead of the actual smiley face). Retrieving character names in hexadecimal might be useful if you want to convert your data into ASCII.  Note how the string size is affected. For example, the string "abc☺" nominally consists of four characters plus the null-terminating character. Accordingly, a string size result (retrieved with, for example, [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md)) will return 5 as the string size. For the same string mentioned above ("abc☺"), when [`M_HEX_UTF16_FOR_ALL`](../../Reference/dmr/MdmrGetResult.md) is specified, you will obtain "\x0041\x0042\x0043\x263A" and a string size of 25. When [`M_HEX_UTF16_FOR_NON_BASIC_LATIN`](../../Reference/dmr/MdmrGetResult.md) is specified, you will obtain "abc\x263A" and a string size of 10.

| Value | Description |
| --- | --- |
| `M_HEX_UTF16_FOR_ALL` | Retrieves results with the name of all characters in hexadecimal 16-bit Unicode (UTF-16) format. |
| `M_HEX_UTF16_FOR_NON_BASIC_LATIN` | Retrieves results with the names of characters that fall out of the Basic Latin range, in hexadecimal 16-bit Unicode (UTF-16) format. |

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

> **Note:** Note that this result type cannot be used when the [`StringIndex`](../../Reference/dmr/MdmrGetResult.md) parameter is set to [`M_ALL`](../../Reference/dmr/MdmrGetResult.md).
