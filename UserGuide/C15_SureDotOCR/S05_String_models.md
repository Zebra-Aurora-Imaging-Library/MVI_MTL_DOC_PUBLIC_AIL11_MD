---
doctype: UserGuide
part: "2D processing and analysis"
chapter: SureDotOCR
section: String_models
module_tag: dmr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / SureDotOCR / String models"
---

# String models

A string model is a data structure, within a SureDotOCR context, that stores the requirements that a dot-matrix string in a target image must meet, for the string to be read. These requirements can range from general to specific; they can apply to the string models themselves, or to the different string model positions. For example, you can create a string model to read any string from a target image that contains any two characters from any font in a context, or you can create a string model to read a string that contains the numbers '4' and '2' from the _Galaxy_ font.

A SureDotOCR context must contain one or more string models. To add a string model to a context, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_STRING_ADD`](../../Reference/dmr/MdmrControl.md). To delete a string model from a context, use [`M_STRING_DELETE`](../../Reference/dmr/MdmrControl.md). You can also call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) to control context settings related to string models. To inquire about such settings, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md). To control and inquire about string models themselves, call [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) and[`MdmrInquireStringModel`](../../Reference/dmr/MdmrInquireStringModel.md), respectively. Context settings related to string models apply to all string models in that context.

For string model modifications to take effect, you must preprocess the context by calling [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md).

## Required settings

Before reading dot-matrix text from a target image with [`MdmrRead`](../../Reference/dmr/MdmrRead.md), you must specify the following settings related to string models:

- Dot diameter.
- Text block dimensions.
- Minimum and maximum number of characters.

Ideally, the string in the target image should adhere to each of these settings. If it doesn't, the string might still be read, although possibly with a lower resulting score. For more information, see [Acceptance and certainty](S05_String_models.md).

### Dot diameter

The dot diameter refers to the size of the dots that represent the characters of the strings in the target.

*[Image: MdmrDotDiameter.png]*

To set the diameter, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md). The diameter is set for a context and applies to every dot in all strings read for each string model in that context.

The dots that make up the characters in the image must be visually distinguishable, both vertically and horizontally. If dots are merged into a solid bar, as seen in the following image, there can be difficulties reading.

*[Image: MdmrDotDiameterSpacing.png]*

When dots are merged (that is, you cannot perceive the bump of each dot) due to low resolution or poor focus, improve the image's quality. If a string's characters are printed such that their center to center spacing is less than the dot diameter, it might not be feasible to read such strings reliably.

SureDotOCR can tolerate a specified amount of spread of the dots. To enable a tolerance for spread, enable [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_DOT_DIAMETER_SPREAD_MODE`](../../Reference/dmr/MdmrControl.md). Then, specify the tolerance for the size of the dot diameter using [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md). Specify the tolerance as a value centered at the [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md). In the image below, [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md) is set to 4.0 and [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md) is set to 2.0. This will allow dots with a diameter between 3.0 and 5.0.

*[Image: Mdmr_DotSpread.png]*

When [`M_DOT_DIAMETER_SPREAD_MODE`](../../Reference/dmr/MdmrControl.md) is enabled, it is recommended that you also set the dot diameter step using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_DOT_DIAMETER_STEP`](../../Reference/dmr/MdmrControl.md). The diameter step is the increment between steps SureDotOCR will use when checking the range of diameters set by [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md). For example, if [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md) is set to 4.0, [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md) is set to 2.0, and [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md) to 0.2, SureDotOCR will check 3.0, 3.2, 3.4, 3.6,... 4.6, 4.8, 5.0 until it finds.

### Text block dimensions

The text block refers to a rectangle that encloses all dot-matrix text in the target image buffer (or in the buffer's associated region of interest).

*[Image: MdmrStringBox.png]*

To set the text block dimensions, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_TEXT_BLOCK_WIDTH`](../../Reference/dmr/MdmrControl.md) and [`M_TEXT_BLOCK_HEIGHT`](../../Reference/dmr/MdmrControl.md). Set the width and height according to the orientation of the string(s) to read in the text block. The width is oriented along the axis that represents the string's base; while the height is oriented along the vertical axis that is perpendicular to the string's base.

