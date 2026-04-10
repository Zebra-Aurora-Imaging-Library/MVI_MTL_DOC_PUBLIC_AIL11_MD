---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Deep_learning_OCR
section: Settings_and_string_models
module_tag: dlocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Deep_learning_OCR / Settings and string models"
---

# Settings, string models, and positional constraints

The Deep Learning OCR module works as an out of box solution for reading text in an image. The Deep Learning OCR module can read all text in a target image within the default character height bounds. You can limit the strings to be read. Deep Learning OCR contexts store settings, string models, and positional constraints.

## Context settings

Many settings can be controlled at the context level to limit the strings being read. These settings apply to all read operations performed using the context. When using a string model, string occurrences must meet the requirements of the context, the string model settings, and positional constraints therein.

For setting modifications to take effect, you must preprocess the context by calling [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md) before performing a read operation, using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).

### Accepted characters

The accepted characters in a Deep Learning OCR context determine the list of characters that can be read. The accepted characters apply to all string models in the context. By default, the list of accepted characters is all upper and lowercase letters and digits. The accepted characters can be limited to a particular type of character, such as upper-case letters using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md) set to [`M_LETTERS_UPPERCASE_CHARS`](../../Reference/dlocr/MdlocrControl.md) or numbers with [`M_DIGITS_CHARS`](../../Reference/dlocr/MdlocrControl.md). You can also specify a custom list of accepted characters by passing a string of characters as the value for [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md). If you want to read punctuation characters, you must specify them in this list.

### Character size and shape

By default, the Deep Learning OCR module will read characters between 20 and 150 pixels tall. If you need to read characters outside of these bounds, you can adjust the bounds using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MIN`](../../Reference/dlocr/MdlocrControl.md) and [`M_DETECTION_CHAR_HEIGHT_MAX`](../../Reference/dlocr/MdlocrControl.md). You might need to adjust the aspect ratio of characters ([`M_CHAR_MAX_ASPECT_RATIO`](../../Reference/dlocr/MdlocrControl.md)); specify the aspect ratio as the width/height of the individual characters being read. The image is internally rescaled to improve readability. Characters far outside the expected aspect ratio might be misread.

*[Image: MdlocrCharAspectRatio.png]*

### Separation of characters and reading multiple strings

The Deep Learning OCR module will read characters on the same line as the same string. If the space between characters is great enough, they will be read as separate strings. You can adjust the maximum intercharacter width using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_INTERCHAR_MAX_WIDTH`](../../Reference/dlocr/MdlocrControl.md) to break or merge strings on the same line.

*[Image: Mdlocrgapsize.png]*

You can specify the number of strings that you want to read in an image using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_STRING_NUMBER`](../../Reference/dlocr/MdlocrControl.md). The Deep Learning OCR module will attempt to read exactly this number of strings in the target image while respecting the minimum and maximum number of occurrences of each string model contained in the context. If fewer than this number of strings are read from the target image, then nothing will be read. When [`M_STRING_NUMBER`](../../Reference/dlocr/MdlocrControl.md) is set to [`M_INFINITE`](../../Reference/dlocr/MdlocrControl.md), any number of strings can be read as long as the number of string occurrences for each string model are respected. If more than this number of strings are present in the image, the Deep Learning OCR module will stop reading once this number is reached. This only applies when performing a model-based read operation.

### Image size and rotation

You must set the maximum size of the target image buffer used for Deep Learning OCR functions; an error will be generated if this is not set. Generally, this should be set to the size of the target image. If a region of interest is used, the maximum target size refers to the size of that region. Setting the maximum target image buffer size much larger than the image or ROI can negatively impact performance. To modify this, use [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_TARGET_MAX_SIZE_X`](../../Reference/dlocr/MdlocrControl.md) and [`M_TARGET_MAX_SIZE_Y`](../../Reference/dlocr/MdlocrControl.md). When working with images or ROIs of different sizes, it is recommended to set these values to the dimensions of the largest image or ROI that will be encountered.

