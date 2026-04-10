---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Deep_learning_OCR
section: Retrieving_results_and_annotation
module_tag: dlocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Deep_learning_OCR / Retrieving results and annotation"
---

# Results and annotations

The results of a read operation ([`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md)) are stored in a Deep Learning OCR result buffer. Most applications typically retrieve and draw various features of these results, using [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md) and [`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md). For example, you can retrieve and draw the resulting string that was read.

## Results

With [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md), you can retrieve global results, string results, or character results. Global results refer to information about the Deep Learning OCR result buffer itself, while string and character results refer to information about the strings and characters in the result buffer.

You can remove all results from a result buffer by calling [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md)with [`M_RESET`](../../Reference/dlocr/MdlocrControl.md). This does not delete the result buffer identifier, as is the case with [`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md). [`M_RESET`](../../Reference/dlocr/MdlocrControl.md) can be a convenient and optimal way of saving memory resources.

### Strings

To retrieve the string that was read, use [`M_STRING`](../../Reference/dlocr/MdlocrGetResult.md).

With [`M_STRING`](../../Reference/dlocr/MdlocrGetResult.md), you will retrieve the string or strings that were read. This result includes the name (symbol) of all the characters in the string and the terminating null character.

When retrieving the results of characters in strings, you must use [`M_INDEX_IN_STRING(n)`](../../Reference/dlocr/MdlocrGetResult.md).

### Typical results to retrieve

In addition to retrieving the actual strings and characters read, you will also typically want to retrieve:

- The total number of strings read ([`M_STRING_NUMBER`](../../Reference/dlocr/MdlocrGetResult.md)).
- The number of characters read for a string ([`M_STRING_CHAR_NUMBER`](../../Reference/dlocr/MdlocrGetResult.md)).
- The name of individual characters in the strings read ([`M_CHAR_NAME`](../../Reference/dlocr/MdlocrGetResult.md)). This returns the name of the character as a string (that is, the name of the character and the terminating null character).
- The score of the strings read ([`M_STRING_SCORE`](../../Reference/dlocr/MdlocrGetResult.md)). The string score quantifies, as a percentage, the confidence that a string has been read correctly. The string score is the average score of the individual characters in the string, which you can also retrieve ([`M_CHAR_SCORE`](../../Reference/dlocr/MdlocrGetResult.md)).
- Information about the bounding box of the strings read, such as height ([`M_STRING_HEIGHT`](../../Reference/dlocr/MdlocrGetResult.md)), width ([`M_STRING_WIDTH`](../../Reference/dlocr/MdlocrGetResult.md)), angle ([`M_STRING_ANGLE`](../../Reference/dlocr/MdlocrGetResult.md)), center ([`M_STRING_POSITION...`](../../Reference/dlocr/MdlocrGetResult.md)), and corners ([`M_STRING_BOX_...`](../../Reference/dlocr/MdlocrGetResult.md)).
  You can also retrieve similar results for each character in the string read ([`M_CHAR_...`](../../Reference/dlocr/MdlocrGetResult.md)). For example, you can retrieve the width of character's bounding box ([`M_CHAR_WIDTH`](../../Reference/dlocr/MdlocrGetResult.md)).
- Information regarding the state of the read operation ([`M_STATUS`](../../Reference/dlocr/MdlocrGetResult.md)).

### Size of string results

Deep Learning OCR returns the [`M_STRING`](../../Reference/dlocr/MdlocrGetResult.md) and [`M_CHAR_NAME`](../../Reference/dlocr/MdlocrGetResult.md) results as a string. To determine the size of the string returned, add [`M_STRING_SIZE`](../../Reference/dlocr/MdlocrGetResult.md) to the requested result. That is, [`M_STRING`](../../Reference/dlocr/MdlocrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dlocr/MdlocrGetResult.md) and [`M_CHAR_NAME`](../../Reference/dlocr/MdlocrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dlocr/MdlocrGetResult.md).

### Positional and dimensional results

If your target image ([`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md)) was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the center of top-left pixel in the target image. To specify this, call [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md). For more information, see [Working with real-world units](../C28_Calibration/S09_Working_with_realworld_units.md).

