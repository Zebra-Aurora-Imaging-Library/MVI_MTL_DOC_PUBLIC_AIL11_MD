---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Retrieving_and_analyzing_the_results
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Retrieving and analyzing the results"
---

# Retrieving and analyzing the results

After having potentially located the string in your target image using [`MocrReadString`](../../Reference/ocr/MocrReadString.md) or [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md), you can extract the required results from your result buffer using [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md). All OCR results are based on the concept of a character. A string is a group of characters that reside along the same line along the same principal axis and the same angle. Multiple strings are a group of strings that are to be read together.

Results are returned in the order in which they are read/verified, from left to right. Always check the validity of the string to ensure that the match score is greater than or equal to the acceptance level, using [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md) with [`M_STRING_VALID_FLAG`](../../Reference/ocr/MocrGetResult.md). Additional checks against the actual length read and the intended length could be made to assure accuracy.

Invalid characters in the resulting strings can be replaced with an ASCII character using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_INVALID`](../../Reference/ocr/MocrControl.md). Note that this control type must be set before performing the read/verify operation.

## A character

Character data is returned for each character in the string or strings. This data includes:

- Position along the X- and Y-axis.
- The height and width of the character.
- The match score of the character.
- The spacing of the character.
- The validity of the character (based on the character's acceptance level).
- The ASCII version of the character.

## A string

String data is returned for each string. String data includes:

- The contents of the string.
- The angle of the string.
- The length of the string.
- The match score of the string.
- The validity of the string (based on the string's acceptance level).

## A text

When reading/verifying multiple strings, text data is returned for all the strings. The text data includes:

- All the characters read, even if they are on different lines in the target image.
- The number of characters read in total.
- The match score for the entire text.
- The threshold value used to binarize the target image. Note that this is only available when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, multiple strings, and/or the string has a few degrees of rotation.

> **Note:** All the results for a single string are also available for each string in the entire text.

## Understanding odd results

Unexpected results can come from one of the following scenarios.

- If characters are vertically overlapping (or touching).
  Create a child buffer that contains the characters below the point where they overlap. This effectively removes the overlapping portion of the characters.
  > **Note:** The more that characters overlap vertically, the less chance for a valid result.
  To read a target image containing multiple lines of text, Aurora Imaging Library OCR requires that there is enough space between lines. If lines are too close together (vertically), the robustness of the search will suffer.
  Use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TOUCHING_CHAR`](../../Reference/ocr/MocrControl.md) to read horizontally touching characters.
- If the string or strings have non-uniform inter-character spacing.
  Read/verify the string with an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context type and use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md) set to [`M_ANY`](../../Reference/ocr/MocrControl.md).
  > **Note:** This setting is best suited to read non-uniformly spaced characters.
  Create a child buffer to contain the most extreme spacing examples for best results.
- If the string or strings in the target image are greatly disparate.
  Create a child buffer for each different region in position, angle, and/or length. Read each region individually for best results.
  > **Note:** The more similar the strings in angle, position, and length, the faster and more robust the search.
  If the spacing, length or angle differ, use different Aurora Imaging Library OCR font contexts for each child buffer, with their operational and processing controls configured appropriately for each situation. For more information, see [Defining the target strings](S06_Defining_the_target_string.md).
- If the target image contains multiple lines with different lengths.
  Create a child buffer that contains the area with the shorter strings and read the shorter strings, using a different OCR font context, into a different OCR result buffer.
  > **Note:** Strings of similar length are easiest to locate.
  Make sure that the target image has a consistent inter-line spacing, and that all lines start at a similar location along the X-axis, for best results.
- If the target image contains blanks.
  Verify that your string length is correct.
  > **Note:** Your string length determines the number of characters sought.
  If the problem persists and the ability to read blank spaces is enabled (use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md)) remember to include them in your string length (use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_CHAR_NUMBER`](../../Reference/ocr/MocrControl.md)).
- If the target image contains a lot of non-character blobs.
  Try to improve your target image before trying to read/verify again. For more information, see[Defining the target strings](S06_Defining_the_target_string.md).
  > **Note:** An image that does not include blobs of equal intensity as the foreground will return best results.
  Use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_MORPHOLOGIC_FILTERING`](../../Reference/ocr/MocrControl.md) to internally enhance the contrast of the image.
- If the target image contains a string longer or shorter than what was expected.
  > **Note:** Aurora Imaging Library OCR will try to return the number of characters that you set as existing in the target image using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_CHAR_NUMBER`](../../Reference/ocr/MocrControl.md).
  If your string length is set to a value larger than the actual length of the string in the target image, and if using [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, Aurora Imaging Library will return the best matches found and stop searching after it has found the specified number of characters. If using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context, the string length determines the exact number of characters found, even if this means returning characters with low match scores.
- If the target string is at an angle.
  Verify that the principal axis of the entire text is equal to the expected angle (nominal angle), specified using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md).
  > **Note:** A string at an angle not equal to 0 takes longer to find.
  Create a target image with a string angle of 0. If the string is properly read, your original application did not follow the expected use of string angle.

## Hooking functions

Aurora Imaging Library OCR allows you to attach or detach a user-defined function to an Aurora Imaging Library OCR event when the specified OCR font context is used. This can be used to impose global string constraints, and can be used to implement custom checksum functions or to reject strings that would have otherwise met the character constraints imposed.

Once your user-defined function is created, use [`MocrHookFunction`](../../Reference/ocr/MocrHookFunction.md) to attach it to the validation of a string. It will then execute during the last stage of either [`MocrReadString`](../../Reference/ocr/MocrReadString.md) or [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md). When [`MocrHookFunction`](../../Reference/ocr/MocrHookFunction.md) is used in conjunction with an [`M_SEMI...`](../../Reference/ocr/MocrAllocFont.md) OCR font context type, the function being hooked will replace the default validation function.
