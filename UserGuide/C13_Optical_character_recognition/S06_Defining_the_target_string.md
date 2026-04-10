---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Defining_the_target_string
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Defining the target string"
---

# Defining the target strings

Define the target strings in the following ways:

- Calibrating your font helps to match the characters in the target image to those within the Aurora Imaging Library OCR font.
- Setting the appropriate processing controls and string information helps better define the search criteria for best results.
- Constraints allow you to limit the search to specific characters within the Aurora Imaging Library OCR font.

## Calibrating your font

You should either manually or automatically calibrate your Aurora Imaging Library OCR font to better match the size of the characters and the inter-character spacing in the target image. The more Aurora Imaging Library OCR knows about your target image, the faster it can search for the required string(s) and the more robust the results.

> **Note:** Note that in some cases, it might be more efficient to change the size of the actual font than to calibrate.

### Automatic font calibration

Automatic font calibration is only available when you are using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context.

Use [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md) with a small, given range of sizes in which to search, and it will test all the possible sizes in that range, returning the best match of the width, the height, and the spacing of the characters in your sample target image. The sample target image should be the best possible image with an angle, string length, and number of strings that are representative of the target images.

Automatic font calibration is always performed using only the first string with the highest match score found in the sample target image. To calibrate multiple strings, create a child buffer around each. If the string cannot be located in the image, an error is generated. Skipping the string locator step (using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SKIP_STRING_LOCATION`](../../Reference/ocr/MocrControl.md)) might produce undesirable results. This is discussed later in this chapter.

When you are dealing with long strings, the spacing in your target image should be as accurate as possible.

> **Note:** Automatic font calibration resets values set during a manual font calibration.

### Manual font calibration

Manual font calibration can be done for either [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) or [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context types. With an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context type, manual font calibration does not require exact numbers.

Use [`MocrControl`](../../Reference/ocr/MocrControl.md) to manually set the width ([`M_TARGET_CHAR_SIZE_X`](../../Reference/ocr/MocrControl.md)), the height ([`M_TARGET_CHAR_SIZE_Y`](../../Reference/ocr/MocrControl.md)), and spacing ([`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md)) of the target image characters, in pixels. If the exact measurements are known, manually setting the size of the target characters is faster than an automatic font calibration.

When using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context with a fixed size font and reading a string that has the same spacing between characters, but the size of the spacing is unknown, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md) set to [`M_SAME`](../../Reference/ocr/MocrControl.md). If the spacing is unknown and/or not the same between characters, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md) set to [`M_ANY`](../../Reference/ocr/MocrControl.md).

Note that the scale factors between target character sizes and font character sizes must be between 0.25 and 4.0, inclusive. The following restrictions apply:

- [`TargetCharSizeXMin`](../../Reference/ocr/MocrCalibrateFont.md) `/(` [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md) `) >= 0.25`.
- [`TargetCharSizeYMin`](../../Reference/ocr/MocrCalibrateFont.md) `/(` [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md) `) >= 0.25`.
- [`TargetCharSizeXMax`](../../Reference/ocr/MocrCalibrateFont.md) `/(` [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md) `) &lt;= 4.0`.
- [`TargetCharSizeYMax`](../../Reference/ocr/MocrCalibrateFont.md) `/(` [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md) `) &lt;= 4.0`.

## Setting appropriate processing controls

Each of the following controls can improve the robustness of the search. Note, however, that these controls increase the complexity of the operation and reduce its overall speed.

### Blanks

By default, Aurora Imaging Library OCR ignores blanks in the target image.

When using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context type, you can use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md) to enable the ability to read blank characters. Blank characters should not be included in your Aurora Imaging Library OCR font. Once enabled, you must modify your string length to include the number of blanks that you expect in the target image. Blanks before the target string are not counted. A blank space will have the same size and inter-character spacing as all other characters in the Aurora Imaging Library OCR font, unless [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md) is set to [`M_ANY`](../../Reference/ocr/MocrControl.md).

