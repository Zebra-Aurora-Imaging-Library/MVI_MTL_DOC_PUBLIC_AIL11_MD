---
doctype: UserGuide
part: "2D processing and analysis"
chapter: SureDotOCR
section: Retrieving_results_and_annotation
module_tag: dmr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / SureDotOCR / Retrieving results and annotation"
---

# Results and annotations

Aurora Imaging Library stores the results of a read operation ([`MdmrRead`](../../Reference/dmr/MdmrRead.md)) in a SureDotOCR result buffer. Most applications typically retrieve and draw various features of these results, using [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) and [`MdmrDraw`](../../Reference/dmr/MdmrDraw.md). For example, you can retrieve and draw the resulting string that was read.

> **Note:** SureDotOCR supports Unicode environments. See [](S06_Character_names.md) for details on working with character names in their native representation or in hexadecimal format.

## Results

With [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md), you can retrieve global results, string results, or character results. Global results refer to information about the SureDotOCR result buffer itself, while string and character results refer to information about the strings and characters in the result buffer.

You can remove all results from a result buffer by calling [`MdmrControl`](../../Reference/dmr/MdmrControl.md)with [`M_RESET`](../../Reference/dmr/MdmrControl.md). This does not delete the buffer identifier, as is the case with [`MdmrFree`](../../Reference/dmr/MdmrFree.md). [`M_RESET`](../../Reference/dmr/MdmrControl.md) can be a convenient and optimal way of saving memory resources.

In some cases, unrecognized characters can cause either an unexpected string or no string to be returned; to determine which characters are not recognized, you can set [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_STRING_PARTIAL_MODE`](../../Reference/dmr/MdmrControl.md) to [`M_ENABLE`](../../Reference/dmr/MdmrControl.md). [`M_STRING_PARTIAL_CHAR_INVALID`](../../Reference/dmr/MdmrControl.md) can be used to set the single character string that replaces invalid characters; the default is "?".

### Strings and formatted strings

To retrieve the string that was read, use [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) or [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md).

With [`M_STRING`](../../Reference/dmr/MdmrGetResult.md), you will retrieve the string that was read, according to the string model definition. This result includes the name (symbol) of all the characters in the string and the terminating null character. It also include spaces, if they correspond to a space permitted character constraint in the string model ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)).

With [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md), you will retrieve the string that was read, according to the target string format. This result includes the name (symbol) of all the characters in the string and the terminating null character. It also includes spaces, if the target string has any that are greater than the minimum spacing ([`MdmrControl`](../../Reference/dmr/MdmrControl.md)with [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md)) and less than the maximum spacing ([`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md)).

The following table shows the difference between [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) and [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md), given the target string read for the string model. For simplicity, 'S' refers to space and 'L' refers to letter. Any permitted character other than space could have been used instead of a letter.

| Target string | String model | [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) | [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md) |
| --- | --- | --- | --- |
| ABCD | LLLL | "ABCD" | "ABCD" |
| AB CD | LLLL | "ABCD" | "AB CD" |
| AB CD | LLSLL | "AB CD" | "AB CD" |
| AB CD EF | LLLLLL | "ABCDEF" | "AB CD EF" |
| AB CD EF | LLSLLLL | "AB CDEF" | "AB CD EF" |
| AB CD EF | LLSLLSLL | "AB CD EF" | "AB CD EF" |
| ABWCD | LLSLL | "AB CD" | "AB CD" |
| SureDotOCR attempts to read the best string possible, and can use the space constraint as an area to ignore, even if that area is not empty. This is the case for the target string ABWCD. For more information, see [Reading space](S05_String_models.md). |

When retrieving the results of characters in strings, you must use [`M_INDEX_IN_STRING(n)`](../../Reference/dmr/MdmrGetResult.md) or [`M_INDEX_IN_FORMATTED_STRING(n)`](../../Reference/dmr/MdmrGetResult.md), to specify the character (one or all). The difference between these two settings is similar to the difference between [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) and [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md). For [`M_INDEX_IN_STRING(n)`](../../Reference/dmr/MdmrGetResult.md), spaces read in the target string are indexed as character results if they correspond to a space permitted character constraint in the string model. For [`M_INDEX_IN_FORMATTED_STRING(n)`](../../Reference/dmr/MdmrGetResult.md), spaces read in the target are always indexed as character results.

For example, if the string model definition specifies to read 4 capital letters ("LLLL"), and the target string read is "AB CD", [`M_INDEX_IN_FORMATTED_STRING(3)`](../../Reference/dmr/MdmrGetResult.md)refers to 'C', while [`M_INDEX_IN_STRING(3)`](../../Reference/dmr/MdmrGetResult.md)refers to 'D'. If the string model definition specifies to read 4 capital letters and 1 space ("LLSLL"), and the target string read is "AB CD", [`M_INDEX_IN_FORMATTED_STRING(3)`](../../Reference/dmr/MdmrGetResult.md) and [`M_INDEX_IN_STRING(3)`](../../Reference/dmr/MdmrGetResult.md)both refer to 'C'.