*[Image: MdmrStringBoxAngle.png]*

The text block dimensions are set for a context and must enclose all dot-matrix text in the image or region of interest, if one exists. SureDotOCR establishes the center of the text block automatically. The text block should be smaller than the rectangular region of interest ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)), if the target image specified with [`MdmrRead`](../../Reference/dmr/MdmrRead.md) has one.

### Maximum and minimum number of characters

The number of characters refers to the size of the string to read in the target.

*[Image: MdmrNumberOfCharacters.png]*

To set the number of characters, call [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_STRING_SIZE_MIN_MAX`](../../Reference/dmr/MdmrControlStringModel.md).

Each character in a string that you expect to read represents a position in the string model. For information about how to constrain the different positions to limit the characters permitted, see [Constraining string model positions with permitted characters](S05_String_models.md).

## Optional settings

To fine tune how SureDotOCR reads dot-matrix text from a target image, you can specify the following settings related to string models:

- Foreground.
- Intensity and contrast.
- Acceptance and certainty.
- Rank.
- Number of strings to read.
- String angle.
- Italic angle.

### Foreground

Foreground refers to whether the dots that represent the characters of the strings in the target are darker or lighter than the background.

*[Image: MdmrForeground.png]*

By default, SureDotOCR assumes characters are darker than the backgrounds (for example, black dots on a white surface). To modify this, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/dmr/MdmrControl.md). The foreground is set for a context and applies to all strings read for each string model in that context.

### Intensity and contrast

Intensity refers to the average grayscale value of the pixels that make up the character's dots (foreground) in the strings. Since target images must be 8-bit, the highest possible intensity of a dot is 255 (white). The lowest possible intensity is 0 (black).

*[Image: MdmrIntensity.png]*

Contrast refers to the difference in pixel intensity between a character and its background. The greater the difference, the higher the contrast. The following dot-matrix strings, from left to right, show a decrease in contrast.

*[Image: MdmrContrast.png]*

If a valid character exists in the target image, it can typically be read with the default intensity and contrast settings. However, if the read operation is proving problematic, or you want to try and speed up the read time, you can adjust these settings.

To modify the intensity requirements, set [`M_MIN_INTENSITY_MODE`](../../Reference/dmr/MdmrControl.md) or [`M_MAX_INTENSITY_MODE`](../../Reference/dmr/MdmrControl.md) to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md). You can then specify a minimum or maximum intensity value with [`M_MIN_INTENSITY`](../../Reference/dmr/MdmrControl.md) or [`M_MAX_INTENSITY`](../../Reference/dmr/MdmrControl.md), respectively. Valid values are between 0 and 255, inclusive. The default minimum is 0; the default maximum is 255.

To modify the contrast requirements, set [`M_MIN_CONTRAST_MODE`](../../Reference/dmr/MdmrControl.md) to [`M_USER_DEFINED`](../../Reference/dmr/MdmrControl.md), and specify a minimum contrast value with [`M_MIN_CONTRAST`](../../Reference/dmr/MdmrControl.md). Valid values are between 0 and 255, inclusive. The default value is 15. The default contrast mode is [`M_AUTO`](../../Reference/dmr/MdmrControl.md), which automatically establishes the minimum contrast. Note that with [`M_AUTO`](../../Reference/dmr/MdmrControl.md), SureDotOCR will not read characters with a contrast less than 15.

SureDotOCR ignores potential character data in the target image that does not have an intensity that falls within the specified min and max range or that does not have a contrast greater than or equal to the specified minimum value. Although this can be useful to fine tune your results and potentially decrease read time, particularly in the presence of noise and in unusually contrasted images, be aware that improperly ignoring image data can cause SureDotOCR to ignore actual characters. Intensity and contrast are set for a context and apply to all strings read for each string model in that context.

### Acceptance and certainty

Acceptance and certainty refer to string model settings that regulate when a target string can be read (acceptance) and when a target string must be read (certainty), based on the resulting score of the string and its characters.

When you call [`MdmrRead`](../../Reference/dmr/MdmrRead.md), SureDotOCR analyzes the target image and localizes potential character candidates for each string model. The best candidates that meet the minimum definition of a string (a linear sequence of generally aligned characters) and all user-specified settings will be read.

Every character candidate is given a score between 0 and 100. The higher the score, the better the candidate. This score quantifies, as a percentage, the similarity between the character in the target string and the corresponding character in the font that the string model uses. The string score is the average score of all the characters in the string. To retrieve these scores, call [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_CHAR_SCORE`](../../Reference/dmr/MdmrGetResult.md) and [`M_STRING_SCORE`](../../Reference/dmr/MdmrGetResult.md).

