---
doctype: Reference
module: ocr
function: MocrControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrControl"
---

# MocrControl

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

> Control an OCR font context or an OCR result buffer setting.

## Syntax

```c
void MocrControl(
    AIL_ID     FontContextOrResultOcrId,  //out
    AIL_INT64  ControlType,               //in
    AIL_DOUBLE ControlValue               //in
)
```

## Description

This function allows you to control an OCR read/verify operation setting. This function also allows you to control various settings that affect how results are retrieved by [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md). To inquire the current value of a particular control type, use [`MocrInquire`](../../Reference/ocr/MocrInquire.md).

After changing the OCR controls or constraints, use [`MocrPreprocess`](../../Reference/ocr/MocrPreprocess.md) to speed up any following read or verify operation.

## Parameters

### `FontContextOrResultOcrId` *(out, AIL_ID)*

Specifies the identifier of the OCR font context or the OCR result buffer to control.

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### For the characteristics of the font

The following [`ControlType`](../../Reference/ocr/MocrControl.md) and corresponding [`ControlValue`](../../Reference/ocr/MocrControl.md) parameter settings are used to specify the characteristics of the font.

---

### `M_CHAR_ERASE`

Erases a character from the font of the OCR font context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the ASCII character associated with the character. Note that the character must be present in the OCR font context. |

### For operation controls for read and verify operations

The following [`ControlType`](../../Reference/ocr/MocrControl.md) and corresponding [`ControlValue`](../../Reference/ocr/MocrControl.md) parameter settings are used to specify operational controls for read and verify operations.

---

### `M_BLANK_CHARACTERS`

Sets whether the space between characters in the target image is read or not.  Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. In addtion, blank characters cannot be verified (using [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md)) and will cause an Aurora Imaging Library error.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that blank spaces will not be read from the target image into the result string.  Note that spaces could still be erroneously read if your OCR font context is not manually calibrated properly or the string length for your target image is wrong. |
| `M_ENABLE` | Specifies that blank spaces will be read from the target image into the result string. |

---

### `M_BROKEN_CHAR`

Sets the capability to read/verify a broken character.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that, during the read/verify, OCR should not try to compensate for broken characters. Note that this enables a faster algorithm for reading/verifying a string within the target image.  Note that broken characters could still be erroneously read if your target image is noisy, scratched, has low contrast characters, or if the illumination is not uniform. |
| `M_ENABLE` | Specifies that broken characters should be identifies as possible characters. |

---

### `M_CHAR_ACCEPTANCE`

Sets the acceptance level used to determine a successful match between the font and the characters found within the target image. During a read/verify operation, the character found with the highest match score, either equal to or greater than the acceptance level, will be returned.  If the match score is less than the acceptance level, the result with the highest match score closest to the acceptance level will be returned. If the match score is 0% the invalid character ([`M_CHAR_INVALID`](../../Reference/ocr/MocrControl.md)) is returned.  A perfect match is 100%, no correlation is 0%. The match score depends on the image quality. You should experiment to decide what is a typical match score for your application.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level for a target character. |

---

### `M_CHAR_INVALID`

Sets the ASCII character for all characters within the selected string or strings during the next read or verify operation whose match scores fall below the character acceptance level.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies that no special character will replace unrecognized characters. |
| `1 <= Value <= 255` | Specifies the character that will replace unrecognized characters. |

---

### `M_CONTEXT_CONVERT`

Changes the type of OCR font context. Note that not all controls are available with both types of OCR font contexts. The restrictions are indicated within each control. When you change the type of OCR font context, invalid controls are replaced by their default value.

| Value | Description |
| --- | --- |
| `M_CONSTRAINED` | Specifies an OCR font context that works well with degraded target images. It requires more information about the target string, but provides a more robust search. |
| `M_GENERAL` | Specifies an OCR font context that works well with clean target images. It requires less information about the target string, but provides a less robust search. |

---

### `M_EXTRA_CHARACTERS`

Sets whether to still read and identify the string when the image contains more characters than otherwise expected.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the read/verify operation should not try to compensate for extra characters. |
| `M_ENABLE` | Specifies that the read/verify operation should try to compensate for extra characters. |

---

### `M_MORPHOLOGIC_FILTERING`

Sets the number of iterations of morphological filtering.  Morphological filtering can add robustness to the OCR operation by internally enhancing the contrast of the image, but it requires processing time. The optimal number of iterations depends on the complexity of the target image.

