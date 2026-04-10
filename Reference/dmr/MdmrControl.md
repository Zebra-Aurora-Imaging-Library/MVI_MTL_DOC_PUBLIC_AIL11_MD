---
doctype: Reference
module: dmr
function: MdmrControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrControl"
---

# MdmrControl

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

> Control a SureDotOCR context or result buffer.

## Syntax

```c
void MdmrControl(
    AIL_ID     ContextOrResultDmrId,  //out
    AIL_INT64  ControlType,           //in
    AIL_DOUBLE ControlValue           //in
)
```

## Description

This function allows you to control a SureDotOCR context or a SureDotOCR result buffer. This includes adding or deleting a font or string model from a context, or removing all results from a result buffer. To inquire about a context, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md). To get results from a result buffer, call [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md).

To control the fonts and string models held by a SureDotOCR context, call [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) and [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md), respectively.

For a successful read operation, you must specify an explicit value for the control types that set the diameter of the dots in the string's characters ([`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md)). You must specify the dimensions of the dot-matrix text block using [`M_TEXT_BLOCK_HEIGHT`](../../Reference/dmr/MdmrControl.md) and [`M_TEXT_BLOCK_WIDTH`](../../Reference/dmr/MdmrControl.md); the dot-matrix text block is the area in the image buffer (or in the buffer's region of interest) that includes all dot-matrix text.

You must preprocess the SureDotOCR context after you have finished adjusting context settings and before calling [`MdmrRead`](../../Reference/dmr/MdmrRead.md). To know if a context needs to be preprocessed, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) with [`M_PREPROCESSED`](../../Reference/dmr/MdmrInquire.md).

## Parameters

### `ContextOrResultDmrId` *(out, AIL_ID)*

Specifies the identifier of the SureDotOCR context or result buffer to control. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md). The result buffer must have been previously allocated on the system using [`MdmrAllocResult`](../../Reference/dmr/MdmrAllocResult.md).

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### For controlling a SureDotOCR context setting

The following [`ControlType`](../../Reference/dmr/MdmrControl.md) and corresponding [`ControlValue`](../../Reference/dmr/MdmrControl.md) parameter settings are used to control a setting of the SureDotOCR context specified with the [`ContextOrResultDmrId`](../../Reference/dmr/MdmrControl.md) parameter. SureDotOCR can only read strings that adhere to these settings (they apply to all strings in the context). If there are strings that require different settings, use a separate context.

---

### `M_DOT_DIAMETER`

Sets the diameter of the dots that make up the characters in the strings.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the diameter, in pixels. |

---

### `M_DOT_DIAMETER_SPREAD`