When retrieving results with [`M_STRING_PARTIAL_MODE`](../../Reference/dmr/MdmrControl.md) enabled, you can retrieve the position of the unrecognized characters with[`M_STRING_CHAR_INVALID_INDICES`](../../Reference/dmr/MdmrGetResult.md) and [`M_FORMATTED_STRING_CHAR_INVALID_INDICES`](../../Reference/dmr/MdmrGetResult.md), for [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) and [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md) respectively.

### Reading substrings and skipped positions

If [`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) is less than [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md) and the image does not contain a complete string, Aurora Imaging Library SureDotOCR will return a substring. The substring must be at least as long as [`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) and match the string constraints without a break in the middle. If a substring is read, you can use [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dmr/MdmrGetResult.md) to see which positions in the string constraint model were skipped. For example, if there was the following constraints:

- String size min: 2.
- String size max: 6.
- |   |   |   |   |   |   |   |
  | --- | --- | --- | --- | --- | --- | --- |
  | Position | 0 | 1 | 2 | 3 | 4 | 5 |
  | Constraint | DIGIT | LETTER | DIGIT | LETTER | LETTER | LETTER. |

The table below shows the input, success, and skipped positions, using the string constraints listed above.

| String in image | String read successful | Skipped Positions |
| --- | --- | --- |
| 2A1 | Yes | 3,4,5 |
| ABC | Yes | 0,1,2 |
| A1B | Yes | 0,4,5 |
| 11ABC | No | N/A |
| 1A2CDE | Yes | N/A |

If a substring cannot be read, you can only retrieve the position of unrecognized characters if [`M_STRING_PARTIAL_MODE`](../../Reference/dmr/MdmrControl.md) was enabled prior to the read operation. In this case, you can retrieve the position of the unrecognized characters with[`M_STRING_CHAR_INVALID_INDICES`](../../Reference/dmr/MdmrGetResult.md) and [`M_FORMATTED_STRING_CHAR_INVALID_INDICES`](../../Reference/dmr/MdmrGetResult.md), for [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) and [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md) respectively.

### Typical results to retrieve

In addition to retrieving the actual strings and characters read, you will also typically want to retrieve:

- The total number of strings read ([`M_STRING_NUMBER`](../../Reference/dmr/MdmrGetResult.md)).
- The number of characters read for a string ([`M_STRING_CHAR_NUMBER`](../../Reference/dmr/MdmrGetResult.md)).
- The name of individual characters in the strings read ([`M_CHAR_NAME`](../../Reference/dmr/MdmrGetResult.md)). If a space was read, the name returned is an actual space (and the terminating null character).
- The score of the strings read ([`M_STRING_SCORE`](../../Reference/dmr/MdmrGetResult.md)). The string score quantifies, as a percentage, how closely the target string resembles its corresponding string model in the SureDotOCR context. The string score is the average score of the individual characters in the string, which you can also retrieve ([`M_CHAR_SCORE`](../../Reference/dmr/MdmrGetResult.md)).
  If an [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) permitted character was read ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md)), its resulting character score will be 100%. SureDotOCR does not use such scores when calculating the string score.
- Information about the bounding box of the strings read, such as height ([`M_STRING_HEIGHT`](../../Reference/dmr/MdmrGetResult.md)), width ([`M_STRING_WIDTH`](../../Reference/dmr/MdmrGetResult.md)), angle ([`M_STRING_ANGLE`](../../Reference/dmr/MdmrGetResult.md)), center ([`M_STRING_POSITION...`](../../Reference/dmr/MdmrGetResult.md)), and corners ([`M_STRING_BOX_...`](../../Reference/dmr/MdmrGetResult.md)).
  You can also retrieve similar results for each character in the string read ([`M_CHAR_...`](../../Reference/dmr/MdmrGetResult.md)). For example, you can retrieve the width of character's bounding box ([`M_CHAR_WIDTH`](../../Reference/dmr/MdmrGetResult.md)).
- Information regarding the state of the read operation ([`M_STATUS`](../../Reference/dmr/MdmrGetResult.md)).

### Size of string results

SureDotOCR returns the [`M_STRING`](../../Reference/dmr/MdmrGetResult.md), [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md), and [`M_CHAR_NAME`](../../Reference/dmr/MdmrGetResult.md) results as a string. To determine the size of the string returned, add [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md) to the requested result. That is, [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md), [`M_FORMATTED_STRING`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md), and [`M_CHAR_NAME`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md).

