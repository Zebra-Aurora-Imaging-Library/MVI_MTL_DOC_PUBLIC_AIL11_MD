---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Creating_and_customizing_the_fonts_for_a_font_based_context
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Creating and customizing the fonts for a font based context"
---

# Creating and customizing the fonts for a font-based context

Once you have allocated a String Reader font-based context, using [`MstrAlloc`](../../Reference/str/MstrAlloc.md) and [`M_FONT_BASED`](../../Reference/str/MstrAlloc.md), you must add at least one font to the font list of that context, and at least one character to the font. By default, the font list and the font are empty.

You can add characters to the font from one of the following:

- **System font**. These characters are referred to as system characters (for example, characters added from your system's Arial font).
- **Source image**. These characters are referred to as user-defined characters (for example, characters added from a mosaic of license plates).

Once a font has been added to the font list of the context, the font is automatically assigned default settings. Typically, these settings are sufficient for many applications. It is recommended that you try the defaults before changing them. However, if the default font settings do not suit your needs, you can:

- Set the font's character type.
- Normalize the font's characters to an appropriate size.
- Set the space size.
- Set the font's character baseline.
- Sort the font's characters.

You can check the font settings by drawing the font characters in an image buffer, using [`MstrDraw`](../../Reference/str/MstrDraw.md).

## Adding and deleting fonts to the context

To add a font to the font list in the String Reader context, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_FONT_ADD`](../../Reference/str/MstrControl.md). The fonts that you add to the String Reader context are empty; that is, they do not contain any characters. You must therefore add them. To do so, see [Adding and deleting characters to the font](S04_Creating_and_customizing_the_fonts_for_a_font_based_context.md).

When a font has been added to the context, it is assigned an index. Fonts are indexed using positive, consecutive integers starting at zero, and increasing by one. You can use the index to access a font. The maximum number of fonts that you can add to a String Reader context is 255.

To control a font setting, you must use [`MstrControl`](../../Reference/str/MstrControl.md) with the [`M_FONT_INDEX()`](../../Reference/str/MstrControl.md) macro set to the index of a specific font, or to all fonts ([`M_FONT_INDEX()`](../../Reference/str/MstrControl.md)).

You can also delete a font or all fonts from the font list. To do so, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_FONT_DELETE`](../../Reference/str/MstrControl.md) set to the index of a specific font, or to all fonts ([`M_ALL`](../../Reference/str/MstrControl.md)). Note that when a font is deleted, its index is reassigned; that is, index values greater than that of the removed font are reduced by one.

Fonts can also be given a user-defined numeric label, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_FONT_USER_LABEL`](../../Reference/str/MstrControl.md). A user label can be used as a means of identifying your font, independently from its index in the String Reader context. However, user labels cannot be used as a direct replacement for the index; to retrieve the index of a font from the user label, use [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_FONT_INDEX_FROM_LABEL`](../../Reference/str/MstrInquire.md). The user label, once converted to an index value, can be used with any String Reader function that takes an index value. All user labels must be unique integers; that is, no two user labels can have the same integer. To remove a label from a font, set the user label to [`M_NO_LABEL`](../../Reference/str/MstrControl.md).

## Adding and deleting characters to the font

Once you have added a font to the context, you must add characters to that font. To do so, use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_FONT_INDEX()`](../../Reference/str/MstrControl.md) and [`M_CHAR_ADD`](../../Reference/str/MstrEditFont.md). You cannot add a character that is smaller than 6x6 pixels. Characters can either be added from your system or from a source image. For more information, see [Character representation](S04_Creating_and_customizing_the_fonts_for_a_font_based_context.md). Note that all operations that apply to the characters of a specific font are done using [`MstrEditFont`](../../Reference/str/MstrEditFont.md).

After a character is added to a font, it can be accessed with its respective font's index and its own character value (ASCII or Unicode). These two values uniquely identify the character. For example, to access the character 'A' in font 0, you must use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with the [`FontIndex`](../../Reference/str/MstrEditFont.md) parameter set to [`M_FONT_INDEX()`](../../Reference/str/MstrEditFont.md), and the [`Param2Ptr`](../../Reference/str/MstrEditFont.md) parameter set to 'A'.

Since characters are partly identified based on their own value, two characters in one font cannot have the same value. If you try to add a character with a value that already exists in a font, the existing character will be replaced by the newly added one, unless you use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_NO_OVERWRITE`](../../Reference/str/MstrEditFont.md). However, multiple fonts within the context can have characters with the same value, each of which is distinguished by its respective font index.