Sets the supported spread of the dots. This essentially sets the tolerance for the size of the dot diameter, as a value centered around the specified nominal dot diameter ([`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md)). For example, if [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md) is set to 4.0 and this control type is set to 2.0, Aurora Imaging Library will read all dots between the size of 3.0 and 5.0.  This control type is ignored if [`M_DOT_DIAMETER_SPREAD_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_DISABLE`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the tolerance for the size of the dot diameter, in pixels. |

---

### `M_DOT_DIAMETER_SPREAD_MODE`

Sets whether the dot diameter can vary within a specified range of possible diameters specified using[`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md) is ignored. |
| `M_ENABLE` | Specifies that [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md) is used during a read operation. |

---

### `M_DOT_DIAMETER_STEP`

Sets the increment between steps that will be used when checking for dots in the range of supported dot diameters, set using [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md). For example, if [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md) is set to 1.0, [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md) is set to 2.0, and this control type is set to 0.2, Aurora Imaging Library will check 1.2, 1.4, 1.6 and so on.  This control type is ignored if [`M_DOT_DIAMETER_SPREAD_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_DISABLE`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the step, in pixels. |

---

### `M_FOREGROUND_VALUE`

Sets the foreground value of the characters in the strings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FOREGROUND_BLACK` *(default)* | Specifies that strings with characters darker than the background will be read. For example, black characters on a white surface. |
| `M_FOREGROUND_WHITE` | Specifies that strings with characters lighter than the background will be read. For example, white characters on a black surface. |

---

### `M_ITALIC_ANGLE`

Sets the italic angle of all the characters in the strings. The angle must be provided counter clockwise and is relative a line perpendicular to the angle of the string.[`M_ITALIC_ANGLE`](../../Reference/dmr/MdmrControl.md)only has an effect if [`M_ITALIC_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md)is set to [`M_ANGLE`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-90.0 <= Value <= 90.0` *(default)* | Specifies the italic angle, in degrees. A positive angle means that the characters are leaning counter-clockwise (left leaning); a negative angle means that the characters are leaning clockwise (right leaning). |

---

### `M_ITALIC_ANGLE_MODE`

Sets whether the italic angle is detected automatically or explicitly specified. [`M_ITALIC_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md)must be set to [`M_DEFAULT`](../../Reference/dmr/MdmrControl.md)if [`M_STRING_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md)is set to [`M_DEFAULT`](../../Reference/dmr/MdmrControl.md); otherwise, [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md) will generate an error when called.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrControl.md). |
| `M_ANGLE` | Specifies to use the italic angle specified with [`M_ITALIC_ANGLE`](../../Reference/dmr/MdmrControl.md). A string will only be read if its characters have the specified italic angle. |
| `M_AUTO` | Specifies that the italic angle is detected automatically. [`M_ITALIC_ANGLE`](../../Reference/dmr/MdmrControl.md) is ignored. |

---

### `M_ITALIC_PITCH`

Sets the distance between successive dot centers in the italic angle direction. [`M_ITALIC_PITCH`](../../Reference/dmr/MdmrControl.md) only has an effect if [`M_ITALIC_PITCH_MODE`](../../Reference/dmr/MdmrControl.md)is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the pitch, in pixels. |

---

### `M_ITALIC_PITCH_MODE`

Sets whether the distance between successive dot centers, in the italic angle direction, is detected automatically or explicitly specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrControl.md). |
| `M_AUTO` | Specifies to automatically establish the distance between dot centers in the italic angle direction. |
| `M_USER_DEFINED` | Specifies to use the value set with [`M_ITALIC_PITCH`](../../Reference/dmr/MdmrControl.md). A string will only be read if its characters have the specified italic pitch. |

---

### `M_MAX_INTENSITY`

Sets the maximum pixel intensity of a character's dots. [`M_MAX_INTENSITY`](../../Reference/dmr/MdmrControl.md) only has an effect if [`M_MAX_INTENSITY_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 255.0` *(default)* | Specifies the maximum pixel intensity. |

---

### `M_MAX_INTENSITY_MODE`

Sets how to establish the maximum pixel intensity of a character's dots. For a dot to be part of a character, its pixel intensity must be less than or equal to the maximum intensity.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the maximum pixel intensity of a character's dots. By default, SureDotOCR considers dots with any pixel intensity less than or equal to 255.0. |
| `M_USER_DEFINED` | Specifies to use the maximum pixel intensity value set with [`M_MAX_INTENSITY`](../../Reference/dmr/MdmrControl.md). |

---

### `M_MIN_CONTRAST`

Sets the minimum contrast between a character and its background. [`M_MIN_CONTRAST`](../../Reference/dmr/MdmrControl.md) only has an effect if [`M_MIN_CONTRAST_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 255.0` *(default)* | Specifies the minimum contrast. |

---

### `M_MIN_CONTRAST_MODE`

Sets how to establish the minimum contrast (difference in pixel intensity) between a character (foreground) and its background. For a character to be read, its contrast must be greater than or equal to the minimum contrast.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the minimum contrast of characters. Regardless of the automatically calculated value, SureDotOCR will not read characters with a contrast less than 15.0. |
| `M_USER_DEFINED` | Specifies to use the minimum contrast value set with [`M_MIN_CONTRAST`](../../Reference/dmr/MdmrControl.md). |

---

### `M_MIN_INTENSITY`

Sets the minimum pixel intensity of a character's dots. [`M_MIN_INTENSITY`](../../Reference/dmr/MdmrControl.md) only has an effect if [`M_MIN_INTENSITY_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 255.0` *(default)* | Specifies the minimum pixel intensity. |

---

### `M_MIN_INTENSITY_MODE`

Sets how to establish the minimum pixel intensity of a character's dots. For a dot to be part of a character, its intensity must be greater than or equal to the minimum intensity.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the minimum pixel intensity of character dots. By default, SureDotOCR considers dots with any pixel intensity greater than or equal to 0.0. |
| `M_USER_DEFINED` | Specifies to use the minimum pixel intensity value set with [`M_MIN_INTENSITY`](../../Reference/dmr/MdmrControl.md). |

---

### `M_SPACE_SIZE_MAX`

Sets the value with which to determine the maximum width of a space character. This value is interpreted according to [`M_SPACE_SIZE_MAX_MODE`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 3.0. |
| `Value > 0.0` | Specifies the maximum width value. |

---

### `M_SPACE_SIZE_MAX_MODE`

Sets how to determine the maximum width of a space character. If the actual distance between two characters is greater than the maximum width, the read operation considers them part of separate strings. [`M_SPACE_SIZE_MAX_MODE`](../../Reference/dmr/MdmrControl.md) indicates how to interpret [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CHAR_WIDTH_FACTOR` *(default)* | Specifies that the maximum width of a space character is determined by multiplying the [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md) value by the string's maximum character width, in pixels. For example, if [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md) is set to 3 ([`M_DEFAULT`](../../Reference/dmr/MdmrControl.md)), the maximum space character width is equivalent to three times the string's maximum character width. |
| `M_DISABLE` | Specifies no maximum space width. This is equivalent to allowing an infinite amount of maximum space. In this case, no amount of space between aligned characters can cause them to be part of separate strings. This does not apply to characters on separate lines or severely misaligned characters. |

---

### `M_SPACE_SIZE_MIN`

Sets the value with which to determine the minimum width of a space character. This value is interpreted according to [`M_SPACE_SIZE_MIN_MODE`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 1.0. |
| `Value > 0.0` | Specifies the minimum width value. |

---

### `M_SPACE_SIZE_MIN_MODE`

Sets how to determine the minimum width of a space character. If the actual distance between two characters is less than the minimum width, SureDotOCR does not consider there to be a space. [`M_SPACE_SIZE_MIN_MODE`](../../Reference/dmr/MdmrControl.md) indicates how to interpret [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CHAR_WIDTH_FACTOR` *(default)* | Specifies that the minimum width of a space character is determined by multiplying the [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md) value by the string's maximum character width, in pixels. For example, if [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md) is set to 1 ([`M_DEFAULT`](../../Reference/dmr/MdmrControl.md)), the minimum space character width is equivalent to the string's maximum character width. |
| `M_DISABLE` | Specifies no minimum space width. This is equivalent to allowing an infinite amount of minimum space. In this case, no amount of space between characters in a string can cause SureDotOCR to recognize an actual space. |

---

### `M_STRING_ANGLE`

Sets at which angle to read a string, with respect to the coordinate system specified with [`M_STRING_ANGLE_INPUT_UNITS`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_REGION` | Specifies that the angle is set to the angle of the target image's ROI. If this value is chosen but no ROI has been defined, the angle is set to 0.0° in the coordinate system specified with [`M_STRING_ANGLE_INPUT_UNITS`](../../Reference/dmr/MdmrControl.md). |
| `0.0 <= Value <= 360.0` *(default)* | Specifies a fixed angle at which to read a string, in degrees. This is interpreted with respect to the X-axis of the coordinate system specified with [`M_STRING_ANGLE_INPUT_UNITS`](../../Reference/dmr/MdmrControl.md). |

---

### `M_STRING_ANGLE_INPUT_UNITS`

Sets the coordinate system to use for [`M_STRING_ANGLE`](../../Reference/dmr/MdmrControl.md). This control type is ignored when [`M_STRING_ANGLE`](../../Reference/dmr/MdmrControl.md) is set to [`M_ACCORDING_TO_REGION`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. |

---

### `M_STRING_ANGLE_MODE`

Sets whether the string angle is detected automatically or explicitly specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrControl.md). |
| `M_ANGLE` | Specifies to use the angle specified with [`M_STRING_ANGLE`](../../Reference/dmr/MdmrControl.md); the string will be read from left to right with the characters facing upward along the specified angle. A string will only be read if it has the specified angle. |
| `M_AUTO` | Specifies that the string angle is detected automatically; [`M_STRING_ANGLE`](../../Reference/dmr/MdmrControl.md) is ignored. |
| `M_ORIENTATION` | Specifies that the string will be read from left to right with the characters facing upward, or from right to left with the character facing downward. This is useful in cases where the orientation of a string is rotated 180 degrees, and this rotation does not affect the acceptability of a product (for example, if the product is placed backwards at an inspection point but is still a good part). |

---

### `M_STRING_PARTIAL_CHAR_INVALID`

Sets the single character string to replace invalid or unrecognized characters if [`M_STRING_PARTIAL_MODE`](../../Reference/dmr/MdmrControl.md) is enabled. The default value is "?".  > **Note:** Note that the specified character is validated during the preprocess operation to confirm that the selected character does not appear in any font in the context.

| Value | Description |
| --- | --- |
| `"?"` | Specifies the default value as "?". |
| `"SingleCharacterString"` | Specifies the single character string to replace the invalid or unrecognized characters. |

---

### `M_STRING_PARTIAL_MODE`

Sets how partial strings are returned during the reading process.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that only complete strings will be returned. |
| `M_ENABLE` | Specifies that the best match for a partial string will be returned; unrecognized characters in the string will be replaced with[`M_STRING_PARTIAL_CHAR_INVALID`](../../Reference/dmr/MdmrControl.md). |

---

### `M_STRING_PITCH`

Sets the distance between successive dot centers, in the string angle direction. [`M_STRING_PITCH`](../../Reference/dmr/MdmrControl.md) only has an effect if [`M_STRING_PITCH_MODE`](../../Reference/dmr/MdmrControl.md)is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the pitch, in pixels. |

---

### `M_STRING_PITCH_MODE`

Sets whether the distance between successive dot centers, in the string angle direction, is detected automatically or explicitly specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_AUTO`](../../Reference/dmr/MdmrControl.md). |
| `M_AUTO` | Specifies to automatically establish the distance between dot centers in the string angle direction. |
| `M_USER_DEFINED` | Specifies to use the value set with [`M_STRING_PITCH`](../../Reference/dmr/MdmrControl.md). A string will only be read if it has the specified string pitch. |

---

### `M_TEXT_BLOCK_HEIGHT`

Sets the height of the text block. This is the area that encloses all dot-matrix text in the image buffer (or in the buffer's region of interest). The height is oriented along the vertical axis that is perpendicular to the base of the string(s) to read in the text block. [`M_TEXT_BLOCK_HEIGHT`](../../Reference/dmr/MdmrControl.md) only has an effect if [`M_TEXT_BLOCK_SIZE_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the height, in pixels. |

---

### `M_TEXT_BLOCK_SIZE_MODE`

Sets how to establish the size of the text block. This is the area that encloses all dot-matrix text in the image buffer (or in the buffer's region of interest).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED` *(default)* | Specifies that the size of the rectangular block is defined by the [`M_TEXT_BLOCK_HEIGHT`](../../Reference/dmr/MdmrControl.md) and [`M_TEXT_BLOCK_WIDTH`](../../Reference/dmr/MdmrControl.md) settings. |

---

### `M_TEXT_BLOCK_WIDTH`

Sets the width of the text block. This is the area that encloses all dot-matrix text in the image buffer (or in the buffer's region of interest). The width is oriented along the axis that represents the base of the string(s) to read in the text block. [`M_TEXT_BLOCK_WIDTH`](../../Reference/dmr/MdmrControl.md) only has an effect if [`M_TEXT_BLOCK_SIZE_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the width, in pixels. |

---

### `M_TIMEOUT`

Sets the maximum read time for [`MdmrRead`](../../Reference/dmr/MdmrRead.md). No results are returned if the read operation times out.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies an infinite amount of read time. |
| `Value >= 0.0` *(default)* | Specifies the maximum read time, in msec. |

### For adding fonts or string models to, or deleting fonts or string models from, a SureDotOCR context

The following [`ControlType`](../../Reference/dmr/MdmrControl.md) and corresponding [`ControlValue`](../../Reference/dmr/MdmrControl.md) parameter settings are used to add fonts or string models to, or delete fonts or string models from, the SureDotOCR context specified with the [`ContextOrResultDmrId`](../../Reference/dmr/MdmrControl.md) parameter.

---

### `M_FONT_ADD`

Adds an empty font to the context. To add characters to a font, use [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md). You can also add characters to a font, or modify its settings, using [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md). The maximum number of fonts in a context is 255. To inquire the number of fonts in the context, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) with [`M_NUMBER_OF_FONTS`](../../Reference/dmr/MdmrInquire.md).  When you add an empty font, the dimensions of its dot-matrix template are initially set to 5 columns and 7 rows. If the font is empty, you can change these values by calling [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with [`M_FONT_SIZE_COLUMNS`](../../Reference/dmr/MdmrControlFont.md) and [`M_FONT_SIZE_ROWS`](../../Reference/dmr/MdmrControlFont.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEW_LABEL` *(default)* | Specifies to add a font with an automatically assigned label. To inquire about the label, use [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md) with [`M_FONT_LABEL_VALUE`](../../Reference/dmr/MdmrInquireFont.md). To change the label, use [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with [`M_FONT_LABEL_VALUE`](../../Reference/dmr/MdmrControlFont.md). |
| `Value > 0` | Specifies to add a font with the indicated label, as an _integer_. The label must be unique among all font labels in the context. |

---

### `M_FONT_DELETE`

Deletes one or all fonts from the context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FONT_INDEX` | Specifies to delete the font by indicating its index. |
| `M_FONT_LABEL` | Specifies to delete the font by indicating its label. |
| `M_ALL` *(default)* | Specifies to delete all fonts. |

---

### `M_STRING_ADD`

Adds an empty string model to a context. To add character constraints to the string model, or to modify its settings, use [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md). The maximum number of string models in a context is 255. To inquire the number of string models in a context, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) with [`M_NUMBER_OF_STRING_MODELS`](../../Reference/dmr/MdmrInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEW_LABEL` *(default)* | Specifies to add a string model with an automatically assigned label. To inquire about the label, use [`MdmrInquireStringModel`](../../Reference/dmr/MdmrInquireStringModel.md) with [`M_STRING_LABEL_VALUE`](../../Reference/dmr/MdmrInquireStringModel.md). To change the label, use [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_STRING_LABEL_VALUE`](../../Reference/dmr/MdmrControlStringModel.md). |
| `Value > 0` | Specifies to add a string model with the indicated label, as an _integer_. The label must be unique among all string model labels in the context. |

---

### `M_STRING_DELETE`

Deletes one or all string models from a context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_STRING_INDEX` | Specifies to delete the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies to delete the string model by indicating its label. |
| `M_ALL` *(default)* | Specifies to delete all string models. |

### For controlling a SureDotOCR result buffer

The following [`ControlType`](../../Reference/dmr/MdmrControl.md) and corresponding [`ControlValue`](../../Reference/dmr/MdmrControl.md) parameter settings are used to control the SureDotOCR result buffer specified with the [`ContextOrResultDmrId`](../../Reference/dmr/MdmrControl.md) parameter.

---

### `M_RESET`

Removes all results from the SureDotOCR result buffer. This does not delete the result buffer identifier, as is the case with [`MdmrFree`](../../Reference/dmr/MdmrFree.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MstrGetResult`](../../Reference/str/MstrGetResult.md) generates an error if the result was not calculated on a calibrated image. |

---

### `M_STOP_READ`

Stops the current read operation. You can only perform this operation from another thread of higher priority. No results are returned if you stop the read operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
