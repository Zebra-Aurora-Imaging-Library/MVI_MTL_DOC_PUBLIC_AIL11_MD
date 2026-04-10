---
doctype: Reference
module: str
function: MstrControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrControl"
---

# MstrControl

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

> Control a String Reader context, result buffer, a specific string model, or a specific font.

## Syntax

```c
void MstrControl(
    AIL_ID     ContextOrResultId,  //out
    AIL_INT    Index,              //in
    AIL_INT64  ControlType,        //in
    AIL_DOUBLE ControlValue        //in
)
```

## Description

This function sets the specified control for a String Reader context, for one (or all) of the specified string models or fonts contained therein, or a result buffer. These settings control the execution of [`MstrRead`](../../Reference/str/MstrRead.md) operations. All the control type settings can be inquired using [`MstrInquire`](../../Reference/str/MstrInquire.md).

Changing certain control type settings might require preprocessing the String Reader context again, using [`MstrPreprocess`](../../Reference/str/MstrPreprocess.md). To know if the String Reader context needs to be preprocessed, call [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_PREPROCESSED`](../../Reference/str/MstrInquire.md).

Unless otherwise specified, the control type settings are applicable to both font-based and fontless contexts.

## Parameters

### `ContextOrResultId` *(out, AIL_ID)*

Specifies the String Reader context or result buffer whose settings to modify. The String Reader context must have been previously allocated on the system using [`MstrAlloc`](../../Reference/str/MstrAlloc.md). The String Reader result buffer must have been previously allocated on the system using [`MstrAllocResult`](../../Reference/str/MstrAllocResult.md).

### `Index` *(in, AIL_INT)*

Specifies that the String Reader context, a specific font, a specific string model, or a result buffer is controlled. Set this parameter to one of the following values.

*For a String Reader context, specific font, specific string model, or result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default.

If a String Reader context is specified, same as [`M_CONTEXT`](../../Reference/str/MstrControl.md).

If a String Reader result buffer is specified, same as [`M_GENERAL`](../../Reference/str/MstrControl.md). |
| `M_FONT_INDEX` | Specifies to apply the control settings to a font.

Note that this value is only available for a font-based context. |
| `M_STRING_INDEX` | Specifies to apply the control settings to a string. |
| `M_CONTEXT` | Specifies that a general setting of a String Reader context will be controlled. |
| `M_GENERAL` | Specifies that a general setting of a String Reader result buffer will be controlled, if one is specified. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For the context or a specific string

The following [`ControlType`](../../Reference/str/MstrControl.md) and corresponding [`ControlValue`](../../Reference/str/MstrControl.md) parameter settings can be specified for either the String Reader context ([`M_CONTEXT`](../../Reference/str/MstrControl.md)) or each specific string in the String Reader context (**M_STRING_INDEX()**).

---

### `M_STRING_NUMBER`

Sets either the maximum (total) number of all strings that can be read with a String Reader context ([`M_CONTEXT`](../../Reference/str/MstrControl.md)), or the maximum number of a specific string (**M_STRING_INDEX()**) that can be read with a String Reader context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies that all strings will be read.  Note that this setting can increase the read time; always set [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) to a specific number whenever possible. |
| `Value` *(default)* | Specifies the maximum number of strings that can be read.  For [`M_CONTEXT`](../../Reference/str/MstrControl.md), this value specifies the maximum (total) number of strings that can be read. Any value greater than or equal to 1 can be set.  For **M_STRING_INDEX()**, this value specifies the maximum number of a specific string model that can be read. Any value greater than or equal to 0 can be set. |

### For the context

The possible [`ControlType`](../../Reference/str/MstrControl.md) and corresponding [`ControlValue`](../../Reference/str/MstrControl.md) parameter settings for the String Reader context ([`M_CONTEXT`](../../Reference/str/MstrControl.md)) are:

---

### `M_DISABLE_CHAR`

Disables the specified character in the fontless context. Only uppercase characters and numbers can be disabled.  The String Reader module will not try to match found characters with disabled characters.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that no characters will be read. |
| `ASCII value` | Specifies the ASCII value of the character to disable. |

---

### `M_ENABLE_CHAR`

Enables the specified character in the fontless context. Only uppercase characters and numbers can be enabled.  The String Reader module will only try to identify enabled characters.

| Value | Description |
| --- | --- |
| `M_ALL` *(default)* | Specifies that all characters will be read. |
| `ASCII value` | Specifies the ASCII value of the character. |

---

### `M_ENCODING`

Sets the type of character encoding used by the String Reader context.  All functions that accept or return a string of characters as a parameter use character encoding to determine the type of the pointer. The type of character encoding is also used as the default result type ([`MstrGetResult`](../../Reference/str/MstrGetResult.md)) for a string of characters.  The character encoding cannot be changed when there are characters in the context whose ASCII character values lie outside the range [0, 127]. This applies to characters which are in fonts and constraints.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASCII` *(default)* | Specifies an 8-bit ASCII standard. This corresponds to the _char_ data type. |
| `M_UNICODE` | Specifies a 16-bit Unicode standard. This corresponds to the _short_ data type. |

---

### `M_FONT_ADD`

Adds an empty font to the String Reader context. The maximum number of fonts in a String Reader context is 255.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED` *(default)* | Specifies a user-defined type of font. Initially, the font is empty. To add characters to the font, or to edit it, use [`MstrEditFont`](../../Reference/str/MstrEditFont.md). |

---

### `M_FONT_DELETE`

Deletes a font from the String Reader context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies that all fonts will be deleted. |
| `Value >= 0` | Specifies the index of the font to delete. |

---

### `M_MAX_CHAR_SIZE_X`

Sets the width (X-size) of the widest enabled character in the fontless context. Specify the width that the character has when it has a height of [`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md).  If the read algorithm encounters a character with a height other than [`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md), the maximum width will be interpolated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 65536` *(default)* | Specifies the maximum character width, in pixels. |

---

### `M_MIN_CHAR_SIZE_X`

Sets the width (X-size) of the narrowest enabled character in the fontless context. Specify the width that the character has when it has a height of [`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md).  If the read algorithm encounters a character with a height other than [`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md), the minimum width will be interpolated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 65536` *(default)* | Specifies the minimum character width, in pixels. |

---

### `M_MINIMUM_CONTRAST`

Sets the minimum contrast between a character in the target image and its background, in order for the character to be read by [`MstrRead`](../../Reference/str/MstrRead.md).  Increasing the minimum contrast will typically speed up the read operation, particularly in presence of noise; however, if the minimum contrast is higher than the contrast of an actual character, that character will be lost.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 255` *(default)* | Specifies the minimum contrast. |

---

### `M_REF_CHAR_SIZE_Y`

Sets the height which will be the reference for [`M_MIN_CHAR_SIZE_X`](../../Reference/str/MstrControl.md), [`M_MAX_CHAR_SIZE_X`](../../Reference/str/MstrControl.md), and [`M_REF_CHAR_THICKNESS`](../../Reference/str/MstrControl.md).  The reference height is the height at which you specify the width of the narrowest and widest characters, as well as their thickness, with [`M_MIN_CHAR_SIZE_X`](../../Reference/str/MstrControl.md), [`M_MAX_CHAR_SIZE_X`](../../Reference/str/MstrControl.md), and [`M_REF_CHAR_THICKNESS`](../../Reference/str/MstrControl.md), respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 65536` *(default)* | Specifies the height, in pixels. |

---

### `M_REF_CHAR_THICKNESS`

Sets the thickness (stroke width) of enabled characters in the fontless context. Specify the thickness of characters when they have a height of[`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md).  If the read algorithm encounters a character with a height other than [`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md), the maximum width will be interpolated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 65536` *(default)* | Specifies the thickness, in pixels. |

---

### `M_SEARCH_CHAR_ANGLE`

Sets whether to perform calculations specific to angular-range search strategies, for characters.  The read algorithm is naturally robust to variations in a character's angle. Therefore, a character might still be read correctly, even if [`M_SEARCH_CHAR_ANGLE`](../../Reference/str/MstrControl.md) is disabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to perform calculations specific to angular-range search strategies. |
| `M_ENABLE` | Specifies to perform calculations specific to angular-range search strategies. |

---

### `M_SEARCH_SKEW_ANGLE`

Sets whether to perform calculations specific to angular-range skew search strategies, for characters.  The read algorithm is naturally robust to variations in a character's skew. Therefore, a character might still be read correctly, even if [`M_SEARCH_SKEW_ANGLE`](../../Reference/str/MstrControl.md) is disabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to perform calculations specific to angular-range skew search strategies. |
| `M_ENABLE` | Specifies to perform calculations specific to angular-range skew search strategies. |

---

### `M_SEARCH_STRING_ANGLE`

Sets whether to search for the string angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to search for the string angle. |
| `M_ENABLE` *(default)* | Specifies to search for the string angle. |

---

### `M_SPACE_CHARACTER`

Sets the character value to use as a space character within the formatted string or text that will be read.  [`M_SPACE_CHARACTER`](../../Reference/str/MstrControl.md) applies only when getting the formatted string ([`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md)) or the formatted text ([`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_TEXT`](../../Reference/str/MstrGetResult.md)). A space character is inserted in the string each time the distance between two adjacent characters exceeds the space width, at the scale of the string.  The space character must be set before the string is read.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` | Specifies no space character. |
| `Value` *(default)* | Specifies the space character, as its numerical code (ASCII or Unicode). For example, ASCII 10 results in a new line character, ASCII 32 results in a space character. Note that the character value must be cast to an _AIL_DOUBLE_. |

---

### `M_SPEED`

Sets the search and read speed of the [`MstrRead`](../../Reference/str/MstrRead.md) operation. Note that increasing the speed can decrease the robustness of [`MstrRead`](../../Reference/str/MstrRead.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies a high speed. |
| `M_MEDIUM` *(default)* | Specifies a medium speed. |

---

### `M_STOP_EXPERT`

Stops the current string expert operation.  Note that you can only perform this operation from another thread of higher priority.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_STOP_READ`

Stops the current string read operation.  Note that you can only perform this operation from another thread of higher priority.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_STRING_ADD`

Adds a string model to the String Reader context. The maximum number of string models in a String Reader context is 255.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED` *(default)* | Specifies a user-defined type of string model. Initially, a user-defined string model does not have any constraints, does not use any checksum error detection, and all its parameters are set to their default value. |

---

### `M_STRING_DELETE`

Deletes a string from the String Reader context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies that all strings will be deleted. |
| `Value >= 0` | Specifies the index of the string to delete. |

---

### `M_STRING_SEPARATOR`

Sets the character value to use as a string separator within the formatted text that will be read.  [`M_STRING_SEPARATOR`](../../Reference/str/MstrControl.md) applies only when getting the formatted text ([`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_TEXT`](../../Reference/str/MstrGetResult.md)). A string separator is inserted between each string in the text; that is, each time the distance between two adjacent characters is greater than the space size. The space size depends on the space width (at the scale of the string) and the maximum number of consecutive spaces.  The string separator character must be set before the string is read.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` | Specifies no separator character. |
| `Value` *(default)* | Specifies the separator character, as its numerical code (ASCII or Unicode). For example, ASCII 10 results in a new line character, ASCII 32 results in a space character. Note that the character value must be cast to an _AIL_DOUBLE_. |

---

### `M_THICKEN_CHAR`

Sets the number of character thickening iterations.  Each iteration enlarges the thickness of the target character. This is useful for some types of fonts (for example, dotted characters). You should choose a value large enough for the intra character dots to connect but not too large to connect separate characters together.  Note that the characters included in the font should not be dotted. Instead a thickened version should be included.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 100` *(default)* | Specifies the number of character thickening iterations. |

---

### `M_THRESHOLD_MODE`

Sets the threshold method of the character blob extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LOCAL` | Specifies a local thresholding that uses only one threshold step that is automatically computed by the module. This threshold method can be used in various contrast situations and can improve the read operation's speed, compared to [`M_LOCAL_WITH_RESEGMENTATION`](../../Reference/str/MstrControl.md). |
| `M_LOCAL_WITH_RESEGMENTATION` *(default)* | Specifies a local thresholding that uses multiple threshold steps and split and merge techniques. This threshold method allows the module to extract character blobs in low contrast and in complex situations such as merged or broken characters. |
| `M_USER_DEFINED` | Specifies a global thresholding that uses the threshold value set with [`M_THRESHOLD_VALUE`](../../Reference/str/MstrControl.md). This threshold method improves the read operation's speed, compared to [`M_LOCAL_WITH_RESEGMENTATION`](../../Reference/str/MstrControl.md) and [`M_LOCAL`](../../Reference/str/MstrControl.md). |

---

### `M_THRESHOLD_VALUE`

Sets the global threshold value when [`M_THRESHOLD_MODE`](../../Reference/str/MstrControl.md) is set to [`M_USER_DEFINED`](../../Reference/str/MstrControl.md) (otherwise it is ignored).  > **Note:** Note that two different threshold values, which should segment the target image in exactly the same way, might produce different results, even when using a binarized target image. This is because during the read operation, the image is occasionally resized or rotated internally which can cause pixels to be interpolated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_COMPUTE` *(default)* | Specifies to automatically compute the global threshold value. |
| `0 <= Value <= 255` | Specifies the threshold value. |

---

### `M_TIMEOUT`

Sets the maximum read time for [`MstrRead`](../../Reference/str/MstrRead.md), in msec. If the read operation times out, the strings read up to that point will be returned as a result.  Note that the actual maximum read time might vary slightly from the time that you set.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies an infinite amount of read time. |
| `Value >= 0` *(default)* | Specifies the maximum read time, in msec. |

### For a specific string

The possible [`ControlType`](../../Reference/str/MstrControl.md) and corresponding [`ControlValue`](../../Reference/str/MstrControl.md) parameter settings for a specific string (**M_STRING_INDEX()**) are:

---

### `M_CHAR_ACCEPTANCE`

Sets the acceptance level for the character score.  The character's score quantifies the similarity between the character in the target and the character in the font; it also quantifies the similarity between the character in the target and the other characters of the string in the target.  To be accepted as a string result, the character's score must be greater than or equal to [`M_CHAR_ACCEPTANCE`](../../Reference/str/MstrControl.md).  Scores can be retrieved with [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

---

### `M_CHAR_ASPECT_RATIO_MAX_FACTOR`

Sets the factor used to determine the maximum acceptable aspect ratio for the characters in the string. This value is relative to the found string aspect ratio. That is, _UpperLimit_ = _FoundStringAspectRatio_ x [`M_CHAR_ASPECT_RATIO_MAX_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the maximum possible aspect ratio variation for the characters in the string. Characters with an aspect ratio outside the aspect ratio range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 2.0` *(default)* | Specifies the factor that determines the maximum aspect ratio of the characters in the string. |

---

### `M_CHAR_ASPECT_RATIO_MIN_FACTOR`

Sets the factor used to determine the minimum acceptable aspect ratio for the characters in the string. This value is relative to the found string aspect ratio. That is, _LowerLimit_ = _FoundStringAspectRatio_ x [`M_CHAR_ASPECT_RATIO_MIN_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the minimum possible aspect ratio variation for the characters in the string. Characters with an aspect ratio outside the aspect ratio range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.5 <= Value <= 1.0` *(default)* | Specifies the factor that determines the minimum aspect ratio of the characters in the string. |

---

### `M_CHAR_HOMOGENEITY_ACCEPTANCE`

Sets the acceptance level for the character's homogeneity score.  The character's homogeneity score quantifies the similarity between the character in the target and the other characters of the string in the target.  To be accepted as a string result, the character's homogeneity score must be greater than or equal to [`M_CHAR_HOMOGENEITY_ACCEPTANCE`](../../Reference/str/MstrControl.md).  Scores can be retrieved with [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

---

### `M_CHAR_MAX_BASELINE_DEVIATION`

Sets the maximum baseline deviation that all characters defined in the font can have in the target image.  A character's maximum baseline deviation is the maximum tolerable baseline deviation that a character in a string can have before it is rejected from the string. For more information on character baseline deviations, see [Character's maximum baseline deviation](../../UserGuide/C14_String_Reader/S09_Degrees_of_freedom.md).  Note that for non-punctuation characters, String Reader calculates the baseline deviation, as a percentage of each individual character's height (Y-size). For example, a baseline deviation of 10 means that a character's baseline deviates from the string's baseline by 10% of the character's height (Y-size). For punctuation characters ([`M_PUNCTUATION`](../../Reference/str/MstrEditFont.md)), String Reader calculates the baseline deviation as a percentage of the height of the character with the greatest Y-size in the font. For example, a baseline deviation of 10 means that the punctuation character's baseline deviates from the string's baseline by 10% of the height of the tallest character in the font. This is done because the Y-size of punctuation characters is typically too small to calculate the baseline deviation as a percentage of the character's height.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 100` *(default)* | Specifies the character's maximum baseline deviation within the string model. |

---

### `M_CHAR_SCALE_MAX_FACTOR`

Sets the factor used to determine the upper limit (maximum permitted scale) of the scale range for the characters in the string. This value is relative to the found string scale. That is, _UpperLimit_ = _FoundStringScale_ x [`M_CHAR_SCALE_MAX_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the possible increasing scale variation for the characters in the string. Characters with a scale outside the scale range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 2.0` *(default)* | Specifies the factor that determines the maximum scale of the characters in the string. |

---

### `M_CHAR_SCALE_MIN_FACTOR`

Sets the factor used to determine the lower limit (minimum permitted scale) of the scale range for the characters in the string. This value is relative to the found string scale. That is, _LowerLimit_ = _FoundStringScale_ x [`M_CHAR_SCALE_MIN_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the possible decreasing scale variation for the characters in the string. Characters with a scale outside the scale range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.5 <= Value <= 1.0` *(default)* | Specifies the factor that determines the minimum scale of the characters in the string. |

---

### `M_CHAR_SIMILARITY_ACCEPTANCE`

Sets the acceptance level for the character's similarity score.  The character's similarity score quantifies the similarity between the character in the target and the character in the font.  To be accepted as a string result, the character's similarity score must be greater than or equal to [`M_CHAR_SIMILARITY_ACCEPTANCE`](../../Reference/str/MstrControl.md).  Scores can be retrieved with [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

---

### `M_FOREGROUND_VALUE`

Sets the foreground color of the string.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FOREGROUND_BLACK` *(default)* | Specifies that black is the foreground color of the string. |
| `M_FOREGROUND_BLACK_OR_WHITE` | Specifies that the foreground color of the string can be either black or white. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color of the string. |

---

### `M_SPACE_MAX_CONSECUTIVE`

Sets the maximum number of consecutive space characters allowed in the string.  Together with the space width ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md)), [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md) determines the maximum distance between 2 adjacent characters of the same string. If the distance between two characters is greater than the maximum distance, then they are considered to be part of 2 different strings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of consecutive space characters. |

---

### `M_STRING_ACCEPTANCE`

Sets the acceptance level for the string score.  To be accepted as a string result, both the string's score and the string target score must be greater than or equal to their respective acceptance levels.  The string score is the average score of the characters in the string.  Scores can be retrieved with [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

---

### `M_STRING_ANGLE`

Sets the nominal angle of the string.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_REGION` *(default)* | Specifies to set the nominal angle to the angle of the rectangular ROI (set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)) in the image.  If no region of interest has been defined, the default value is 0.0°. This implies that to read a string at an angle, the target image must have a rectangular ROI defined around the string to be read, and that ROI should be at an angle. |

---

### `M_STRING_ANGLE_DELTA_NEG`

Sets the lower limit of the string's angular range, relative to the nominal angle ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_ANGLE`](../../Reference/str/MstrControl.md)). That is, _LowerLimit_ = [`M_STRING_ANGLE`](../../Reference/str/MstrControl.md)  - [`M_STRING_ANGLE_DELTA_NEG`](../../Reference/str/MstrControl.md).  This value sets the possible clockwise angular variation of the string. Strings with angles outside the angular range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 10.0` *(default)* | Specifies the lower limit of the angular range, in degrees. |

---

### `M_STRING_ANGLE_DELTA_POS`

Sets the upper limit of the angular range, relative to the nominal angle ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_ANGLE`](../../Reference/str/MstrControl.md)). That is, _UpperLimit_ = [`M_STRING_ANGLE`](../../Reference/str/MstrControl.md)  + [`M_STRING_ANGLE_DELTA_POS`](../../Reference/str/MstrControl.md).  This value sets the possible counter-clockwise angular variation of the string. Strings with angles outside the angular range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 10.0` *(default)* | Specifies the upper limit of the angular range, in degrees. |

---

### `M_STRING_ASPECT_RATIO`

Sets the nominal aspect ratio of the string.  The aspect ratio of the string is the median aspect ratio of all the characters in the string, while the aspect ratio of a character is the ratio of its scale in X by its scale in Y. This indicates how the width and the height of the characters in the string vary relative to the characters in the font.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.5 <= Value <= 2.0` *(default)* | Specifies the ratio of pixel width to pixel height of the target. |

---

### `M_STRING_ASPECT_RATIO_MAX_FACTOR`

Sets the factor used to determine the upper limit of the string's aspect ratio. This value is relative to the nominal aspect ratio ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_ASPECT_RATIO`](../../Reference/str/MstrControl.md)). That is, _UpperLimit_ = [`M_STRING_ASPECT_RATIO`](../../Reference/str/MstrControl.md)  x [`M_STRING_ASPECT_RATIO_MAX_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the possible increasing aspect ratio variation for the string. Strings with an aspect ratio outside the aspect ratio range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 2.0` *(default)* | Specifies the factor that determines the maximum aspect ratio of the string. |

---

### `M_STRING_ASPECT_RATIO_MIN_FACTOR`

Sets the factor used to determine the lower limit of the string's aspect ratio. This value is relative to the nominal aspect ratio ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_ASPECT_RATIO`](../../Reference/str/MstrControl.md)). That is, _LowerLimit_ = [`M_STRING_ASPECT_RATIO`](../../Reference/str/MstrControl.md)  x [`M_STRING_ASPECT_RATIO_MIN_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the possible decreasing aspect ratio variation for the string. Strings with an aspect ratio outside the aspect ratio range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.5 <= Value <= 1.0` *(default)* | Specifies the factor that determines the minimum aspect ratio of the string. |

---

### `M_STRING_CERTAINTY`

Sets the certainty level for the string score.  If both the string score and the string target score of a string candidate are above their respective certainty levels, the candidate is immediately considered a string result, without reading the rest of the target image for a string with higher scores (provided the specified number of strings to read has been read).  Scores can be retrieved with [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the string score, as a percentage. |

---

### `M_STRING_SCALE`

Sets the nominal scale of the string.  The scale of the string is the median scale in the X-direction of all the characters in the string.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.25 <= Value <= 4.0` *(default)* | Specifies the value of the scale. |

---

### `M_STRING_SCALE_MAX_FACTOR`

Sets the factor used to determine the upper limit (maximum permitted scale) of the string's scale range. This value is relative to the nominal scale ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SCALE`](../../Reference/str/MstrControl.md)). That is, _UpperLimit_ = [`M_STRING_SCALE`](../../Reference/str/MstrControl.md)  x [`M_STRING_SCALE_MAX_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the possible increasing scale variation for the string. Strings with a scale outside the scale range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 2.0` *(default)* | Specifies the factor that determines the minimum scale of the string. |

---

### `M_STRING_SCALE_MIN_FACTOR`

Sets the factor used to determine the lower limit (minimum permitted scale) of the string's scale range. This value is relative to the nominal scale ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SCALE`](../../Reference/str/MstrControl.md)). That is, _LowerLimit_ = [`M_STRING_SCALE`](../../Reference/str/MstrControl.md)  x [`M_STRING_SCALE_MIN_FACTOR`](../../Reference/str/MstrControl.md).  This value sets the possible decreasing scale variation for the string. Strings with a scale outside the scale range will not be returned as results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.5 <= Value <= 1.0` *(default)* | Specifies the factor that determines the minimum scale of the string. |

---

### `M_STRING_SIZE_MAX`

Sets the maximum string size (number of characters) of the string model.  The space characters are not counted in the string size. The maximum string size must be greater than or equal to the minimum string size ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SIZE_MIN`](../../Reference/str/MstrControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no maximum string size. |
| `Value >= 1` | Specifies the maximum string size. |

---

### `M_STRING_SIZE_MIN`

Sets the minimum string size (number of characters) of the string model.  The space characters are not counted in the string size. The minimum string size must be lower than or equal to the maximum string size ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SIZE_MAX`](../../Reference/str/MstrControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the minimum string size. |

---

### `M_STRING_TARGET_ACCEPTANCE`

Sets the acceptance level for the string target score.  To be accepted as a string result, both the string's score and the string target score must be greater than or equal to their respective acceptance levels.  The string target score is computed from the unread characters that are in the region of the string in the target image. These otherwise readable characters remain unread due to user-specified constraints, such as scale and grammar restrictions. The more unread characters there are, the lower the target score will be.  Scores can be retrieved with [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable string target score, as a percentage. |

---

### `M_STRING_TARGET_CERTAINTY`

Sets the certainty level for the string target score.  If both the string score and the string target score of a string candidate are above their respective certainty levels, the candidate is immediately considered a string result, without reading the rest of the target image for a string with higher scores (provided the specified number of strings to read has been read).  Scores can be retrieved with [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the string target score, as a percentage. |

---

### `M_STRING_USER_LABEL`

Sets a unique user-defined label for the specified string model. This label can be used as a means of identifying your string model, independently from its index, in the String Reader context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NO_LABEL` *(default)* | Specifies that no user label is associated with the string model. |
| `Value` | Specifies the user label of the string model. The value must be an integer that is not associated as a label with any other string model in the String Reader context. Since the same label cannot be associated with multiple string models, you cannot pass a user label if you use [`MstrControl`](../../Reference/str/MstrControl.md) with **M_STRING_INDEX()** set to **M_STRING_INDEX()**. |

### For a specific font

The possible [`ControlType`](../../Reference/str/MstrControl.md) and corresponding [`ControlValue`](../../Reference/str/MstrControl.md) parameter settings for a specific font (**M_FONT_INDEX()**) are:

---

### `M_DRAW_BOX_MARGIN_X`

Sets the margin in the X-direction, between the character box and the drawing box.  The character box is the bounding box of the character. The drawing box is the box used for drawing purposes. For more information, see [`MstrDraw`](../../Reference/str/MstrDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the horizontal margin, in pixels. |

---

### `M_DRAW_BOX_MARGIN_Y`

Sets the margin in the Y-direction, between the character box and the drawing box.  The character box is the bounding box of the character. The drawing box is the box used for drawing purposes. For more information, see [`MstrDraw`](../../Reference/str/MstrDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the vertical margin, in pixels. |

---

### `M_FONT_USER_LABEL`

Sets a unique user-defined label for the specified font. This label can be used as a means of identifying your font, independently from its index, in the String Reader context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NO_LABEL` *(default)* | Specifies that no user label is associated with the font. |
| `Value` | Specifies the user label of the font. The value must be an integer that is not associated as a label with any other font in the String Reader context. Since the same label cannot be associated with multiple fonts, you cannot pass a user label if you use [`MstrControl`](../../Reference/str/MstrControl.md) with **M_FONT_INDEX()** set to **M_FONT_INDEX()**. |

### For a font-based or a fontless context

The following [`ControlType`](../../Reference/str/MstrControl.md) and corresponding [`ControlValue`](../../Reference/str/MstrControl.md) parameter settings can be specified for either a font-based context or for a fontless context.  Note that the [`Index`](../../Reference/str/MstrControl.md) parameter must be set to **M_FONT_INDEX()** for a font-based context or to [`M_CONTEXT`](../../Reference/str/MstrControl.md) for a fontless context.

---

### `M_SPACE_WIDTH`

Sets the width of the space character of the font.  Note that the space character is not like other characters in the font. For example, you cannot set it as a constraint ([`MstrSetConstraint`](../../Reference/str/MstrSetConstraint.md)), and it is not counted in the string size ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SIZE_MIN`](../../Reference/str/MstrControl.md) and [`M_STRING_SIZE_MAX`](../../Reference/str/MstrControl.md)). For more information, see [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_STRING_SIZE`](../../Reference/str/MstrGetResult.md), [`M_STRING`](../../Reference/str/MstrGetResult.md), and [`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md).  Together with the maximum number of consecutive space characters allowed in the string ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md)), [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md) determines the maximum distance between 2 adjacent characters of the same string. If the distance between two characters is greater than the maximum distance, then they are considered to be part of 2 different strings. [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md) also determines how to convert the distance into space characters, when getting formatted string results ([`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies that the width of the space character is infinite.  In this case, all characters on the same line are always part of the same string. Also, there will never be a space character in the read string since the distance can never be large enough to constitute a space character. |
| `M_MAX_CHAR_WIDTH` | Specifies that the width of the space character is equal to the maximum character X-size of the font. |
| `M_MEAN_CHAR_WIDTH` *(default)* | Specifies that the width of the space character is equal to the average character X-size of the font. |
| `M_MIN_CHAR_WIDTH` | Specifies that the width of the space character is equal to the minimum character X-size of the font. |
| `M_QUARTER_MAX_CHAR_WIDTH` | Specifies that the width of the space character is equal to a quarter of the maximum character width of the font.  [`M_QUARTER_MAX_CHAR_WIDTH`](../../Reference/str/MstrControl.md) is the closest to the typical space width of a standard font. |
| `Value >= 1` | Specifies the width of the space character, in pixels. |

### For the result buffer

The following [`ControlType`](../../Reference/str/MstrControl.md) and corresponding [`ControlValue`](../../Reference/str/MstrControl.md) parameter settings are used to control settings of the result buffer.  Note that the [`Index`](../../Reference/str/MstrControl.md) parameter must be set to [`M_GENERAL`](../../Reference/str/MstrControl.md).

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MstrGetResult`](../../Reference/str/MstrGetResult.md) generates an error if the result was not calculated on a calibrated image. |

Note that this control type is only available for a fontless context.

Note that this control type is only available for a font-based context.