By default, the Deep Learning OCR module is designed to read horizontal strings. If the text appears at an angle in the target image, you should set the detection angle to the angle that text appears in the target image, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_ANGLE`](../../Reference/dlocr/MdlocrControl.md). Valid values for the angle are specified in degrees, and can range from 0 to 360. The string angle is set relative to the X-axis of the pixel coordinate system. Alternatively, you can set the detection angle to the angle of an ROI using [`M_ACCORDING_TO_REGION`](../../Reference/dlocr/MdlocrControl.md) if one is used.

### AI presearch

The AI presearch is designed to accelerate calls to [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) in large images containing few characters. By default, the AI presearch is disabled. To enable the AI presearch, set [`M_PRESEARCH_STRATEGY`](../../Reference/dlocr/MdlocrControl.md) to [`M_PRESEARCH_NET1_V1`](../../Reference/dlocr/MdlocrControl.md).

You can adjust the range of angles at which the presearch can find strings, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_PRESEARCH_ANGLE_RANGE`](../../Reference/dlocr/MdlocrControl.md). By default, this is set to [`M_NARROW_RANGE`](../../Reference/dlocr/MdlocrControl.md), which allows strings to be found at a range of [-90,90] degrees around the detection angle ([`M_DETECTION_ANGLE`](../../Reference/dlocr/MdlocrControl.md)). Setting [`M_PRESEARCH_ANGLE_RANGE`](../../Reference/dlocr/MdlocrControl.md) to [`M_WIDE_RANGE`](../../Reference/dlocr/MdlocrControl.md) allows strings to be found at a range of [-180,180] degrees around the detection angle.

## String model settings

A string model is a data structure, within a Deep Learning OCR context, that stores the requirements that a string in a target image must meet for the string to be read. These requirements can range from general to specific; they can apply to the entire string, or the different string model positions, know as positional constraints. String models are not required to read text in an image.

One or more string models can be added to a Deep Learning OCR context to limit what is read. String models can be defined from a string occurrence after an initial read operation using [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md). To manually add a string model to a context, use [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md) with [`M_ADD`](../../Reference/dlocr/MdlocrDefineModel.md). To delete a string model to a context, use [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md) with [`M_DELETE`](../../Reference/dlocr/MdlocrDefineModel.md). You can use [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) to control settings specific to the string model and inquire these settings using [`MdlocrInquireStringModel`](../../Reference/dlocr/MdlocrInquireStringModel.md).

For string model modifications to take effect, you must preprocess the context by calling [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).

Some string model settings mirror those set in the context. By default, the string model character height bounds are set to the same values as the context. These bounds can be set to different values for the models than the context. The character height bounds must comply with the bounds set in the context. The string model height bounds can be set using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_CHAR_HEIGHT_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md) and [`M_CHAR_HEIGHT_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md).

You can set a minimum and maximum number of strings to read for each string model in a context. You can set these using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_MIN_NUMBER_OF_OCCURRENCES`](../../Reference/dlocr/MdlocrControlStringModel.md) and [`M_MAX_NUMBER_OF_OCCURRENCES`](../../Reference/dlocr/MdlocrControlStringModel.md). If the minimum number of strings is not met, then no occurrences of the string model will be read. When the maximum number of strings that respect the model are read, the Deep Learning OCR module will stop reading according to specified model. The Deep Learning OCR module will continue to read strings that respect any other model in the context that has not met the maximum number of occurrences.

You can set the minimum and maximum number of characters in a string model using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md) and [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md).

### Text anchors

String models can be used to set textual anchors. This is useful when the string that you want to read starts or ends with a known sub-string.

You can use [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_TEXT_ANCHOR_MODE`](../../Reference/dlocr/MdlocrControlStringModel.md) to enable only reading of strings that have an expected prefix or suffix. You can set the value of the affix using [`M_TEXT_ANCHOR_VALUE`](../../Reference/dlocr/MdlocrControlStringModel.md).

For example, when reading serial numbers, every serial number might start with the string "S/N:". In this case, you can use [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_TEXT_ANCHOR_MODE`](../../Reference/dlocr/MdlocrControlStringModel.md) set to [`M_TEXT_PREFIX`](../../Reference/dlocr/MdlocrControlStringModel.md) and then use[`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_TEXT_ANCHOR_VALUE`](../../Reference/dlocr/MdlocrControlStringModel.md) to the string "S/N:". Textual anchors are not included in the string occurrence.

*[Image: MdlocrTextAnchors.png]*

## Constraining string model positions with permitted characters

As previously discussed, any character can, by default, be read at any string model position. For example, if you have two string models (_StringModel1_ and _StringModel2_), each with a maximum and minimum number of characters of two, and you have set your accepted characters to "42", the strings '24', '42', '22', or '44' can be read from the target image.

*[Image: MdlocrStringModelEx1.png]*

You can specify that position 0 of _StringModel1_ must be the digit "2" and position 1 of _StringModel2_ must be the digit "4". This will allow _StringModel1_ to read the strings, "22" and "24", and allow _StringModel2_ to read the strings "24" and "44".

*[Image: MdlocrStringModelEx2.png]*

Constraining string model positions is especially useful to avoid commonly mismatched characters. For example, the digit "0" and the uppercase letter "O" are commonly mismatched. If you want to read the string "01234" in an image, you might retrieve the result "O1234".