By default, the acceptance level for the score of both the string and its characters is 50%. To modify these levels, call [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_STRING_ACCEPTANCE`](../../Reference/dmr/MdmrControlStringModel.md) or [`M_CHAR_ACCEPTANCE`](../../Reference/dmr/MdmrControlStringModel.md).

Strings can only be read if the string score, and the score of every character, is greater than or equal to their respective acceptance level. For example, if a string has three characters, and the first two have a score of 55%, and the last has a score of 40%, the string's score is 50%, which meets the string's default acceptance level. However, since one character does not meet the default character acceptance level, this string will not be read. To read this string, you can lower the character's acceptance level to 40%, without having to lower the acceptance level of the string.

By default, the certainty level is 70%. If a string's score is equal to or above this level, the string is immediately read as a valid string result, without processing the rest of the target image for strings with higher scores (provided the specified number of strings to read has been read). To modify the certainty level, call [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_STRING_CERTAINTY`](../../Reference/dmr/MdmrControlStringModel.md).

Acceptance and certainty are set for string models; each one can have their own levels. You should typically set the acceptance to represent a threshold for a score that is adequate, though you would like to check the rest of the image for strings with better scores. The certainty level represents a threshold for a score that is so amazingly good, you need not even consider the possibility that there is a better string out there. Since lowering the certainty usually results in less image analysis, it can speed up the read operation. Remember to set the certainty carefully, since a low level can cause a premature termination of the read operation, and allow the best candidates to remain unread.

The default acceptance and certainty settings are generally fine. However if you are not getting the results you want, try experimenting with them. For example, changing the acceptance might be necessary for images with poorly printed characters. When used in concert, these settings offer a simple, fast, and versatile way of accepting and rejecting strings.

### Rank

Rank refers to the order to read for a string model, relative to the other string models in a context.

SureDotOCR reads strings from left to right, and from top to bottom, with respect to the angle of the target. Following this order, the strings read in the illustrations below are "JOHN", "PAUL", "GEORGE", and "RINGO".

*[Image: MdmrRank.png]*

Rank 0 (the default) indicates that the string for that string model should be read first; accounting for angle, it will be the top left-most string. In this example, that first string is "JOHN". Rank is only relevant when there are multiple string models in a context.

To specify a string model's rank, call [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_STRING_RANK`](../../Reference/dmr/MdmrControlStringModel.md). String models with lower ranks are read before those with higher ones. Specifying a rank order can make the read operation faster and more accurate. Ranks are set for string models; each one can have its own value. Strings with different ranks must be on separate lines.

If multiple string models have identical ranks, SureDotOCR reads just one string, depending on which is more similar to its string model. For example, if a context has string models with ranks set to [0, 0, 0, 1], only one of the three string models with rank 0 can have a string read for it.

