---
doctype: Reference
module: str
function: MstrEditFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrEditFont"
---

# MstrEditFont

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

> Edit a specified font.

## Syntax

```c
void MstrEditFont(
    AIL_ID       ContextId,      //out
    AIL_INT      FontIndex,      //in
    AIL_INT64    Operation,      //in
    AIL_INT64    OperationMode,  //in
    AIL_INT      Param1,         //in
    const void * Param2Ptr,      //in
    const void * Param3Ptr       //in
)
```

## Description

This function allows you to edit a specified font. For example, you can add or remove characters from a font, as well as normalize a character in a font. Use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_FONT_ADD`](../../Reference/str/MstrControl.md) to add a font to the context.

Note that this function is only available for a font-based context.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the String Reader context that contains the font to edit. The String Reader context must have been previously allocated on the required system using [`MstrAlloc`](../../Reference/str/MstrAlloc.md).

### `FontIndex` *(in, AIL_INT)*

Specifies the index of the font to edit.

*For specifying the font to edit*

| Value | Description |
| --- | --- |
| `M_FONT_INDEX` | Specifies the font to which to apply the edit settings. |

### `Operation` *(in, AIL_INT64)*

Specifies the operation to be performed.

### `OperationMode` *(in, AIL_INT64)*

Specifies the mode of the operation.

### `Param1` *(in, AIL_INT)*

Specifies a value that is dependent on the operation and mode chosen.

### `Param2Ptr` *(in, *void)*

Specifies the address of the value that will be used to edit the font. The value is dependent on the operation and mode chosen.

### `Param3Ptr` *(in, *void)*

Specifies the address of the value that will be used to edit the font. The value is dependent on the operation and mode chosen.

## Parameter Associations

### For performing the operation

To specify the operation to perform, set the [`Operation`](../../Reference/str/MstrEditFont.md), [`OperationMode`](../../Reference/str/MstrEditFont.md), [`Param1`](../../Reference/str/MstrEditFont.md), [`Param2Ptr`](../../Reference/str/MstrEditFont.md), and [`Param3Ptr`](../../Reference/str/MstrEditFont.md) parameters to the following values:

---

### `M_CHAR_ADD`

Adds character(s) to the font.

#### `M_SYSTEM_FONT`

Specifies that characters from a given system font (for example, TrueType and Postscript) will be added.

| Value | Description |
| --- | --- |
| `Value > 6` | Specifies the size, in points. |
| `M_NULL` | Specifies that the standard characters of the font will be added. That is, A to Z, a to z, and 0 to 9. |
| `"String"` | Specifies the string. This must be a null-terminated string. |
| `"System font file name"` | Specifies the string containing the font's file name (for example, "TrueType" and "Postscript"). This must be a null-terminated string. |

#### `M_USER_DEFINED`

Specifies that user-defined characters will be added from a given image to the specified font.

| Value | Description |
| --- | --- |
| `Image identifier` | Specifies the image identifier. The characters to add in the image must all be approximately the same size. You cannot add a character that is smaller than 6x6 pixels. Note that the size of the characters is automatically determined from the image.  The characters in the image are taken from left to right and from top to bottom and are associated with the corresponding characters given in the character list. Each character in the image must be entirely connected (except for the accentuated characters) and should not be merged with other characters or other image objects.  It is best to provide a character definition image that is of good quality, and that ideally contains only the characters that you want to add to the font (though this is not mandatory). If all the characters in the string you want to add cannot be found in the image, an error is returned. Even if the operation succeeds, you should draw the characters of the font to ensure that every character is defined as expected, using [`MstrDraw`](../../Reference/str/MstrDraw.md).  The image must be of type 1-band, 8-bit, unsigned. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `"String"` | Specifies the string. This must be a null-terminated string.  Two characters in one font cannot have the same value. If you try to add a character with a value that already exists in a font, the existing character will be replaced by the newly added one, unless you use [`M_NO_OVERWRITE`](../../Reference/str/MstrEditFont.md). For more information, see the combination constants tables below.  For a multi-string definition image, the space character (ASCII 32) must be inserted to separate the strings included in this null-terminated string. You should separate the string when one (or more) of the following situations is encountered: the characters are not all on the same line, the contrast is clearly different between characters, there is clearly a horizontal space between the characters (for example, ABC DEF). |

#### `M_USER_DEFINED + M_SINGLE`

Specifies that the whole image will be used to define a single user-defined character in the specified font. This allows you to define special characters, which are not necessarily connected. The image is associated with the character specified by the [`Param2Ptr`](../../Reference/str/MstrEditFont.md) parameter.  It is best to provide a character definition image that is of good quality. Note that if the character you want to add already exists in the font, the new character will replace the old one. You cannot add a character that is smaller than 6x6 pixels. By default, user-defined characters have no baseline.

---

### `M_CHAR_BASELINE`

Sets the baseline of the characters in the font.

#### `M_DEFAULT`

Implements the default behavior.  By default, for system fonts ([`M_SYSTEM_FONT`](../../Reference/str/MstrEditFont.md)), the baseline will be taken from that font's file.  By default, for user-defined fonts ([`M_USER_DEFINED`](../../Reference/str/MstrEditFont.md)), the baseline will be set to [`M_AUTO_COMPUTE`](../../Reference/str/MstrEditFont.md), which automatically computes the baseline to an appropriate value.  When using a font to read a string model, the baseline of all characters in the string model are expected to be aligned together within a certain degree of tolerance (set using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_CHAR_MAX_BASELINE_DEVIATION`](../../Reference/str/MstrControl.md)), otherwise the string will not be read.

