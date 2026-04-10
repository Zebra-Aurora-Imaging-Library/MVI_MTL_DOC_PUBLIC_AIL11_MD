---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Using_a_fontless_context
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Using a fontless context"
---

# Using a fontless context

Characters have distinct features which distinguish them from one another. The String Reader module can make use of these characteristics to read strings in a target image without making use of a font. To do so, it requires that you use a predefined fontless context. You might choose to use a fontless context over a font-based context in cases where you expect to read strings in images with variable fonts. A fontless context is faster to set up than a font-based one.

To use a fontless context, you need to restore, using [`MstrRestore`](../../Reference/str/MstrRestore.md), one of the predefined context files located in the installed Contexts folder (for example, _C:\Program Files\Aurora Imaging Library\11\Contexts\_). The String Reader module comes with three predefined fontless contexts:

- _"FONTLESS_ANPR.msr"_. A generic context useful to read a wide variety of license plate types written in Latin-based alphabets and Arabic numerals.
- _"FONTLESS_EUROPEAN_ANPR.msr"_. A context useful to read European license plates.
- _"FONTLESS_MACHINE_PRINT.msr"_. A special context that reads machine printed characters in Arial, Ocr-B, or other sans-serif fonts.

The String Reader module can only use a fontless context to read uppercase characters and numbers. When using a fontless context, [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_CONTEXT_TYPE`](../../Reference/str/MstrInquire.md) returns [`M_FONTLESS`](../../Reference/str/MstrInquire.md).

## Enabling and disabling characters

When you restore a fontless context, it contains information about all uppercase characters and numbers. By default, the read operation searches the image for this entire range. However, for some applications, it is preferable to search for a subset of these characters, especially when you know one of two similar looking characters will not appear. In a fontless context, characters are not added or deleted; instead, they are enabled or disabled. You can disable characters using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_DISABLE_CHAR`](../../Reference/str/MstrControl.md) or re-enable them with [`M_ENABLE_CHAR`](../../Reference/str/MstrControl.md).

When only a few characters are needed, you can disable all characters ([`M_DISABLE_CHAR`](../../Reference/str/MstrControl.md) set to [`M_ALL`](../../Reference/str/MstrControl.md)) and then re-enable the required characters, rather than disabling characters one at a time.

## Customizing a fontless context

After you have restored a fontless context, you need to specify the size of the characters to read. Use [`MstrControl`](../../Reference/str/MstrControl.md) to specify the following values in pixels:

- A reference height ([`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md)).
- The width of the narrowest character ([`M_MIN_CHAR_SIZE_X`](../../Reference/str/MstrControl.md)) at the reference height.
- The width of the widest character ([`M_MAX_CHAR_SIZE_X`](../../Reference/str/MstrControl.md)) at the reference height.
- The thickness (stroke width) of a typical character ([`M_REF_CHAR_THICKNESS`](../../Reference/str/MstrControl.md)) at the reference height.

If the characters vary in size, set the reference height ([`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md)) to the average value of the character set's heights. Then, specify minimum and maximum allowable scale factors based on the reference height with [`M_STRING_SCALE_MIN_FACTOR`](../../Reference/str/MstrControl.md) and [`M_STRING_SCALE_MAX_FACTOR`](../../Reference/str/MstrControl.md).

When setting [`M_MIN_CHAR_SIZE_X`](../../Reference/str/MstrControl.md), [`M_MAX_CHAR_SIZE_X`](../../Reference/str/MstrControl.md) and [`M_REF_CHAR_THICKNESS`](../../Reference/str/MstrControl.md), specify the width and thickness that the characters have when they have a height of [`M_REF_CHAR_SIZE_Y`](../../Reference/str/MstrControl.md). When the read operation finds characters of a different height, size values are scaled by the ratio between the height found and the reference height.

In the following example, the characters have a reference height of 41 pixels. The widest character, Q, has a width of 20 pixels, the narrowest character, 1, has a width of 11 pixels, and the thickness of the characters is 6 pixels.

*[Image: StringReader_ref_size.png]*