When specifying ranks, you must include rank 0, and you must not skip successive values. For example, if you have four string models with their respective ranks set to [1, 2, 3, 4], you will get an error, because you did not include rank 0. You will also get an error if you set their ranks to [0, 1, 2, 4], because you included ranks 2 and 4 but skipped rank 3. Examples of acceptable rank settings are [0, 1, 2, 3], [0, 0, 0, 0], [2, 2, 1, 0], and [2, 1, 0, 1].

### Number of strings to read

SureDotOCR establishes the required number of strings to read for a context according to the rank of its string models ([`M_STRING_RANK`](../../Reference/dmr/MdmrControlStringModel.md)). That is: `_TotalNumberOfStringsToRead_ = _HighestRankValue_ + 1`. For example, if you have four string models with ranks set to [0, 0, 1, 2], SureDotOCR must read 3 strings. If 3 strings cannot be read, no results are returned.

Since the default rank for every string model is 0, SureDotOCR tries to read 1 string, regardless of how many string models are in a context, unless you modify rank. In all cases, a maximum of 1 string can be read for each string model.

### Space

Space generally refers to the distance between characters. To help deal with spaces in target strings, SureDotOCR allows you to control space settings for string models in a context, by calling [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md) and [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md). Establishing these settings properly can help differentiate characters in the same word, characters in different words but the same string, and characters in different strings.

By default, the minimum width of a space character is equivalent to the maximum character width in the target string. The default maximum width of a space character is equivalent to three times the maximum character width in the string.

The following table shows how SureDotOCR interprets the actual space in the target between one character and another.

| Distance between characters | Interpretation of characters on either side of the space |
| --- | --- |
| `_MinimumSpaceWidth_ &lt;= _CharacterDistance_ &lt;= _MaximumSpaceWidth_` | SureDotOCR considers the characters to be part of separate words in a string, and can insert a space character between them. The width of this space is a value between _MinimumSpaceWidth_ and_MaximumSpaceWidth_, depending on the actual distance between the characters in the target. |
| `_CharacterDistance_ &lt; _MinimumSpaceWidth_` | SureDotOCR considers the characters to be part of the same word, and will not insert a space character. |
| `_CharacterDistance_ > _MaximumSpaceWidth_` | SureDotOCR considers the characters to be part of separate strings. |

Space values are set for a context, so they apply to all strings read for all string models in that context.

To disable space settings, set [`M_SPACE_SIZE_MIN_MODE`](../../Reference/dmr/MdmrControl.md) and [`M_SPACE_SIZE_MAX_MODE`](../../Reference/dmr/MdmrControl.md) to [`M_DISABLE`](../../Reference/dmr/MdmrControl.md). If you disable the maximum space width, no amount of space between aligned characters can cause them to be part of separate strings. This does not apply to characters on separate lines or severely misaligned characters. If you disable the minimum space width, no amount of space between characters in the same string can cause SureDotOCR to recognize the characters as part of separate words; that is, SureDotOCR will not recognize an actual space character.

If necessary, SureDotOCR allows you to specify a space character as a permitted character constraint. For more information, see [Reading space](S05_String_models.md).

### String angle

By default, the angle of a string is detected automatically ([`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_STRING_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md) set to [`M_AUTO`](../../Reference/dmr/MdmrControl.md)). However, you can explicitly specify the nominal angle at which to read a string. To do so, you must first set the angle mode ([`M_STRING_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md)) to [`M_ANGLE`](../../Reference/dmr/MdmrControl.md); you can then specify the angle using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_STRING_ANGLE`](../../Reference/dmr/MdmrControl.md). Valid values for the angle are specified in degrees, and can range from 0 to 360. The string angle can be set relative to the X-axis of the pixel coordinate system or relative coordinate system; to specify which one, set [`M_STRING_ANGLE_INPUT_UNITS`](../../Reference/dmr/MdmrControl.md) to[`M_PIXEL`](../../Reference/dmr/MdmrControl.md) or [`M_WORLD`](../../Reference/dmr/MdmrControl.md), respectively. You can also set the angle to the same angle as the target image's region of interest by setting the angle to [`M_ACCORDING_TO_REGION`](../../Reference/dmr/MdmrControl.md). A benefit of setting the string angle explicitly is that inspection speed increases if a string angle is chosen and the fixture angle is known.

Alternatively, you can set that the string can be read from left to right with the characters facing upward, or from right to left with the characters facing downward, by setting [`M_STRING_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md) to [`M_ORIENTATION`](../../Reference/dmr/MdmrControl.md). This is useful if the product can be rotated 180 degrees, and this rotation does not affect the acceptability of a product ( for example, if the product is placed backwards at an inspection point but is still a good part).