| Value | Description |
| --- | --- |
| `M_AUTO_COMPUTE` | Specifies that the baseline will be automatically computed to an appropriate value. |
| `M_NONE` | Specifies no baseline. |
| `-1000 <= Value <= 1000` | Specifies the baseline value, as a percentage of the character's height.  The position of the baseline in a character is defined as a percentage of the height of the character. That is, 0 refers to the bottom of character, 100 refers to the top of the character, and 50 refers to the middle of character. For example, in "ABCD", you would typically set the baseline to 0 for all characters, while in "pobq", you would typically set the baseline to 0 for "ob" and approximately 25 for "pq". Note that you can set a baseline value that falls outside the limits of the character's height (Y-size). |
| `M_NULL` | Specifies that the baseline will be set for all the characters in the font. |
| `"String"` | Specifies the string. This must be a null-terminated string. |

---

### `M_CHAR_DELETE`

Deletes characters from the font.

#### `M_DEFAULT`

Implements the default behavior.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to delete all the characters in the font. |
| `"String"` | Specifies the string of characters to delete. This must be a null-terminated string. |

---

### `M_CHAR_NORMALIZE`

Normalizes the characters of the font.

#### `M_SIZE_X`

Specifies that the characters of the font are normalized by taking the X-size as the reference. That is, all the specified characters will be uniformly scaled to a given X-size. Note that the characters' Y-size is adjusted to maintain the same aspect ratio.

| Value | Description |
| --- | --- |
| `Value >= 8` | Specifies the size, in pixels. |
| `M_NULL` | Specifies that all the characters in the font will be normalized a given X-size. |
| `"String"` | Specifies the string. This must be a null-terminated string. |

#### `M_SIZE_Y`

Specifies that the characters of the font are normalized by taking the Y-size as the reference. That is, all the specified characters will be uniformly scaled to a given Y-size. Note that the characters' X-size is adjusted to maintain the same aspect ratio.

| Value | Description |
| --- | --- |
| `Value >= 8` | Specifies the size, in pixels. |
| `M_NULL` | Specifies that all the characters in the font will be normalized a given Y-size. |
| `"String"` | Specifies the string. This must be a null-terminated string. |

---

### `M_CHAR_SORT`

Sorts the characters of the font.

#### `M_ASCENDING`

Specifies that the characters will be sorted, according to their character values, in ascending order.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that all the characters in the font will be sorted in ascending order. |
| `"String"` | Specifies the string. This must be a null-terminated string. |

#### `M_DESCENDING`

Specifies that the characters will be sorted, according to their character values, in descending order.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that all the characters in the font will be sorted in descending order. |
| `"String"` | Specifies the string. This must be a null-terminated string. |

---

### `M_CHAR_TYPE`

Sets the type of the characters in the font.

#### `M_DEFAULT`

Implements the default behavior.

| Value | Description |
| --- | --- |
| `M_AUTO_COMPUTE` | Specifies that the characters' type will be computed automatically. The type will be set to either [`M_REGULAR`](../../Reference/str/MstrEditFont.md) or [`M_PUNCTUATION`](../../Reference/str/MstrEditFont.md), based on the character's shape and numerical code. |
| `M_PUNCTUATION` | Specifies punctuation type characters. Punctuation characters can typically be categorized as characters that are neither letters nor numbers, such as the hyphen ('-'). Note that to be considered part of the string, a punctuation character must fall within the range of at least one regular character's Y-size. |
| `M_REGULAR` | Specifies regular type characters. Regular characters can typically be categorized as letters and numbers. Note that a string is formed by a linear sequence of regular characters. |
| `M_NULL` | Specifies that the type will be set for all the characters in the font. |
| `"String"` | Specifies the string. This must be a null-terminated string. |

---

### `M_THICKEN_CHAR`

Thickens the characters of the font. The characters of the font should always be the thickened version, so use this [`Operation`](../../Reference/str/MstrEditFont.md) if the font has dotted characters.

#### `M_DEFAULT`

Implements the default behavior.

| Value | Description |
| --- | --- |
| `0 <= Value <= 100` | Specifies the number of iterations. |
| `M_NULL` | Specifies to normalize all the characters of the font. |
| `"String"` | Specifies the string. This must be a null-terminated string. |

### Combination Constants — For setting the foreground of the characters

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the foreground of the characters in the definition image when adding user-defined characters to a font.

Note that when a font is defined, it has no foreground. When performing a read operation ([`MstrRead`](../../Reference/str/MstrRead.md)), the foreground the font is read with is set using [`M_FOREGROUND_VALUE`](../../Reference/str/MstrControl.md) in [`MstrControl`](../../Reference/str/MstrControl.md). In this case (when adding user-defined characters to a font), you are setting the foreground for the font in the definition image.

| Value | Description |
| --- | --- |
| `M_FOREGROUND_BLACK` *(default)* | Specifies that black is the foreground color, for the definition image. |
| `M_FOREGROUND_WHITE` | Specifies that white is the foreground color, for the definition image. |

### Combination Constants — For not overwriting characters

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that characters will not be added to the font if they already exist.

| Value | Description |
| --- | --- |
| `M_NO_OVERWRITE` *(default)* | Specifies that the characters previously added to the font will not be overwritten. |