To delete a character in a font, use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_DELETE`](../../Reference/str/MstrEditFont.md), specifying the font's index and the character's value. Instead of passing the character value, you can also pass a null-terminated string array of all the characters to delete, or [`M_NULL`](../../Reference/str/MstrEditFont.md), which will delete all characters in the font.

When you add a character to a font, you can also decide its type. Its type determines whether String Reader treats the character as regular ([`M_REGULAR`](../../Reference/str/MstrEditFont.md)) or as punctuation ([`M_PUNCTUATION`](../../Reference/str/MstrEditFont.md)). Regular characters can typically be categorized as letters or numbers. A linear sequence of regular characters forms a string; that is, each regular character in a string must fall within the Y-size range of every character in that string. Punctuation characters can typically be categorized as characters that are not letters or numbers, such as the quotation ("). To be considered part of the string, a punctuation character must fall within the range of at least one regular character's Y-size. To have String Reader automatically establish the type of the characters, use [`M_AUTO_COMPUTE`](../../Reference/str/MstrEditFont.md). With this setting, String Reader will decide whether the type should be [`M_REGULAR`](../../Reference/str/MstrEditFont.md) or [`M_PUNCTUATION`](../../Reference/str/MstrEditFont.md), based on the character's shape and numerical code. When you add characters to a font, their default character type is [`M_AUTO_COMPUTE`](../../Reference/str/MstrEditFont.md). In the vast majority of cases, [`M_AUTO_COMPUTE`](../../Reference/str/MstrEditFont.md) will choose the correct character type.

## Character representation

Characters in the font are referred to as either system characters or user-defined characters. This is based on how you specify their character representation when you add the characters to the font.

### System characters

System characters are characters that have been added from a given system font, such as Arial. To add characters from a system font, use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_ADD`](../../Reference/str/MstrEditFont.md) and [`M_SYSTEM_FONT`](../../Reference/str/MstrEditFont.md). You must also specify a null-terminated string of the characters to add from the system font, the name of the system font, and the size of the characters to add, in points (for example size 10 font). You can add the regular characters of the font (that is, 'A' to 'Z', 'a' to 'z', and '0' to '9'), as well as punctuation (such as the hyphen or the comma). The system font's name must be provided in standard U.S. English; for example, "Arial", "Arial Bold", "Times New Roman Italic", and "Courier New Bold Italic". You can find the full list of system font names with the Aurora Imaging String Reader Interactive Utility.

### User-defined characters

