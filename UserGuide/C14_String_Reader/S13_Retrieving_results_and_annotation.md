---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Retrieving_results_and_annotation
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Retrieving results and annotation"
---

# Retrieving results and annotation

After adding at least one string model and one font to the String Reader context, and performing a successful preprocess and read operation, you would typically retrieve and draw various features of your results. To do so, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) and [`MstrDraw`](../../Reference/str/MstrDraw.md). These functions offer many different types of results and draw operations. For example, you can retrieve the aspect ratio of the characters, using [`M_CHAR_ASPECT_RATIO`](../../Reference/str/MstrGetResult.md), or draw a cross at the center of the bounding box of each string's character, using [`M_DRAW_STRING_CHAR_POSITION`](../../Reference/str/MstrDraw.md).

To return the index of the font of each individual character within the string, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_CHAR_FONT`](../../Reference/str/MstrGetResult.md). To return the index of the character in the font for each individual character within the string, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_CHAR_INDEX`](../../Reference/str/MstrGetResult.md). In either case, the index returned is the index value at the moment the [`MstrRead`](../../Reference/str/MstrRead.md) operation is performed. Therefore, if the font list changes after the read operation has been called (for example, you add or remove a character), the index value that you retrieve might not be valid.

## Annotation

The String Reader module allows you to draw specific features of the String Reader context or String Reader results, using [`MstrDraw`](../../Reference/str/MstrDraw.md). For example, you can draw your string results to ensure that the read operation has read the correct characters.

After each drawing operation, the optimal image size that was needed for that operation is updated. This information can be inquired using [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_DRAW_LAST_SIZE_X`](../../Reference/str/MstrInquire.md) and [`M_DRAW_LAST_SIZE_Y`](../../Reference/str/MstrInquire.md). You can also update these size constants without actually performing the drawing operation; to do so, set the destination image of [`MstrDraw`](../../Reference/str/MstrDraw.md) to [`M_NULL`](../../Reference/str/MstrDraw.md). This can be useful if you, for example, want to get the optimal image buffer size for the drawing operation.

All characters are drawn according to their draw box margin, which you can set using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_DRAW_BOX_MARGIN_X`](../../Reference/str/MstrControl.md) and [`M_DRAW_BOX_MARGIN_Y`](../../Reference/str/MstrControl.md). These margins determine the amount of white space between each character that is drawn. The default value is 3. Note that a character has no inherent margin; that is, if these margin values are set to 0, the characters are drawn, one after the other and one above the other, with no white space.

*[Image: DrawBoxMarginExample.png]*

For a String Reader context, you can draw a character representation of the font in the destination image, using [`MstrDraw`](../../Reference/str/MstrDraw.md) with [`M_DRAW_CHAR`](../../Reference/str/MstrDraw.md). This is useful to ensure that the characters added to the font are correct. To do so, you must specify the explicit list of characters to draw and verify. Characters are drawn from left to right, and from top to bottom. Characters that fall outside the destination image are clipped. To draw the characters on multiple lines, a space character must be inserted to separate the strings included in the null-terminated string.

It might be useful to sort the characters before you draw them, thereby making it easier to visually ascertain if all the required characters are there. For more information, see [Sorting characters](S04_Creating_and_customizing_the_fonts_for_a_font_based_context.md).

String Reader offers numerous drawing operations, which, if required, can be combined to draw multiple features simultaneously. For example, you can draw the characters of the string read, a box around the string read, and a cross at the center of the bounding box of each string's character (the character position). The following image shows how drawing the position of characters of a string ([`M_DRAW_STRING_CHAR_POSITION`](../../Reference/str/MstrDraw.md)) for a typical Quebec license plate is displayed:

*[Image: DrawStringCharPositionExample.png]*

For a full list of drawing operations, see [`MstrDraw`](../../Reference/str/MstrDraw.md). Note that characters are always drawn at the location read in the target with the correct angle, scale and aspect ratio.

## Transformation coefficients

In addition to many other kinds of results, String Reader provides you with forward and reverse transformation coefficients. These results allow you to transform any point from the character's source image (the image from which the characters were defined) to its corresponding position in the target image (forward), or from the read position in the target image to its corresponding position in the character's source image (reverse). With this information, the equivalent position of any point of any character can be drawn.

For more information, see the transformation coefficients table in [`MstrGetResult`](../../Reference/str/MstrGetResult.md).

## Formatted and non-formatted results

Unlike String Reader, humans have a hard time processing a long string of characters uninterrupted by any space, or any indication of where one string is separated from another. To make strings easier to read, you can display formatted results.

To retrieve the string as a formatted result, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md). A space character is inserted in the string each time the distance between two adjacent characters exceeds the space width ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md)), at the scale of the string.

You can also retrieve the entire text read as a formatted result, using [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_TEXT`](../../Reference/str/MstrGetResult.md). In addition to inserting space characters, string separators are also inserted in the text each time the distance between two adjacent characters exceeds the space size, which depends on the space width (at the scale of the string) and the maximum number of consecutive spaces ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md)). For more information, see [Space size](S04_Creating_and_customizing_the_fonts_for_a_font_based_context.md). Both the space character and the string separator must be set before the string is read.

To retrieve the non-formatted string (no spaces or string separators added), use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_STRING`](../../Reference/str/MstrGetResult.md).

Regardless of which result is being returned, strings are always ordered in the natural Latin-based reading order. This is the order that you read text written in a Latin-based language (left to right), such as Canadian English.

As mentioned earlier in this chapter, the space is not considered a character, and is not a part of any string that has been read. However, String Reader does mark the existence of a space, and can therefore artificially insert a space and/or a string separator at the correct place. For more information, see [Space](S07_Space.md).

The default character used as the space character is ASCII 32, which is a space. You can, however, set a different character, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_CHARACTER`](../../Reference/str/MstrControl.md). Depending on the type of character encoding used by the String Reader context (specified [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md)) any character of that specified type (either ASCII or Unicode) can be used as the space character. You can also set [`M_SPACE_CHARACTER`](../../Reference/str/MstrControl.md) to [`M_NONE`](../../Reference/str/MstrControl.md), which specifies no space character.

The default string separator is ASCII 10, which is a new line. Therefore, when retrieving the entire text ([`M_TEXT`](../../Reference/str/MstrGetResult.md)), each string will be on a separate line, which makes it ideal for printing purposes. You can, however, control the string separator character, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SEPARATOR`](../../Reference/str/MstrControl.md). Depending on the type of character encoding used by the String Reader context (specified [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md)) any character of that specified type (either ASCII or Unicode) can be used as the string separator. You can also set [`M_STRING_SEPARATOR`](../../Reference/str/MstrControl.md) to [`M_NONE`](../../Reference/str/MstrControl.md), which specifies no separator character.

You can use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) to get the size of the formatted string ([`M_FORMATTED_STRING`](../../Reference/str/MstrGetResult.md) + [`M_STRING_SIZE`](../../Reference/str/MstrGetResult.md)), the size of the non-formatted string ([`M_STRING`](../../Reference/str/MstrGetResult.md) + [`M_STRING_SIZE`](../../Reference/str/MstrGetResult.md)), and the size of the entire formatted text ([`M_TEXT`](../../Reference/str/MstrGetResult.md) + [`M_STRING_SIZE`](../../Reference/str/MstrGetResult.md)). When getting the size of the formatted string, the string's characters, the inserted space characters, and the null-terminated character are included. When getting the size of the non-formatted string, only the string's characters are included. When getting the entire formatted text, all the strings' characters, all the inserted space characters, and all the string separators are included; null-terminated characters are not included.