| Value | Description |
| --- | --- |
| `0 <= Value <= 100` *(default)* | Specifies the number of iterations. 0 disables the morphologic filtering. Only integer values are accepted. |

---

### `M_SKIP_STRING_LOCATION`

Sets whether to first locate the strings in the target image before trying to identify characters against the OCR font context or to skip this step and save processing time.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that the step will not be skipped. |
| `M_ENABLE` | Specifies that the step will be skipped. |

---

### `M_SPEED`

Sets the algorithm's search speed. Note that increasing the search speed can decrease the robustness and subpixel accuracy of a read/verify operation. This is most effective on large-sized characters.

| Value | Description |
| --- | --- |
| `M_HIGH` | Specifies a high speed. |
| `M_LOW` | Specifies a low speed. |
| `M_MEDIUM` *(default)* | Specifies a medium speed. |
| `M_VERY_HIGH` | Specifies a very high speed. |
| `M_VERY_LOW` | Specifies a very low speed. |

---

### `M_STRING_ACCEPTANCE`

Sets the acceptance level used to determine a successful match between the font and a read/verified string. During a read/verify operation, the string found with the highest match score, either equal to or greater than the acceptance level, will be returned.  If the match score is less than the acceptance level, the result with the highest match score closest to the acceptance level will be returned. If the match score is 0%, a blank string is returned. Note that, in this case, [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md) with [`M_STRING_VALID_FLAG`](../../Reference/ocr/MocrGetResult.md) would be false.  A perfect match is 100%, no correlation is 0%. The match score depends on the image quality. You should experiment to decide what is a typical match score for your application.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level for a string. |

---

### `M_STRING_ANGLE_INTERPOLATION_MODE`

Sets the interpolation mode to use when reading/verifying a string at an angle.

| Value | Description |
| --- | --- |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated. |
| `M_BILINEAR` *(default)* | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

---

### `M_TOUCHING_CHAR`

Sets the capability to read/verify characters that touch each other in the target image.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the identification of touching characters. Enables a faster algorithm for read or verify operations. |
| `M_ENABLE` | Specifies to enable the identification of touching characters. |

### For the characteristics of the target characters

The following [`ControlType`](../../Reference/ocr/MocrControl.md) and corresponding [`ControlValue`](../../Reference/ocr/MocrControl.md) parameter settings are used to set the characteristics of the target characters.

---

### `M_CHAR_POSITION_VARIATION_X`

Sets the amount by which the position of the characters in the target string can vary along the X-axis. This tolerance is relative to the position of each character, as determined by [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md). This tolerance increases the region being searched for characters within the target image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the position tolerance, in pixels of the character, not of the target image. |

---

### `M_CHAR_POSITION_VARIATION_Y`

Sets the amount by which the position of the characters in the target string can vary along the Y-axis. This tolerance is relative to the position of each character, as determined by [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md). This tolerance increases the region being searched for characters within the target image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the position tolerance, in pixels of the character, not of the target image. |

---

### `M_STRING_ANGLE`

Sets the expected angle at which the string can be found.

| Value | Description |
| --- | --- |
| `M_ACCORDING_TO_REGION` *(default)* | Specifies to use the angle of the rectangular ROI (set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)) associated with the target image buffer. If the target image is not associated with an ROI, an angle of 0.0 is used. |
| `0.0 <= Value <= 360.0` | Specifies the angle, in degrees. |

---

### `M_STRING_ANGLE_DELTA_NEG`

Sets the possible angle variation in a clockwise rotation, relative to [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md). When searching for a string over a range of angles (setting [`M_STRING_ANGLE_DELTA_POS`](../../Reference/ocr/MocrControl.md) and/or [`M_STRING_ANGLE_DELTA_NEG`](../../Reference/ocr/MocrControl.md) to a value greater than 1°), the font context's character size must be greater than 16x16 to avoid an Aurora Imaging Library error when calling [`MocrReadString`](../../Reference/ocr/MocrReadString.md),[`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md) or [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the possible clockwise angle variation, in degrees. A value less than or equal to 1° will not search over a range of angles. |

---

### `M_STRING_ANGLE_DELTA_POS`

