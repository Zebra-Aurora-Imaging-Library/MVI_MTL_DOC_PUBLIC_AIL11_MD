---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Space
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Space"
---

# Space

When using the Aurora Imaging Library String Reader module, the space is not considered a normal character. For example, you can never read a space with String Reader, nor can you set a space as a constraint to which a valid character must adhere, to be read (for more information on character constraints, see [Rules for character placement](S10_Rules_for_character_placement.md)).

The space is used to delimit multiple strings in the target image when performing a read operation ([`MstrRead`](../../Reference/str/MstrRead.md)). That is, the space size establishes the maximum distance between two adjacent characters before each character is considered part of two separate strings. The space size is calculated using the space width (a font setting) and the maximum number of consecutive spaces (a string model setting). For more information, see [Space size](S04_Creating_and_customizing_the_fonts_for_a_font_based_context.md).

Note that, like the space, the carriage return also signifies a new string. If a character is on another line, it is automatically part of another string.

If the space was treated like a normal character, there would be tricky issues to contend with, such as distinguishing the space from noise. By defining a space as the marker that separates strings, there is less chance for an incorrect read operation. For example, consider an ANPR application that must read the following target image:

*[Image: LicWithFlag.png]*

Since most license plates do not have any symbol between characters, and the ones that do cannot be ignored, multiple types of spaces might have to be considered for this string to be read correctly. You would not only have to account for the space that occurs between strings, but how this space might vary when noise exists (in this case, the flag). Essentially, it is simpler to decide whether or not the characters exist, rather than deciding whether or not a space exists.

When a space is encountered in the target image, it is marked by String Reader and can be artificially inserted to display formatted results. For more information, see [Formatted and non-formatted results](S13_Retrieving_results_and_annotation.md).

## Space width

To set the width of the space character in the font, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md). By default, the space width is set to [`M_MEAN_CHAR_WIDTH`](../../Reference/str/MstrControl.md), which is equal to the average X-size (width) of the characters in the font. You can also set the space width to [`M_INFINITE`](../../Reference/str/MstrControl.md), which is equal to an infinite amount of space. That is, all characters on the same line are always part of the same string. In this case, however, there will never be a space character in the formatted string since the distance can never be large enough to constitute a space character. Other settings for the space width include the maximum X-size or minimum X-size character in the font, and a quarter of the maximum character width in the font. You can also set the space width to a specific value, in pixels.

The space width that you set is relative to the font. Therefore, the space width is considered to be at the size of the font (that is, the point size of the characters in the font), and adjusted according to the scale and aspect ratio of the string.

## Maximum number of consecutive spaces

To set the maximum number of consecutive spaces required for two adjacent characters to be part of two separate strings, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md). The default value is 3 consecutive spaces.

Note that [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md) is a string model setting, while [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md) is a font setting. Therefore, each string model can have a maximum number of consecutive spaces, while all string models must adhere to their font's space width.

The notion of consecutive spaces is important, since non-consecutive spaces do not cause a split in the string. Consider the following Quebec license plate.

*[Image: ConsecutiveSpaceExample.png]*

In this example, the first 3 characters are separated by a single space each, which makes them part of the same string. However, following the letter 'N', there are several consecutive spaces; if you set a maximum of one consecutive space, the '9' would be part of a different string. Based on the maximum number of spaces, this license plate can either be read as two strings, or one string with a large space between 'N' and '9'.