### Positional and dimensional results

If your target image ([`MdmrRead`](../../Reference/dmr/MdmrRead.md)) was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the center of top-left pixel in the target image. To specify this, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dmr/MdmrControl.md). For more information, see [Working with real-world units](../C28_Calibration/S09_Working_with_realworld_units.md).

Positional and dimensional results take into account the angle at which the result was found.

*[Image: MdmrResultStringBoxPosition.png]*

This example shows how the result for the bottom-left corner of the string box ([`M_STRING_BOX_BL_X`](../../Reference/dmr/MdmrGetResult.md) and [`M_STRING_BOX_BL_Y`](../../Reference/dmr/MdmrGetResult.md)) depends on the location and angle at which the string was read.

### Recommendations to improve results

After performing the read operation, you might not always get the most optimal results. Typical issues include:

- Unread strings.
- Incorrectly read strings (misidentified characters).
- Low string or character scores.

The following are recommendations to improve reading and get the best results possible.

- Don't have characters touching or cut off at image borders.
- Ensure a contrast level of at least 15 gray levels.
- Ensure the rank of each string model is properly set. For example, strings with different ranks must be on separate lines. For more information, see [Rank](S05_String_models.md).
- Ensure the string length is at least 4 characters to prevent the string from being read upside down.
- Ensure that the diameter of the dots is between 4-7 pixels wide. If they are too small, there can be trouble reading the dots; if they are too large, it will take more processing time.
- Ensure the variation between the smallest and largest dot spacing within a string is less than half of the average dot space.
- Ensure the distance between two consecutive dots along a character axis is larger than the dot size.
- Ensure the distance between consecutive characters is 2x the dot spacing of dots found in individual characters.
- Ensure the minimum vertical spacing between stacked strings is the dot height.
- Fonts should only contain the characters to read; delete all others. For more information, see [Delete, control, and inquire about fonts and characters](S04_Fonts.md).
- Set constraints at each character position to ensure the correct character is read at the correct position, with the specified font.
- Set up the context to read all the strings in the target image, even if you are not interested in all strings. Once strings are read, get the results of the ones you want.
- Set up the string model to read the entire string, even if you are not interested in all of it. Once the string is read, parse out the characters that you don't want.
- Set up the string box so that it is only large enough to read all the characters to be read and exclude all other features of the image.
- Set minimum and maximum dot intensities to improve performance.
- The dot-matrix of each character in the fonts should be the same as the dot-matrix of each character in the strings to read. For more information, see [Dot-matrices in fonts and strings to read should be the same](S04_Fonts.md).

## Annotations

With [`MdmrDraw`](../../Reference/dmr/MdmrDraw.md), you can draw specific features of a SureDotOCR result buffer in the destination image buffer or 2D graphics list. In general, the drawings that you will want to perform include:

- Drawing a bounding box around the string that was read ([`M_DRAW_STRING_BOX`](../../Reference/dmr/MdmrDraw.md)).
  *[Image: MdmrDrawStringBox.png]*
- Drawing a bounding box around the characters that were read ([`M_DRAW_STRING_CHAR_BOX`](../../Reference/dmr/MdmrDraw.md)).
  *[Image: MdmrDrawCharacterBox.png]*
- Drawing a circled dot at the center of the bounding box of each string's character ([`M_DRAW_STRING_CHAR_POSITION`](../../Reference/dmr/MdmrDraw.md)).
  *[Image: MdmrDrawPosition.png]*
- Drawing the names of the characters that were read ([`M_DRAW_AIL_FONT_STRING`](../../Reference/dmr/MdmrDraw.md)).
  *[Image: MdmrDrawAILFont.png]*

When applicable, the required information is drawn at the location that the string was read in the target, at the correct angle, scale, and aspect ratio. You can specify that the characters in the string appear according to how the target string is formatted (for example, with non-constrained spaces), by specifying [`M_DRAW_AIL_FONT_FORMATTED_STRING`](../../Reference/dmr/MdmrDraw.md) instead of [`M_DRAW_AIL_FONT_STRING`](../../Reference/dmr/MdmrDraw.md).

## Timeout or stop

The read operation can occasionally take an unexpectedly long time to calculate. To manage this, you can call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) to specify a timeout value or to stop the read.

Timeout refers to the maximum time that [`MdmrRead`](../../Reference/dmr/MdmrRead.md) has to read all strings for a context. By default, the maximum time is 2000.0 msec. To change this, use[`M_TIMEOUT`](../../Reference/dmr/MdmrControl.md).

You can also explicitly halt the read operation, using [`M_STOP_READ`](../../Reference/dmr/MdmrControl.md). This call must be done from another thread of higher priority.

No results are returned if the read operation times out or is stopped.