### Italic angle

SureDotOCR can also read characters at a specified angle (skew). To do so, you must first set the character angle mode, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_ITALIC_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md), to [`M_ANGLE`](../../Reference/dmr/MdmrControl.md); then, set [`M_ITALIC_ANGLE`](../../Reference/dmr/MdmrControl.md) to the required angle in degrees. Valid values range from -90 to 90 degrees, relative to a line perpendicular to the angle of the string. The Y-axis is the zero reference (instead of the X-axis), and a positive angle searches for the characters leaning counter-clockwise at a specified angle (left-leaning); whereas, a negative angle searches for the characters leaning clockwise at the specified angle (right-leaning). If [`M_ITALIC_ANGLE`](../../Reference/dmr/MdmrControl.md) is set to 0, SureDotOCR searches for non-italic characters. A benefit of setting an italic angle explicitly is that inspection speed increases if italic angle is chosen and the fixture angle is known. When[`M_STRING_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md) is set to [`M_DEFAULT`](../../Reference/dmr/MdmrControl.md), [`M_ITALIC_ANGLE_MODE`](../../Reference/dmr/MdmrControl.md) must also be set to [`M_DEFAULT`](../../Reference/dmr/MdmrControl.md).

*[Image: Suredot_stringangle.png]*

## Constraining string model positions with permitted characters

As previously discussed, any character from any font can, by default, be read at any string model position. For example, if you have two string models (_StringGood_ and _StringBetter_), each with a maximum and minimum number of characters of two, and you have two fonts (_FontNice_ and_FontNicer_), each with the characters '2' and '4', the strings '24', '42', '22', or '44' can be read from the target image, where each '2' and '4' can correspond to how they are represented in either font.

*[Image: MdmrPermittedCharsEx1.png]*

To restrict the characters that can be read, according to a specified character type and font, call [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md). This can be considered defining the string's grammar. For example, you can specify that for _StringGood_, a target string can only be read if the character at position 0 is a '4' from _FontNice_, and the character at position 1 is a '2' from _FontNice_. Similarly, for _StringBetter_, you can specify that a string can only be read if the character at position 0 is a '4' from _FontNicer_and the character at position 1 is a '2' from _FontNicer_.

*[Image: MdmrPermittedCharsEx2.png]*

When permitting explicit characters to be read, use [`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_CHAR_LIST`](../../Reference/dmr/MdmrControlStringModel.md) and specify the list of character names, such as "42". If you are using Windows API functions with the Unicode macro, and your source code is also Unicode, you can specify the characters directly in Unicode. Be aware that each character in the list must exist in the specified font. You can also specify character names in hexadecimal format beginning with "\\x", such as "\\x34\\x32" instead of "42". Hexadecimal notation is necessary if your data is in an ASCII format and you want to have Unicode characters beyond the Basic Latin range. For example, Basic Latin does not include the smiley face character; to specify it, use "\\x263A". To list a string of character names with mixed notation, you should also use "\\x" (for example, "ADAMS\\x34\\x32\\x263A").

In addition to permitting one or more specific characters to be read at the different string model positions, you can permit a group of characters to be read. For example, you can permit all digits ([`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md)), letters ([`M_LETTERS`](../../Reference/dmr/MdmrControlStringModel.md)), lowercase letters ([`M_LETTERS_LOWERCASE`](../../Reference/dmr/MdmrControlStringModel.md)), and uppercase letters ([`M_LETTERS_UPPERCASE`](../../Reference/dmr/MdmrControlStringModel.md)) that exist in the specified font to be read. You can also permit all characters present in the font to be read ([`M_ANY`](../../Reference/dmr/MdmrControlStringModel.md)). The specified font must contain at least one instance of the characters you are permitting to be read. For example, if you use [`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md), the specified font must have at least one character between '0' and '9'.

All permitted characters in the target, whether you identify them explicitly or as a group, must adhere to their corresponding character definition in either a specific font or in any font in the context. To specify one or any font, use [`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_FONT_INDEX()`](../../Reference/dmr/MdmrControlStringModel.md) or [`M_FONT_LABEL()`](../../Reference/dmr/MdmrControlStringModel.md). For example, you can permit SureDotOCR to read all digits ([`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md)) represented by a specific font ([`M_FONT_INDEX(n)`](../../Reference/dmr/MdmrControlStringModel.md)), or all digits ([`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md)) represented by any font ([`M_FONT_INDEX()`](../../Reference/dmr/MdmrControlStringModel.md) set to[`M_FONT_INDEX()`](../../Reference/dmr/MdmrControlStringModel.md)).

[`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md) is cumulative. Each time you specify permitted characters, they are added to the previously specified permitted characters, if they exist. For example, in one call to [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md), you can specify the number '1' (with [`M_CHAR_LIST`](../../Reference/dmr/MdmrControlStringModel.md)) as a permitted character constraint at position 0, and in another call to [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md), you can specify [`M_LETTERS`](../../Reference/dmr/MdmrControlStringModel.md) as a permitted character constraint at position 0. These two entries allow the number '1' and any letter in the font to be read at position 0. To inquire the number of permitted character entries, call [`MdmrInquireStringModel`](../../Reference/dmr/MdmrInquireStringModel.md) with [`M_NUMBER_OF_PERMITTED_CHARS_ENTRIES`](../../Reference/dmr/MdmrInquireStringModel.md). If this inquire returns 0, it indicates that you have not added any permitted character entries, which means SureDotOCR reads any character from any font (initial default behavior). You can also delete a character constraint at a specific index by calling [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_DELETE_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md). You can select to delete a restriction at a specific index, or delete all restrictions.

### Optional positions

You can choose whether a position in a string model is optional, using [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_IS_OPTIONAL`](../../Reference/dmr/MdmrControlStringModel.md). You should mark a model position as optional if you do not require a character to be returned for that position in the model. If the string model has a minimum string size ([`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md)) less than its maximum size ([`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md)), optional positions can be skipped if their constraints cannot be met (up to a maximum of [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md)-[`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) skips).