Sets the possible angle variation in a counter-clockwise rotation, relative to [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md). When searching for a string over a range of angles (setting [`M_STRING_ANGLE_DELTA_POS`](../../Reference/ocr/MocrControl.md) and/or [`M_STRING_ANGLE_DELTA_NEG`](../../Reference/ocr/MocrControl.md) to a value greater than 1°), the font context's character size must be greater than 16x16 to avoid an Aurora Imaging Library error when calling [`MocrReadString`](../../Reference/ocr/MocrReadString.md),[`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md) or [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the possible counter-clockwise angle variation, in degrees. A value less than or equal to 1° will not search over a range of angles. |

---

### `M_STRING_CHAR_NUMBER`

Sets the length of the string to be read/verified from the target image.

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the length of the string is unknown.  Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. |
| `Value <= M_STRING_SIZE_MAX` | Specifies the string length to read/verify. The length of this string must be less than or equal to the maximum string length of the OCR font context.  Note that when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context and [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md) are enabled, blank characters must be counted in the string length.  The default value is set to the maximum string length of the OCR font context. When OCR font context constraints are set, specifying a string length can improve the speed of a following read/verify operation.  Note that if [`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) is used, the string length must be 12. If [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md) is used, the string length must be 18. |

---

### `M_STRING_NUMBER`

Sets the number of strings to be read/verified from the target image. Multiple strings can only be found if each string resides on a different line within the target image and each line of text does not overlap the previous.  Note that, for best results, all strings read in an image with multiple strings should be of similar length, have a consistent inter-line spacing and should start at a similar location along the X-axis.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that the number of strings in the target image should be determined automatically. |
| `Value` *(default)* | Specifies the number of lines of text in the target image. Note that specifying this value can improve the speed of a following read/verify operation. |

---

### `M_TARGET_CHAR_SIZE_X`

Sets the width of the target characters. Note that this can also be automatically set using [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md).

| Value | Description |
| --- | --- |
| `M_SAME` | Specifies that the character width is found automatically. Use this value when the font uses a fixed width for all its characters.  Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. |
| `Value > 1` | Specifies the character width in pixels (with subpixel accuracy).  The default value is set to the character size X of the font. |

---

### `M_TARGET_CHAR_SIZE_Y`

Sets the height of the target characters. Note that this can also be automatically set using [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md).

| Value | Description |
| --- | --- |
| `M_SAME` | Specifies that the character height is found automatically. Use this value when the font uses a fixed height for all its characters.  Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. |
| `Value > 1` | Specifies the character height in pixels (with subpixel accuracy).  The default value is set to the character size Y of the font. |

---

### `M_TARGET_CHAR_SPACING`

Sets the amount of space between characters in the string. Note that this can also be automatically set using [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md).

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the inter-character spacing is unknown and not the same between the characters. This enables automatic spacing detection.  Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. |
| `M_SAME` | Specifies that the inter-character spacing is the same throughout the target string. This enables automatic spacing detection.  Note that this is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. |
| `Value >= 2` | Specifies the inter-character spacing, in pixels, with subpixel accuracy.  The default value is set to the character cell size of the font. |

---

### `M_TEXT_STRING_SEPARATOR`

Sets the ASCII character to be used as a string separator within the text read/verified. This separator will be inserted between each string. Note that this must be set before the text is read/verified.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the ASCII code of the character. |

---

### `M_THICKEN_CHAR`

Sets the number of character thickening iterations.  Each iteration enlarges the thickness of the target character. This is useful for some types of fonts (for example, dotted characters). You should choose a value large enough for the intra character dots to connect but not too large to connect separate characters together.  > **Note:** Note that the characters included in the font should not be dotted. Instead a thickened version should be included.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 100` *(default)* | Specifies the number of times a character should be thickened. Only integer values are accepted. |

---

### `M_THRESHOLD`

Sets the threshold value used to internally binarize the target image for segmentation of characters.  Note that this control type is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context.

| Value | Description |
| --- | --- |
| `M_AUTO` | Specifies to use an automatically computed threshold value. |
| `Value` *(default)* | Specifies the threshold value. |

### For the result buffer

The following [`ControlType`](../../Reference/ocr/MocrControl.md) and corresponding [`ControlValue`](../../Reference/ocr/MocrControl.md) parameter settings are used to control settings of the result buffer.

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specifed, calling [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md) generates an error if the result was not calculated on a calibrated image. |

---

### `M_SELECT_STRING`

Selects the line of text to return from the result buffer, when multiple lines of text are read/verified.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that all strings are selected. |
| `0 <= Value < M_STRING_NUMBER` *(default)* | Specifies a specific string. Strings are identified by numbers from 0 to ([`M_STRING_NUMBER`](../../Reference/ocr/MocrControl.md)-1). |