Positional and dimensional results take into account the angle at which the result was found.

*[Image: MdmrResultStringBoxPosition.png]*

This example shows how the result for the bottom-left corner of the string box ([`M_STRING_BOX_BL_X`](../../Reference/dlocr/MdlocrGetResult.md) and [`M_STRING_BOX_BL_Y`](../../Reference/dlocr/MdlocrGetResult.md)) depends on the location and angle at which the string was read.

### Recommendations to improve results

You can optimize the performance after a successful read in certain situations. Typical situation for which optimization are possible include:

- Unread strings.
- Incorrectly read strings (misidentified characters).
- Low string or character scores.

The following are recommendations to improve reading and get the best results possible.

- Don't have characters touching or cut off at image borders.
- Ensure a contrast level of at least 15 gray levels.
- Set constraints at each character position to ensure the correct character is read at the correct position, using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md).
- Set up the context to read all the strings in the target image, even if you are not interested in all strings. Once strings are read, get the results and define string models of the ones you want, using [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md).
- Set up the string model to read the entire string, even if you are not interested in all of it. Once the string is read, parse out the characters that you don't want.
- Ensure that the detection angle is set correctly, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_ANGLE`](../../Reference/dlocr/MdlocrControl.md).
- Some fonts, such as Arial, Calibri, Liberation Sans, and Cambria perform poorly. Consider using another string reading module if you experience frequently misread characters.
- Ensure that the list of accepted characters includes all of the required characters. Note that by default, punctuation is not included in this list. To inquire the list of accepted characters, use [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrInquire.md).
- Ensure that the character heights respect the set height bounds. If not, adjust the bounds accordingly, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MIN`](../../Reference/dlocr/MdlocrControl.md) and [`M_DETECTION_CHAR_HEIGHT_MAX`](../../Reference/dlocr/MdlocrControl.md).

## Annotations

With [`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md), you can draw specific features of a Deep Learning OCR result buffer in the destination image buffer or 2D graphics list. In general, the drawings that you will want to perform include:

- Drawing a bounding box around the string that was read ([`M_DRAW_STRING_BOX`](../../Reference/dlocr/MdlocrDraw.md)).
  *[Image: MdmrDrawStringBox.png]*
- Drawing a bounding box around the characters that were read ([`M_DRAW_STRING_CHAR_BOX`](../../Reference/dlocr/MdlocrDraw.md)).
  *[Image: MdmrDrawCharacterBox.png]*
- Drawing a circled dot at the center of the bounding box of each string's character ([`M_DRAW_STRING_CHAR_POSITION`](../../Reference/dlocr/MdlocrDraw.md)).
  *[Image: MdmrDrawPosition.png]*
- Drawing the names of the characters that were read ([`M_DRAW_STRING`](../../Reference/dlocr/MdlocrDraw.md)).
  *[Image: MdmrDrawAILFont.png]*
- Drawing the index of the strings that were read ([`M_DRAW_STRING_INDEX`](../../Reference/dlocr/MdlocrDraw.md) shown in red text).
  *[Image: Mdlocrdrawindex.png]*

When applicable, the required information is drawn at the location that the string was read in the target, at the correct angle, scale, and aspect ratio.

## Timeout or stop

The read operation can occasionally take an unexpectedly long time to calculate. To manage this, you can call [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) to specify a timeout value or to stop the read.

Timeout refers to the maximum time that [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) has to read all strings for a context. By default, the maximum time is 10000.0 msec. To change this, use[`M_TIMEOUT`](../../Reference/dlocr/MdlocrControl.md).

You can also explicitly halt the read operation, using [`M_STOP_READ`](../../Reference/dlocr/MdlocrControl.md). This call must be done from another thread of higher priority.

No results are returned if the read operation times out or is stopped.
