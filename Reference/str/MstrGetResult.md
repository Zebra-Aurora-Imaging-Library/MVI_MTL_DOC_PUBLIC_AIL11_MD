---
doctype: Reference
module: str
function: MstrGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrGetResult"
---

# MstrGetResult

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

> Get the specified type of result(s) from a String Reader result buffer.

## Syntax

```c
void MstrGetResult(
    AIL_ID    ResultId,       //in
    AIL_INT   Index,          //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a String Reader result buffer. Results are only available after calling [`MstrRead`](../../Reference/str/MstrRead.md).

When retrieving results, you will need to know the required type and size of the array in which to store the results. Typically, the default data type is _AIL_DOUBLE_. For certain results (such as, [`M_TEXT`](../../Reference/str/MstrGetResult.md), [`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md), and [`M_STRING`](../../Reference/str/MstrGetResult.md)), the default type of the required array depends on the selected encoding scheme ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md)). You can specify any data type by explicitly adding it to the result. The size of the array depends on the result type.

Unless otherwise specified, the value settings are applicable to both font based and fontless contexts.

If your target image ([`MstrRead`](../../Reference/str/MstrRead.md)) was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the center of top-left pixel in the target image.

If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MstrControl`](../../Reference/str/MstrControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/str/MstrControl.md) control type set to [`M_PIXEL`](../../Reference/str/MstrControl.md). However, if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/str/MstrControl.md) to [`M_WORLD`](../../Reference/str/MstrControl.md) without specifying a calibrated image in which to calculate the results, [`MstrGetResult`](../../Reference/str/MstrGetResult.md) generates an error.

## Parameters

### `ResultId` *(in, AIL_ID)*

Specifies the identifier of the String Reader result buffer from which to retrieve results.

### `Index` *(in, AIL_INT)*

Specifies where to get results. This parameter can be set to one of the following:

*For specifying where to get results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies that the results of all strings will be retrieved. |
| `M_GENERAL` | Specifies that the results relating to the entire String Reader context will be retrieved. |
| `0 <= Value < M_STRING_NUMBER` | Specifies the index of the string from which to get results. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write results.

## Parameter Associations

### For retrieving general results

When retrieving a general result ([`M_GENERAL`](../../Reference/str/MstrGetResult.md)), the [`ResultType`](../../Reference/str/MstrGetResult.md) parameter can be set to one of the following values.

---

### `M_CHAR_NUMBER`

Retrieves the total number of characters read (all strings together).

---

### `M_CONTEXT_ID`

Retrieves the identifier of the String Reader context used by [`MstrRead`](../../Reference/str/MstrRead.md) to obtain the results in the result buffer. Note that this identifier might not be valid if the String Reader context has been freed using [`MstrFree`](../../Reference/str/MstrFree.md).

---

### `M_ENCODING`

Retrieves the type of character encoding that [`MstrRead`](../../Reference/str/MstrRead.md) used to generate the contents of the result buffer.

| Value | Description |
| --- | --- |
| `M_ASCII` | Specifies that the characters were encoded with an 8-bit ASCII standard. |
| `M_UNICODE` | Specifies that the characters were encoded with a 16-bit Unicode standard. |

---

### `M_STRING_NUMBER`

Retrieves the total number of strings read.

---

### `M_TEXT`

Retrieves the entire text. This includes all string characters, the inserted space characters, the string separators, and the terminating null character ("\0").  Strings are ordered in the natural Latin-based reading order.  A space character is inserted in the text each time the distance between two adjacent characters exceeds the space width, at the scale of the string. The inserted space character can be set using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_CHARACTER`](../../Reference/str/MstrControl.md). By default, the inserted space character is a space (ASCII code 32). Note that a string separator is inserted between different strings when getting all strings simultaneously. The string separator can be set using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SEPARATOR`](../../Reference/str/MstrControl.md). By default, the string separator is a new line (ASCII code 10). Refer to [`M_STRING`](../../Reference/str/MstrGetResult.md) to retrieve the strings without separators. Refer to [`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md) to retrieve the formatted string.

---

### `M_TEXT_ALLOC_SIZE_BYTE`

Retrieves the number of bytes required to retrieve [`M_TEXT`](../../Reference/str/MstrGetResult.md) result(s). This includes all string characters, the inserted space characters, the string separators, the terminating null character ("\0"), and takes the encoding bit size into account ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md)). Note that the size returned is always the number of bytes, not the number of characters.

---

### `M_TIMEOUT_END`