> **Note:** Note that [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md) is available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. In addition, blank characters cannot be verified (using [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md)) and will cause an Aurora Imaging Library error.

### Broken characters

If the target image contains characters that contain scratches or breaks, the target image contains a broken character. To force Aurora Imaging Library OCR to identify these characters as a possible match of the characters within the Aurora Imaging Library OCR font, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_BROKEN_CHAR`](../../Reference/ocr/MocrControl.md) enabled. This will reduce the speed of subsequent read/verify operations but can increase robustness. Broken characters should not be included in an Aurora Imaging Library OCR font.

> **Note:** Note that, when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, [`M_BROKEN_CHAR`](../../Reference/ocr/MocrControl.md) must be enabled to read a broken character. In an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context, Aurora Imaging Library OCR will automatically try to identify broken characters.

### Morphological filtering

When the difference between the foreground and the background is so slight that it causes read or verification errors, increase or decrease the value for morphological filtering, using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_MORPHOLOGIC_FILTERING`](../../Reference/ocr/MocrControl.md), to improve Aurora Imaging Library OCR's chances of finding and identifying the characters within the target string. Experimentation is required to determine the exact number that should be passed to this control.

### Thickness and dots

You can thicken the target characters using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_THICKEN_CHAR`](../../Reference/ocr/MocrControl.md) when reading thin characters made up of dots to make the characters easier to find and identify.

### Touching characters

If characters in the target image touch each other, or if they are connected to blobs of a similar intensity, enable touching characters, using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TOUCHING_CHAR`](../../Reference/ocr/MocrControl.md). This will improve Aurora Imaging Library OCR's chances of finding and identifying these characters. This will reduce the speed of subsequent read/verify operations but can increase robustness.

> **Note:** Note that, when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, [`M_TOUCHING_CHAR`](../../Reference/ocr/MocrControl.md) must be enabled to read touching characters. When using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context, Aurora Imaging Library OCR will automatically try to identify touching characters.

## Specifying other string information

In some cases, additional information about the target string is required. For example, to search for multiple strings, or to search at an angle, certain control types should be set. In other cases, additional information will increase the robustness of the search.

### Number of strings

You can set the search to find multiple strings using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_NUMBER`](../../Reference/ocr/MocrControl.md). Multiple strings can only be found if each string resides on a different line within the target image and each line of text does not overlap the previous.

> **Note:** Note that, for best results, all strings in an image should be of similar length, have a consistent inter-line spacing, and should start at a similar location along the X-axis.

Identifying the number of strings to read/verify is important when dealing with a target image containing multiple lines. It does not have to be specified for single-string images since the default value is 1.

### String lengths

You should specify the length of the strings to be read, to receive reliable results using Aurora Imaging Library OCR. The maximum string length must be set at the time of OCR font context allocation. With an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context type, the maximum string length can be set to [`M_ANY`](../../Reference/ocr/MocrControl.md). Note, however, that this increases the processing time since the OCR module must then try to calculate the string length based on successful matches between the target image and the Aurora Imaging Library OCR font.

The default string length is set to the maximum string length of the OCR font context. When the OCR font context constraints are set (discussed later), specifying a string length can also improve the speed of a following read/verify operation.

> **Note:** Note that if [`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) is used, the string length must be 12. If [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md) is used, the string length must be 18.

If the entire text of the target image contains one or more strings that differ significantly in length, use an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context for fast results. For more robust results, allocate separate [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font contexts for each line of text to be read/verified. Experimentation with both OCR font context types is the only way to determine which provides the best solution for each case.

Often, when using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context, errors in string length result in unpredictable results.

### Angle

You can search for the strings within the target image at a specific angle, or through an angular range. The angle of the search is used to locate the string or strings before a read/verify operation. If individual lines in the target image are set at different angles, create a child buffer containing each string at a different angle and read them using a different OCR font context.

> **Note:** These child buffers should not overlap.

For each OCR font context, you can specify the nominal angle of the search using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md). By default, the search angle is 0.0°.