For example, if you want to read 10-digit telephone numbers in the formats '###-###-####' and '##########', you can set the minimum string size to 10, set the maximum string size to 12, and mark positions 3 and 7 as optional. Then, set the permitted character constraints to [`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md) for positions 0-2, 4-6, and 8-11 and to hyphens for positions 3 and 7. If the target string is "123-456-7890", all the constraints are met, and SureDotOCR will return "123-456-7890". If the target string is "1234567890", positions 3 and 7 are skipped because they are optional and their constraints cannot be met, and SureDotOCR will return "1234567890".

Note that unless you set a model position as optional, its character constraints must be met to return the string.

Regardless of whether a model position is optional, if a character in the target image does not meet the constraints and [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md) is set wide enough, SureDotOCR will ignore the character and check if the constraints are met by the next character. If the distance between the previous character and the next character is less than the minimum space width, the current character is ignored and the next character can be used to meet the constraints. In the example above, if you had set the model to '##########', the target string "123-456-7890" might still have been read (as "1234567890") if the minimum space width was wide enough. It is always safer to include optional characters in the model and mark their positions as optional.

You can use [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dmr/MdmrGetResult.md) to see which positions in the model were skipped. Note that a string read with a lower number of skips will be returned as a higher priority result than a string read with a higher number of skips.

Note that [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md) will not accept optional constraints if [`M_STRING_PARTIAL_MODE`](../../Reference/dmr/MdmrControl.md) is enabled.

### Reading space

SureDotOCR also allows you to read a space character as a permitted character constraint, using [`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md). Unlike other permitted characters, [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) has the following restrictions (not following them can cause an error).