User-defined characters are characters that have been defined from source images. When the required system font is not available, user-defined characters are the ideal solution, since all you need is an image of the characters in the required font. To add user-defined characters to the font, use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_ADD`](../../Reference/str/MstrEditFont.md) and [`M_USER_DEFINED`](../../Reference/str/MstrEditFont.md). You must also specify the identifier of the characters' source image, and a null-terminated string with the list of characters to associate with the character representations in the image. Note that the size of the characters is automatically determined from the source image.

> **Note:** It is recommended that you use the Aurora Imaging String Reader Interactive Utility to create new characters to add to an existing font.

Typically, the characters in your list represent one string that you want to read. However, you can specify multiple strings to read by adding a space between groups of characters. Generally, you can use multiple strings when one or more of the following situations in the source image are encountered: the characters are not all on the same line, the contrast is clearly different between characters, or there is clearly a horizontal space between groups of characters.

When adding user-defined characters, the character representations in the source image are taken from left to right and from top to bottom and are associated with the corresponding characters in your character list. String Reader is able to locate the characters in the source image because it assumes a Latin-based font. For example, if you want to define an 'A', String Reader will look through the source image from left to right and from top to bottom until the Latin-based character 'A' is located. If it is not located, an error is returned.

For the most part, String Reader's Latin-based font definition is very useful, since it can, for example, tell the difference between a real character and background noise. However, this type of automatic font definition makes it problematic to define non-Latin-based characters, like Chinese letters, or to define an 'I' as a 'W'.

You can, however, choose to add a single user-defined character that bypasses String Reader's automatic Latin-based font definition. To do so, you must combine [`M_USER_DEFINED`](../../Reference/str/MstrEditFont.md) with [`M_SINGLE`](../../Reference/str/MstrEditFont.md). In this case, the entire source image is automatically defined as one character. For example, if your source image depicts the letter 'I', and the character in the character list is the letter 'W', then you have just added a 'W' that looks like an 'I'.

Although this is not mandatory, when adding user-defined characters it is best to provide a high quality source image that ideally contains only the characters to add to the font. A good source image is especially important for a single user-defined character, which is typically added because it is, in some respect, out of the ordinary. The image should also be binarized exactly as you want. Otherwise, it will be binarized by Aurora Imaging Library.

Each potential character in the source image must be entirely connected (except for the accentuated characters, like "è") and should not be merged with other characters or other image objects. Also, the characters you want to add from the source image must all be approximately the same size. Even if the operation succeeds, it is recommended that you draw the characters of the font, using [`MstrDraw`](../../Reference/str/MstrDraw.md), to ensure that every character is defined as expected. For more information, see [Annotation](S13_Retrieving_results_and_annotation.md).

Locating the specified characters in the source image is typically very robust. For example, you do not need to specify the specific size or location of the characters. However, if you are experiencing problems, or if you want to speed up this process, you can create a child buffer or define a rectangular [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI to specify the area that contains the characters to add. For more information, see [Using child buffers, ROIs, or a copy to manipulate specific data areas](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md).

## Source image foreground

When defining characters from a source image and adding them to a font, you must be certain that the foreground is set correctly. By default, the foreground is black. You can, however, change it to white. To alter the foreground, you can combine [`M_USER_DEFINED`](../../Reference/str/MstrEditFont.md) with either [`M_FOREGROUND_BLACK`](../../Reference/str/MstrEditFont.md) or [`M_FOREGROUND_WHITE`](../../Reference/str/MstrEditFont.md). This foreground setting only allows you to set the foreground of the character representations in the source image. This setting does not in any way affect the read operation, as fonts themselves do not have any foreground at all. To read characters in a target image, where the characters have a white foreground, you must explicitly set the read foreground control type of the string model to white; otherwise, you will not be able to read the characters. By default, the foreground used for the read operation is black. For more information, see [Target image foreground](S06_Adding_and_deleting_string_models_to_the_context.md).

## Normalize characters

Whether you are working with system fonts or user-defined fonts, it is not unusual for the characters to have different dimensions. In fact, fonts added from multiple system fonts, or multiple images, often create a rather chaotic group of character sizes; this can make altering some String Reader settings difficult, such as reading within a specific scale range. In this case, you should normalize the characters, with respect to a given X-size or Y-size. Since the other dimension is always scaled to maintain the same aspect ratio, this results in all the characters having the same width or height.

To normalize your characters, you must first decide on the appropriate dimension. This is typically done by obtaining the size of a character with an appropriate height or width, using [`MstrInquire`](../../Reference/str/MstrInquire.md) with either [`M_CHAR_SIZE_X`](../../Reference/str/MstrInquire.md) or [`M_CHAR_SIZE_Y`](../../Reference/str/MstrInquire.md). You can then use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_NORMALIZE`](../../Reference/str/MstrEditFont.md) and either the X-size or Y-size information to normalize the characters in the font. You can normalize a specific character in the font or all characters. In the former case, pass a null-terminated string of the characters to normalize. To normalize all the characters, pass [`M_NULL`](../../Reference/str/MstrEditFont.md) instead. The reference X-size or Y-size must be greater than 8 pixels.