To search through an angular range, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_ANGLE_DELTA_POS`](../../Reference/ocr/MocrControl.md) to specify the positive range and/or [`M_STRING_ANGLE_DELTA_NEG`](../../Reference/ocr/MocrControl.md) to specify the negative range in which to search. A search for a string within the target image through an angular range always starts at the nominal angle of the string, specified with [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md). Note that the range in which to search, either in the positive or negative direction, should never be greater than 180°.

*[Image: OCRDeltaConvention.png]*

If you were searching for a string at approximately 25°, you might want to search between 40° and 10°. To do this, you would set [`M_STRING_ANGLE`](../../Reference/ocr/MocrControl.md) to 25°, and both [`M_STRING_ANGLE_DELTA_POS`](../../Reference/ocr/MocrControl.md) and [`M_STRING_ANGLE_DELTA_NEG`](../../Reference/ocr/MocrControl.md) to 15°. This results in a 30° arc that will be searched for your string.

Searching through a range of angles is enabled only if at least one of the [`M_STRING_ANGLE_DELTA...`](../../Reference/ocr/MocrControl.md) control types are set to a value greater than or equal to 1°. Once enabled, the font context's character size must be greater than 16x16 to avoid an Aurora Imaging Library error when calling [`MocrReadString`](../../Reference/ocr/MocrReadString.md), [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md) or [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md).

> **Note:** Note that, after setting [`M_STRING_ANGLE...`](../../Reference/ocr/MocrControl.md), you must call [`MocrPreprocess`](../../Reference/ocr/MocrPreprocess.md) before calling [`MocrReadString`](../../Reference/ocr/MocrReadString.md) or [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md).

### Positional variation

If the inter-character spacing is not even, you can increase the robustness of the search by specifying the maximum variation in position of the characters. The position variation increases the area being searched for characters within the target image (using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_POSITION_VARIATION_X`](../../Reference/ocr/MocrControl.md) and [`M_CHAR_POSITION_VARIATION_Y`](../../Reference/ocr/MocrControl.md)). These values are relative to the expected position of each character.

## Constraints

The read operation compares each character in the target image to each character in the font to find the best match. You might know beforehand that certain characters (or types of characters) should appear at specific positions in the string. If this is the case, you can speed up and increase the robustness of the read operation by restricting the comparison to only those characters in the font. The following types of constraints can be set for each character in the string, using [`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md):

- A digit ([`M_DIGIT`](../../Reference/ocr/MocrSetConstraint.md)): ASCII codes 48 to 57.
- A letter ([`M_LETTER`](../../Reference/ocr/MocrSetConstraint.md)): ASCII codes 65 to 90 and 97 to 122.
- An uppercase letter ([`M_LETTER`](../../Reference/ocr/MocrSetConstraint.md) + [`M_UPPERCASE`](../../Reference/ocr/MocrSetConstraint.md)): ASCII characters 65 to 90.
- A lowercase letter ([`M_LETTER`](../../Reference/ocr/MocrSetConstraint.md) + [`M_LOWERCASE`](../../Reference/ocr/MocrSetConstraint.md)): ASCII characters 97 to 122.
- A character from a specific list of characters, for example A, 1, b, 2. This includes special characters and punctuations, for example ampersand (&), hyphen (-), ellipsis (...).

> **Note:** Note that Aurora Imaging Library OCR will search for the number of characters specified by the target string length. For best results, the number of characters to be found in your target image should match your string length.

The constraints are stored with the OCR font context as part of its information set and can be inquired, using [`MocrInquire`](../../Reference/ocr/MocrInquire.md).

The following is an example of how character constraints are set. For example, the character in the first position should be the letter K and the character in the second position should be any upper or lowercase letter.

> **Code example:** [userguide.optical_character_recognition.setting_character_constraints](userguide.optical_character_recognition.setting_character_constraints)