- You cannot represent a space character in a font. As discussed, you establish a space character with the [`M_SPACE_SIZE_...`](../../Reference/dmr/MdmrControl.md) context controls.
- A space constraint is exclusive at a given string model position. For example, if you specify [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) for a position, you cannot add another character constraint, such as [`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md), at that same position.
- You cannot have a space constraint at consecutive positions in a string model.
- The first or last position of a string model cannot have a space constraint.

Reading an [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) character does not necessarily mean that the corresponding space position in the target string is blank. SureDotOCR attempts to read the best string possible, and can use the space constraint as an area to ignore, even if that area is not empty. For example, if you have an icon at a specific position within a target string, you can specify a space character at that position.

Below is a list of examples illustrating how SureDotOCR manages and reads permitted space characters. For simplicity, 'S' refers to space and 'L' refers to letter. Any permitted character other than space could have been used instead of a letter.

| Target string | Permitted character constraints | Will it read? |
| --- | --- | --- |
| AB CD | LLSLL | Yes |
| AB CD EF | LLSLLSLL | Yes |
| ABCD | LLSLL | No |
| ABCDEF | LLSLLSLL | No |
| AB CD | LLLL | Yes |
| AB CD EF | LLLLLL | Yes |
| AB CD EF | LLSLLLL | Yes |
| M*[Image: MdmrIconIgnoreWithSpace.png]*<br/><br/>M | LSL | Yes |
| MWM | LSL | Yes |
| In the target string MWM, the letter 'W' can represent numerous types of data, such as another character, an icon, or noise. |

If an [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) character was read, its resulting character score will be 100% and it will not influence the score of the string. For more information, see [Results and annotations](S07_Retrieving_results_and_annotation.md).

### Default permitted characters, and overriding them

If necessary, you can specify permitted characters to read for every string model position. This can prove tedious, as in cases where you want to permit the same characters for the majority of positions in the string model. To handle such cases, and others like it, you can set default constraints for all the positions in the string model by passing [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md) to the [`Position`](../../Reference/dmr/MdmrControlStringModel.md) parameter, and override the constraints for specific positions by passing [`M_POSITION_IN_STRING(n)`](../../Reference/dmr/MdmrControlStringModel.md) instead, where _n_ is the position for which to specify an explicit (overriding) constraint.

As an example, consider reading the following product lot number:

*[Image: MdmrDefaultPermittedCharsForAllPositions.png]*

Here, the dot-matrix string contains 11 characters. With the exception of one character, which is a hyphen, each character can be a digit between 0 and 9. To handle this, you can specify [`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md) as the default constraint for all positions in the string model. You can then explicitly specify that at position 5, you want to read a hyphen. This requires calling [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) twice, which is a lot more convenient than calling it eleven times.

If you override the default constraints for a specific position, the position is said to be explicitly constrained; otherwise, it is said to be implicitly constrained. In the lot number example above, there is one explicitly constrained position (at position 5).

### General controls for the different positions in the string model

SureDotOCR provides general controls for the different positions in the string model. Specifically, you can:

- Clone the explicit constraints from one position to the next, using [`M_CLONE_CONSTRAINTS_FROM`](../../Reference/dmr/MdmrControlStringModel.md).
- Reset the default constraints for the positions in the string model back to their initial value, using [`M_RESET_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md).
- Reset the explicitly constrained position to be implicitly constrained, using [`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md).
  Once you have explicitly constrained a position, it is considered to be explicitly constrained until you use this operational control, even if you manually set the position's constraints back to the defaults of the string model.