Retrieves whether the timeout limit was reached. You can set the timeout limit using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_TIMEOUT`](../../Reference/str/MstrControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the timeout limit was not reached. |
| `M_TRUE` | Specifies that the timeout limit was reached. |

### For individual strings

When retrieving individual string results, the [`ResultType`](../../Reference/str/MstrGetResult.md) parameter can be set to one of the following values. Results can be retrieved for a specific string, or for all strings ([`M_ALL`](../../Reference/str/MstrGetResult.md)), unless otherwise specified. Note that strings are ordered in the natural Latin-based reading order.

---

### `M_CHAR_ANGLE`

Retrieves the angle of the characters in the string, in degrees. The angle is the same for all the characters in the string; different angles for individual characters within a string cannot be obtained. The characters' angle is not necessarily equal to the string's angle.  The angle is measured counter-clockwise, from the positive X-axis towards the negative Y-axis of the image.

---

### `M_FOREGROUND_VALUE`

Retrieves the foreground value of the string read.

---

### `M_FORMATTED_STRING`

Retrieves the formatted string. This includes all strings characters, the inserted space characters, and the terminating null character ("\0").  A space character is inserted in the formatted string each time the distance between two adjacent characters exceeds the space width, at the scale of the string. The inserted space character can be set using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_CHARACTER`](../../Reference/str/MstrControl.md). By default, the inserted space character is a space (ASCII code 32). Note that a space character is inserted between different strings when getting all strings simultaneously. Refer to [`M_STRING`](../../Reference/str/MstrGetResult.md) to retrieve the strings without separators. Refer to [`M_TEXT`](../../Reference/str/MstrGetResult.md) to retrieve the entire text.

---

### `M_SKEW_ANGLE`

Retrieves the angle of the characters' skew in the string, in degrees. The angle is the same for all the characters in the string; different angles for individual characters within a string cannot be obtained.

---

### `M_STRING`

