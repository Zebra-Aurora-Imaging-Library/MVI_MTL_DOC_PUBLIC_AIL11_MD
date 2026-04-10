---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Global_context_settings
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Global context settings"
---

# Global context settings

The global settings of a String Reader context can determine the strategy used to read all of the context's string models, which in turn, could affect the speed and robustness of the read operation ([`MstrRead`](../../Reference/str/MstrRead.md)). To adjust the global context settings to fit your individual application's needs, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_CONTEXT`](../../Reference/str/MstrControl.md).

The default value for each global context setting is typically good. However, if you want to find the optimum balance between speed and robustness for a particular application, it is recommended that you experiment with different global context setting values.

The global context settings include:

- Minimum contrast.
- Speed.
- Timeout.
- Maximum number of strings to read.
- Encoding.
- Space character.
- String separator.
- Scale.
- Skew.

Unlike other global context settings of String Reader, you can set the maximum number of strings to read for both the String Reader context and each string model within the context. For more information, see [Number of strings to read](S08_Number_of_strings_to_read.md).

Scale and skew have been previously discussed. For more information, see [String scale and character scale](S09_Degrees_of_freedom.md) and [Skew angle](S09_Degrees_of_freedom.md). Space characters and string separators are discussed in [Formatted and non-formatted results](S13_Retrieving_results_and_annotation.md).

If the results of your operation are poor due to defects in the image (causing characters not to be distinguishable from some or all of the background) the image might need further processing, such as using [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_TOP_HAT`](../../Reference/im/MimMorphic.md) or [`M_BOTTOM_HAT`](../../Reference/im/MimMorphic.md). For more information, refer to[Top and bottom hat](../C04_Advanced_image_processing/S03_Custom_morphological_operations.md).

## Minimum contrast

String Reader is contrast invariant. Therefore, if a valid character exists in the target image, it will be read, regardless of whether the contrast between that character and its background is high or low. However, you might be able to speed up the read time by specifying the minimum contrast value, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_MINIMUM_CONTRAST`](../../Reference/str/MstrControl.md). Valid values are between 1 and 255. The default value is 15. Any character in the target image that does not have a contrast greater than the specified minimum value will be ignored, thereby possibly increasing the speed of the read operation.

Increasing the minimum contrast might make a readable character unreadable. For example, if the contrast between a valid character and its background is 25, and you set the minimum contrast to 50, that character will not be read. It is therefore important to make sure that the minimum contrast value is not causing the read operation to ignore acceptable characters.

## Speed

You can specify the algorithm's read speed, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPEED`](../../Reference/str/MstrControl.md). The speed can be set to [`M_MEDIUM`](../../Reference/str/MstrControl.md) or [`M_HIGH`](../../Reference/str/MstrControl.md). The default is [`M_MEDIUM`](../../Reference/str/MstrControl.md).

Higher read speeds cause the read operation to take a greater number of shortcuts, which typically results in shorter read times, though it can also skip important information.

## Timeout

In time critical applications, you can set a time limit, in msec, for String Reader to read using the specified string models. To do so, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_TIMEOUT`](../../Reference/str/MstrControl.md). The default value is 2000 msecs. Due to certain application-dependent calculations, the actual maximum read time might vary slightly from the timeout specified. You can disable the timeout setting using [`M_DISABLE`](../../Reference/str/MstrControl.md).

After the time limit has been reached, the read operation will terminate, even if the required number of strings has not been read. Results are returned for all strings read up to the timeout. It is not possible to predict which strings will be read beforehand.

To check whether the timeout limit has been reached, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_TIMEOUT_END`](../../Reference/str/MstrGetResult.md).

## Encoding

You can set the type of character encoding used by the String Reader context. To do so, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md).

The selected encoding scheme will affect the expected data type of the information that you pass to or retrieve from String Reader. For example, when adding characters to the font, using [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_CHAR_ADD`](../../Reference/str/MstrEditFont.md), the expected data type is based on the [`M_ENCODING`](../../Reference/str/MstrControl.md) setting.

The encoding schemes supported are [`M_ASCII`](../../Reference/str/MstrControl.md) and [`M_UNICODE`](../../Reference/str/MstrControl.md). [`M_ASCII`](../../Reference/str/MstrControl.md) specifies an 8-bit ASCII standard character type, and corresponds to the _char_ data type. This is the default value. [`M_UNICODE`](../../Reference/str/MstrControl.md) specifies a 16-bit Unicode standard character type, and corresponds to the _short_ data type.
