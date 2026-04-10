---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Combination_values
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Combination values"
---

# Combination values and macros

Some Aurora Imaging Library functions make use of combination values and macros. Combination values and macros are mechanisms by which you can specify multiple pieces of information to one parameter.

## Combination values

Some parameters in Aurora Imaging Library functions can take a combination of constants added together; typically these parameters take a primary constant added with what is referred to as a combination value. Combination value are used to indicate an additional setting without requiring an additional parameter or increasing the number of possible constants to cover all permutations. An example of a parameter that can take a combination of constants is the [`Attribute`](../../Reference/buf/MbufAlloc2d.md) parameter of [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md); you can set this parameter to:

[`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md) + [`M_DISP`](../../Reference/buf/MbufAlloc2d.md).

This specifies that the function should allocate an image type buffer, and that it should be displayable. This mechanism works because in these cases, both the primary constant and the combination value are bit-encoded; that said, it is recommended that you combine them using an addition (+) operation and not a bitwise OR operation. This makes your code more legible because the combination values typically indicate an additional setting instead of an alternative setting.

If using the addition operation, be careful not to mistakenly add the same combination value to the primary constant more than once. This has a cumulative effect on the respective bits, leading to what could be a technically valid, yet incorrect constant. For parameters that take combination values, you should also be careful to pass the right constants to the parameter. This is because adding the wrong constants could also lead to a technically valid, yet incorrect constant.

Constants that can be combined with combination values listed in a separate table denote this fact with a plus (+) sign. If you hover over the plus (+) sign, a hover box will appear indicating the table names containing the possible combination values. If the combination value's table is only supported for a subset of the systems for which the primary constant is supported, these will be listed in the combination table and in the hover box.

*[Image: CombinationTableHoverBox.png]*

A combination values table is placed after the table with the last primary constant that can be combined with the values in the combination values table. A combination values table will, at the top of the table, list the primary constants that can (or must) be combined with the combination values in the table.

*[Image: CombinationTableComboValues.png]*

Combination values are typically optional, but at times are required. When a combination value is required, it is indicated in the description of the primary constant and in the preamble of the combination table. In some rare cases, constants can be combined with constants in the same table; in these cases, the fact that they can be combined is indicated in text above the table (for example, the constants for the [`Operation`](../../Reference/blob/MblobDraw.md) parameter in [`MblobDraw`](../../Reference/blob/MblobDraw.md)).

Some combination values can be further combined with other combination values. An example of this is when allocating an image buffer for compressed data using [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md); you could pass its [`Attribute`](../../Reference/buf/MbufAlloc2d.md) parameter [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md) + [`M_COMPRESS`](../../Reference/buf/MbufAlloc2d.md) and further combine these with an [`M_JPEG...`](../../Reference/buf/MbufAlloc2d.md) constant to indicate the compression algorithm. For example, if you want to allocate an image buffer to hold JPEG2000 lossless compressed data, you would specify:

[`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md)+[`M_COMPRESS`](../../Reference/buf/MbufAlloc2d.md) + [`M_JPEG2000_LOSSLESS`](../../Reference/buf/MbufAlloc2d.md).

## Function-like macros

Some parameters in Aurora Imaging Library functions have their values passed through a macro (for example, [`M_RGB888()`](../../Reference/disp/MdispControl.md)). These macros can take one or more parameters, and will perform some internal operation, the result of which will be returned to the parameter calling the macro. Essentially, macros are like calling a function. For example, the [`M_RGB888()`](../../Reference/gra/MgraControl.md) macro takes three 8-bit values, representing each color element (red, green, and blue), and converts them to an encoded RGB value. The following is an example of setting an Aurora Imaging Library function's parameter using a macro:

> **Code example:** [userguide.displaying_an_image.annotating_the_displayed_image_non_destructively04](userguide.displaying_an_image.annotating_the_displayed_image_non_destructively04)

Note that when you set a control type to a value using a macro (for example, [`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispControl.md) with [`M_RGB888()`](../../Reference/gra/MgraControl.md)), and then you inquire this value, you need to use a different macro to decode the returned value. For example, to retrieve the red, green, and blue bands of the encoded RGB value, you must use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros respectively. The following snippet shows how to use the RGB unpacking macros:

> **Code example:** [userguide.building_an_application.combination_values01](userguide.building_an_application.combination_values01)

Some parameters can take one of several macros. For example, the [`LabelOrIndex`](../../Reference/bead/MbeadControl.md) parameter of [`MbeadControl`](../../Reference/bead/MbeadControl.md) can take the label and index macros ([`M_TEMPLATE_LABEL()`](../../Reference/bead/MbeadControl.md) and [`M_TEMPLATE_INDEX()`](../../Reference/bead/MbeadControl.md) respectively); these macros bit-shift values so Aurora Imaging Library understands whether a label or an index is being specified as the value for a function's parameter. Some parameters can take a combination of macros; in such cases, the macros are bit-encoded (for example, [`M_SEQ_OUTPUT()`](../../Reference/seq/MseqControl.md) + [`M_SEQ_DEST()`](../../Reference/seq/MseqControl.md)). This means that they must respect the same rules that were outlined above for when combining with combination values.

Strings passed directly to an Aurora Imaging Library function are passed through the **AIL_TEXT** macro. This macro detects and declares the string in the proper character encoding scheme required for the application at hand. If you intend to pass a string in a variable, you must pass the variable to the parameter without enclosing the variable in the **AIL_TEXT** macro. The **AIL_TEXT** macro is different from other macros; this distinction is due to the fact that, because it is completely resolved at compile-time, it cannot take a variable as a parameter. The following snippet shows how to properly use the **AIL_TEXT** macro:

> **Code example:** [userguide.building_an_application.combination_values02](userguide.building_an_application.combination_values02)

> **Note:** Note that the **AIL_TEXT** macro is only required in C and C++. For more information on dealing with strings in other languages, see [](../C60_Using_AIL_with_NET/S05_Using_NET_strings_with_AIL.md) and [](../C59_Using_AIL_with_Python/S03_AIL_functions_and_constants_in_Python.md).