Retrieves the null terminated string found during the read operation. The string does not contain any space characters or string separators.  Refer to [`M_CHAR_VALUE`](../../Reference/str/MstrGetResult.md) to retrieve all characters of all strings without the terminating null character ("\0"). Refer to [`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md) or [`M_TEXT`](../../Reference/str/MstrGetResult.md) to retrieve all strings with their space characters and/or string separators.

---

### `M_STRING_ALLOC_SIZE_BYTE`

Retrieves the number of bytes required to retrieve [`M_STRING`](../../Reference/str/MstrGetResult.md) result(s). This includes the terminating null character ("\0"), and takes the encoding bit size into account ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md)). [`M_STRING_ALLOC_SIZE_BYTE`](../../Reference/str/MstrGetResult.md) is typically used when retrieving all results ([`M_ALL`](../../Reference/str/MstrGetResult.md)), and will return an array of string size byte (1 element for each string). Note that the size is always the number of bytes, not the number of characters.

---

### `M_STRING_ANGLE`

Retrieves the angle of the string, in degrees.  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise, from the positive X-axis towards the negative Y-axis of the image. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

---

### `M_STRING_ASPECT_RATIO`

Retrieves the aspect ratio of the string. The aspect ratio of the string is the median aspect ratio of all the characters in the string.

---

### `M_STRING_BOX_BL_X`

Retrieves the X-position of the bottom-left corner of the bounding box of the string.

---

### `M_STRING_BOX_BL_Y`

Retrieves the Y-position of the bottom-left corner of the bounding box of the string.

---

### `M_STRING_BOX_BR_X`

Retrieves the X-position of the bottom-right corner of the bounding box of the string.

---

### `M_STRING_BOX_BR_Y`

Retrieves the Y-position of the bottom-right corner of the bounding box of the string.

---

### `M_STRING_BOX_UL_X`

Retrieves the X-position of the top-left corner of the bounding box of the string.

---

### `M_STRING_BOX_UL_Y`

Retrieves the Y-position of the top-left corner of the bounding box of the string.

---

### `M_STRING_BOX_UR_X`

Retrieves the X-position of the top-right corner of the bounding box of the string.

---

### `M_STRING_BOX_UR_Y`

Retrieves the Y-position of the top-right corner of the bounding box of the string.

---

### `M_STRING_CHAR_NUMBER`

Retrieves the number of characters in the string found during the read operation, not including the terminating null character ("\0"); this means it returns the number of characters with results. In addition, space characters are not counted in the string size.

---

### `M_STRING_CHAR_SCORE_MAX`

Retrieves the maximum character score of the string.

---

### `M_STRING_CHAR_SCORE_MIN`

Retrieves the minimum character score of the string.

---

### `M_STRING_MODEL_INDEX`

Retrieves the index of the string model read.

---

### `M_STRING_MODEL_USER_LABEL`

Retrieves the user label of the string model read.

---

### `M_STRING_POSITION_X`

Retrieves the X-position of the string. The position of the string is the position of its first character. For more information, see [`M_CHAR_POSITION_X`](../../Reference/str/MstrGetResult.md).

---

### `M_STRING_POSITION_Y`

Retrieves the Y-position of the string. The position of the string is the position of its first character. For more information, see [`M_CHAR_POSITION_Y`](../../Reference/str/MstrGetResult.md).

---

### `M_STRING_SCALE`

Retrieves the scale of the string. The scale of the string is the median scale in the X-direction of all the characters in the string.

---

### `M_STRING_SCORE`

Retrieves the string score calculated during the read operation for the entire string. The string score is the average score of the characters in the string.

---

### `M_STRING_TARGET_SCORE`

Retrieves the string target score calculated during the read operation for the entire string.  The string target score is computed from the unread characters that are in the region of the string in the target image. These otherwise readable characters remain unread due to user-specified constraints, such as scale and grammar restrictions. The more unread characters there are, the lower the target score will be.

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length..

#### `M_STRING_SIZE`

Retrieves the length of one or more strings, including the terminating null character ("\0"). In the case of [`M_TEXT`](../../Reference/str/MstrGetResult.md), it also includes string separators. This combination constant is useful to establish the required array size to retrieve the string.

### For individual characters of a string

When retrieving results for individual characters of a string, the [`ResultType`](../../Reference/str/MstrGetResult.md) parameter can be set to one of the following values. Results can be returned for characters of a specific string or for characters of all ([`M_ALL`](../../Reference/str/MstrGetResult.md)) strings. However, an array of results is always returned. Note that characters in the string are ordered by their index position in the string.

---

### `M_CHAR_ASPECT_RATIO`

Retrieves the aspect ratio of the character. The aspect ratio of the character is the ratio of its scale in the X-direction and in the Y-direction.

---

### `M_CHAR_BASELINE_DEVIATION`

Retrieves the baseline deviation of every read character within the string. This represents the deviation of the character's baseline from the string's baseline. For non-punctuation characters, this value is a percentage of the character's height (Y-size). For punctuation characters, this value is a percentage of the height of the tallest character within the font. For more information, refer to [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_CHAR_MAX_BASELINE_DEVIATION`](../../Reference/str/MstrControl.md) and [Character's maximum baseline deviation](../../UserGuide/C14_String_Reader/S09_Degrees_of_freedom.md).  Note that if the character's baseline is set to [`M_NONE`](../../Reference/str/MstrEditFont.md), its deviation will always be returned as '0'.

---

### `M_CHAR_BOX_BL_X`

Retrieves the X-position of the bottom-left corner of the bounding box of each character.

---

### `M_CHAR_BOX_BL_Y`

Retrieves the Y-position of the bottom-left corner of the bounding box of each character.

---

### `M_CHAR_BOX_BR_X`

Retrieves the X-position of the bottom-right corner of the bounding box of each character.

---

### `M_CHAR_BOX_BR_Y`

Retrieves the Y-position of the bottom-right corner of the bounding box of each character.

---

### `M_CHAR_BOX_UL_X`

Retrieves the X-position of the top-left corner of the bounding box of each character.

---

### `M_CHAR_BOX_UL_Y`

Retrieves the Y-position of the top-left corner of the bounding box of each character.

---

### `M_CHAR_BOX_UR_X`

Retrieves the X-position of the top-right corner of the bounding box of each character.

---

### `M_CHAR_BOX_UR_Y`

Retrieves the Y-position of the top-right corner of the bounding box of each character.

---

### `M_CHAR_CONSECUTIVE_SPACE`

Retrieves the number of consecutive spaces that can be inserted between this character and the following one in the string. For more information, refer to [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md) and [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md) in [`MstrControl`](../../Reference/str/MstrControl.md).

---

### `M_CHAR_FONT`

Retrieves the index of the font of each individual character within the string(s). The index returned is that of the character's font at the moment the [`MstrRead`](../../Reference/str/MstrRead.md) operation was performed.

---

### `M_CHAR_FONT_USER_LABEL`

Retrieves the user label of the font of each individual character within the string(s).

---

### `M_CHAR_HOMOGENEITY_SCORE`

Retrieves the homogeneity score of each individual character within the string(s). The character's homogeneity score quantifies the similarity between the character in the target and the other characters of the string in the target.

---

### `M_CHAR_INDEX`

Retrieves the index of the character in the font for each individual character within the string(s). The index returned is that of the index of the character in its font at the moment the [`MstrRead`](../../Reference/str/MstrRead.md) operation was performed.

---

### `M_CHAR_POSITION_X`

Retrieves the X-position of the character. The position of a character is the position of its center of gravity.

---

### `M_CHAR_POSITION_Y`

Retrieves the Y-position of the character. The position of a character is the position of its center of gravity.

---

### `M_CHAR_SCALE`

Retrieves the scale of the character. The scale of the character is its scale in the X-direction.

---

### `M_CHAR_SCORE`

Retrieves the score of each individual character within the string(s). The character's score quantifies the similarity between the character in the target and the character in the font; it also quantifies the similarity between the character in the target and the other characters of the string in the target.

---

### `M_CHAR_SIMILARITY_SCORE`

Retrieves the similarity score of each individual character within the string(s). The character's similarity score quantifies the similarity between the character in the target and the character in the font.

---

### `M_CHAR_VALUE`

Retrieves the value of each character read.

### For retrieving transformation coefficient results for individual characters of a string with a font-based context

When retrieving transformation coefficient results for individual characters of a string, the [`ResultType`](../../Reference/str/MstrGetResult.md) parameter can be set to one of the following values.  Transformation coefficients allow you to transform any point from the character's source image (the image from which the characters were defined) to its corresponding position in the target image (forward), or from the read position in the target image to its corresponding position in the character's source image (reverse). This is done using the following equations:  `_x_ <sub>d</sub> = _A_ _x_ <sub>s</sub> + _B_ _y_ <sub>s</sub> + _C_`  `_y_ <sub>d</sub> = _D_ _x_ <sub>s</sub> + _E_ _y_ <sub>s</sub> + _F_`  where `_A_, _B_, _C_, _D_, _E_, and _F_` are the transformation coefficients (forward or reverse); `x <sub>s</sub> and y<sub> s</sub>` specify the source coordinates (character image for forward transformation or target image for reverse transformation); and, `x <sub>d</sub> and y<sub> d</sub>` specify the destination coordinates (target image for forward transformation or character image for reverse transformation).

---

### `M_A_FORWARD`

Retrieves the forward transformation coefficient A.

---

### `M_A_REVERSE`

Retrieves the reverse transformation coefficient A.

---

### `M_B_FORWARD`

Retrieves the forward transformation coefficient B.

---

### `M_B_REVERSE`

Retrieves the reverse transformation coefficient B.

---

### `M_C_FORWARD`

Retrieves the forward transformation coefficient C.

---

### `M_C_REVERSE`

Retrieves the reverse transformation coefficient C.

---

### `M_D_FORWARD`

Retrieves the forward transformation coefficient D.

---

### `M_D_REVERSE`

Retrieves the reverse transformation coefficient D.

---

### `M_E_FORWARD`

Retrieves the forward transformation coefficient E.

---

### `M_E_REVERSE`

Retrieves the reverse transformation coefficient E.

---

### `M_F_FORWARD`

Retrieves the forward transformation coefficient F.

---

### `M_F_REVERSE`

Retrieves the reverse transformation coefficient F.

### For retrieving errors or warnings reported by the last call to MstrExpert()

When retrieving errors or warnings reported by the last call to [`MstrExpert`](../../Reference/str/MstrExpert.md), the [`ResultType`](../../Reference/str/MstrGetResult.md) parameter can be set to one of the following values.  To retrieve general errors or warnings (relating to the entire String Reader context), the [`Index`](../../Reference/str/MstrGetResult.md) parameter must be set to [`M_GENERAL`](../../Reference/str/MstrGetResult.md). To retrieve errors or warnings for a specific string, or for all strings, the [`Index`](../../Reference/str/MstrGetResult.md) parameter must be set to the string's index, or to [`M_ALL`](../../Reference/str/MstrGetResult.md).

---

### `M_REPORT_ERRORS`

Retrieves a list of all the errors reported by the last call to [`MstrExpert`](../../Reference/str/MstrExpert.md).  To inquire the string associated with an error, use [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_REPORT_STRING`](../../Reference/str/MstrInquire.md).

---

### `M_REPORT_NUMBER_OF_ERRORS`

Retrieves the number of errors reported by the last call to [`MstrExpert`](../../Reference/str/MstrExpert.md).

---

### `M_REPORT_NUMBER_OF_WARNINGS`

Retrieves the number of warnings reported by the last call to [`MstrExpert`](../../Reference/str/MstrExpert.md).

---

### `M_REPORT_WARNINGS`

Retrieves a list of all the warnings reported by the last call to [`MstrExpert`](../../Reference/str/MstrExpert.md).  To inquire the string associated with a warning, use [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_REPORT_STRING`](../../Reference/str/MstrInquire.md).

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

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

#### `M_TYPE_CHAR`

Casts the requested results to a _char_.

#### `M_TYPE_TEXT_CHAR`

Casts the requested results to an _AIL_TEXT_CHAR_.

Note that this control type is only available for a font-based context.