## Space size

The font is partly responsible for establishing the space size required to delimit multiple strings. Specifically, the space size threshold that ultimately delimits strings depends on two settings: the space width ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md)) and the maximum number of consecutive spaces ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md)). The space width is a font setting, while the maximum number of consecutive spaces is a string model setting. By default, the space width is equal to the average X-size (width) of the characters in the font, and the maximum number of consecutive spaces is equal to 3. The space size is calculated using the following equation:

`_SpaceWidth_ x (_MaximumNumberOfConsecutiveSpaces_+1)-1.`

For example, if the average width of the characters is 30, the default space size would be 119 (that is, 30 x (3 + 1) - 1). Therefore, two adjacent characters that are greater than 119 pixels apart are considered part of two separate strings. Note that the space size is relative to the scale of the string.

For more information, see [Space](S07_Space.md).

## Baseline

The baseline typically refers to the imaginary horizontal line on which the majority of the characters in a string rest.

*[Image: BaselineIntro.png]*

When you specify characters from a system font ([`M_SYSTEM_FONT`](../../Reference/str/MstrEditFont.md)), such as TrueType or Postscript, the baseline is, by default, taken from that font's file. In general, the relationship between the bottom of such characters and the baseline of the string is appropriate and there's usually no need to change it.

Conversely, when you specify user-defined characters from an image ([`M_USER_DEFINED`](../../Reference/str/MstrEditFont.md)), there is no inherent baseline. Nevertheless, String Reader will still, by default, automatically compute appropriate baselines for most of these characters. When there is insufficient information to calculate the baseline, as is the case with a hyphen (-), String Reader's default behavior is to ignore that character's baseline. Given your application, ignoring the baseline might or might not be appropriate.

To set a specific baseline value, you must use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_BASELINE`](../../Reference/str/MstrEditFont.md). To explicitly ignore the baseline, you must set [`M_CHAR_BASELINE`](../../Reference/str/MstrEditFont.md) to [`M_NONE`](../../Reference/str/MstrEditFont.md). Even though String Reader ignores an [`M_NONE`](../../Reference/str/MstrEditFont.md) baseline, all other conditions must still be respected when determining whether the character is part of the string, such as the character's scale range constraint.

When specifying a specific baseline value, you must set it to reflect the distance between the bottom of the character and the baseline of the string. If the bottom of the character rests directly on the baseline of the string, which is usually the case, the value that you must set for that character's baseline is 0. However, for characters that dip below the baseline, or rise above it, 0 is not appropriate. Notice the letter 'y', apostrophe, and hyphen characters in the following string when the baseline for each character is set to 0.

*[Image: BaselineZeroCompared.png]*

To move the bottom of a character in an upward direction, you must specify a negative baseline value; to move the bottom of a character in a downward direction, you must specify a positive baseline value. The amount of distance to move the character up or down is set as a percentage of the character's height. For example, to move a character up by a distance equal to twice the character's height, you must set its baseline to -200%, while to move a character down by a distance equal to 1/4 of its height, you must set its baseline to 25%.

*[Image: BaselineNegativeForSymbol.png]*

To reset characters to their automatically computed baseline, use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_BASELINE`](../../Reference/str/MstrEditFont.md) and [`M_AUTO_COMPUTE`](../../Reference/str/MstrEditFont.md), and specify a null-terminated string of the characters.

## Sorting characters

Characters are not always added to the font in an orderly fashion; particularly user-defined characters. It can therefore be difficult, when displaying these characters, to quickly tell which ones are missing. Also, you might want to have your characters ordered by sub-lists, such as all the digits, followed by all the letters, followed by all the symbols. To solve these problems, and any others regarding the order of your characters, String Reader provides a sort functionality.

To sort the characters in the font, according to their character values, in either ascending or descending order, use [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_SORT`](../../Reference/str/MstrEditFont.md) and either [`M_ASCENDING`](../../Reference/str/MstrEditFont.md) or [`M_DESCENDING`](../../Reference/str/MstrEditFont.md), and a null-terminated string of the characters.