When permitting explicit characters to be read, use [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) with [`M_CONSTRAINT_TYPE`](../../Reference/dlocr/MdlocrControlConstraint.md) set to [`M_CHAR_LIST`](../../Reference/dlocr/MdlocrControlConstraint.md) and specify the list of character names, such as "42". If you are using Windows API functions with the Unicode macro, and your source code is also Unicode, you can specify the characters directly in Unicode. Be aware that each character in the list must exist in the list of accepted characters in the context. You can also specify character names in hexadecimal format; to do so, prefix the character name with "\\x", such as "\\x34\\x32" instead of "42". Hexadecimal notation is necessary if your data is in an ASCII format and you want to have Unicode characters beyond the Basic Latin range. To list a string of character names with mixed notation, you should also use "\\x" (for example, "ADAMS\\x34\\x32\\x263A").

In addition to permitting one or more specific characters to be read at the different string model positions, you can permit a group of characters to be read. For example, you can permit all digits ([`M_DIGITS`](../../Reference/dlocr/MdlocrControlConstraint.md)), letters ([`M_LETTERS`](../../Reference/dlocr/MdlocrControlConstraint.md)), lowercase letters ([`M_LETTERS_LOWERCASE`](../../Reference/dlocr/MdlocrControlConstraint.md)), and uppercase letters ([`M_LETTERS_UPPERCASE`](../../Reference/dlocr/MdlocrControlConstraint.md)). You can also permit all characters present in the list of accepted characters to be read ([`M_ANY`](../../Reference/dlocr/MdlocrControlConstraint.md)). The list of accepted characters must contain at least one instance of the characters that you are permitting to be read. For example, if you use [`M_DIGITS`](../../Reference/dlocr/MdlocrControlConstraint.md), the list of accepted characters must contain at least one digit between '0' and '9'.

### Optional positions

You can choose whether a position in a string model is optional, using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) with [`M_IS_OPTIONAL`](../../Reference/dlocr/MdlocrControlConstraint.md). You should mark a model position as optional if you do not require a character to be returned for that position in the model. If the string model has a minimum string size ([`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md)) less than its maximum size ([`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md)), optional positions can be skipped if their constraints cannot be met (up to a maximum of [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md)- [`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md) skips).

For example, if you want to read 10-digit telephone numbers in the formats '###-###-####' and '##########', you can set the minimum string size to 10, set the maximum string size to 12, and mark positions 3 and 7 as optional. Then, set the permitted character constraints to [`M_DIGITS`](../../Reference/dlocr/MdlocrControlConstraint.md) for positions 0-2, 4-6, and 8-11 and to hyphens for positions 3 and 7. If the target string is "123-456-7890", all the constraints are met, and Deep Learning OCR will return "123-456-7890". If the target string is "1234567890", positions 3 and 7 are skipped because they are optional and their constraints cannot be met, and Deep Learning OCR will return "1234567890".

Note that unless you set a model position as optional, its character constraints must be met to return the string.

Regardless of whether a model position is optional, if a character in the target image does not meet the constraints, Deep Learning OCR will ignore the character and check if the constraints are met by the next character if [`M_INTERCHAR_MAX_WIDTH`](../../Reference/dlocr/MdlocrControl.md) is set wide enough. If the distance between the previous character and the next character is less than the maximum inter-character width, the current character is ignored and the next character can be used to meet the constraints. In the example above, if you had set the model to '##########', the target string "123-456-7890" might still have been read (as "1234567890") if the maximum inter-character width was wide enough. It is always safer to include optional characters in the model and mark their positions as optional.

You can use [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dlocr/MdlocrGetResult.md) to see which positions in the model were skipped. Note that a string read with a lower number of skips will be returned as a higher priority result than a string read with a higher number of skips.

### Default permitted characters, and overriding them

If necessary, you can specify permitted characters to read for every string model position. This can prove tedious, as in cases where you want to permit the same characters for the majority of positions in the string model. To handle such cases, and others like it, you can set default constraints for all the positions in the string model by passing [`M_DEFAULT`](../../Reference/dlocr/MdlocrControlConstraint.md) to the [`PositionInString`](../../Reference/dlocr/MdlocrControlConstraint.md) parameter, and override the constraints for specific positions by passing the character index instead.

As an example, consider reading the following product lot number:

*[Image: MdlocrDefaultPermittedCharsForAllPositions.png]*

Here, the string contains 11 characters. With the exception of one character, which is a hyphen, each character can be a digit between 0 and 9. To handle this, you can specify [`M_DIGITS`](../../Reference/dlocr/MdlocrControlConstraint.md) as the default constraint for all positions in the string model. You can then explicitly specify that at position 5, you want to read a hyphen. This requires calling [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) twice, which is a lot more convenient than calling it eleven times.

If you override the default constraints for a specific position, the position is said to be explicitly constrained; otherwise, it is said to be implicitly constrained. In the lot number example above, there is one explicitly constrained position (at position 5).