### Managing the different string model positions (implicit or explicitly constrained)

The way in which to manage the different string model positions depends on the [`Position`](../../Reference/dmr/MdmrControlStringModel.md) parameter. Specifying [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md) indicates that you are working with the default constraints for all the positions in the string model (positions that are implicitly constrained). Specifying [`M_POSITION_IN_STRING(n)`](../../Reference/dmr/MdmrControlStringModel.md) indicates that you are overriding the default constraints for the _n_ string model position (that position will then be considered explicitly constrained).

You can also specify [`M_POSITION_IN_STRING(n)`](../../Reference/dmr/MdmrControlStringModel.md) to modify a position (_n_) that has already been explicitly constrained. Alternatively, you can modify such a position with[`M_POSITION_CONSTRAINED_ORDER(n)`](../../Reference/dmr/MdmrControlStringModel.md), where _n_ indicates the order in which the position was explicitly constrained. For example, if you explicitly constrained positions 19, 5, and 24, their respective constrained order values are 0, 1, and 2.

To control all positions that have been explicitly constrained, use [`M_ALL_CONSTRAINED_POSITIONS`](../../Reference/dmr/MdmrControlStringModel.md). This can be useful to, for example, add a permitted character constraint to all existing explicitly constrained positions or to reset all explicitly constrained positions to be implicitly constrained ([`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md)).

Except for [`M_ALL_CONSTRAINED_POSITIONS`](../../Reference/dmr/MdmrControlStringModel.md), all [`Position`](../../Reference/dmr/MdmrControlStringModel.md) parameter settings are available for both controlling and inquiring about string models.

If you want to loop through all explicitly constrained positions, call [`MdmrInquireStringModel`](../../Reference/dmr/MdmrInquireStringModel.md) with the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md) parameter set to [`M_POSITION_CONSTRAINED_ORDER(n)`](../../Reference/dmr/MdmrInquireStringModel.md), where _n_ is the order in which the position was explicitly constrained. Note that the constrained order of a position changes if you reset previously constrained positions to be implicitly constrained ([`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md)).

To inquire the number of explicitly constrained positions, use[`M_NUMBER_OF_CONSTRAINED_POSITIONS`](../../Reference/dmr/MdmrInquireStringModel.md).

## Recommended conditions for optimal string reading

Sometimes strings you want to read are less than optimal: they could be only part of a string, close to other strings, or dissimilar in dot spacing. To avoid false readings, there are some methods you can use to ensure you are reading the intended strings.

### Reading a partial string

If you are only interested in part of a string, the defined text block should be large enough to read all dot-matrix text in the image (or associated region of interest), including the parts you don't want. After reading the entire string, use string related functions to separate the parts of the string you want to read (for example, using a strncpy or std::substr function in C). This avoids problematic results when the module centers the text block in an unexpected place in your target image.

*[Image: SureDotOCRStringsToReadSubstring.png]*

### Reading only one line of a string with multiple lines

If you are only interested in one line of a string that has multiple lines, you should still read all of the lines in one search region, even the ones you don't want, and then extract the required string line. This helps avoid problematic results.

### Reading strings with dissimilar qualities

Multiple strings in an image can sometimes have characters with dissimilar qualities, such as variations in skew angle or spacing between dots. This can occur when, for example, the strings were printed with different ink jets. To prevent inaccurate reading of such strings, use one SureDotOCR context per string model you are looking for, each called to read a different child buffer. If the string can move locations, you can create two child buffers (or more) the size of the entire image, and use a different ROI with each. The following example, illustrates the latter. Two child buffers are set to the size of the whole image; an ROI is set for each such that each encompasses a different part of the string. A different context (with different string constraint) is then used to read each set of strings separately.

*[Image: SureDotOCRStringsToReadWithSkew.png]*
