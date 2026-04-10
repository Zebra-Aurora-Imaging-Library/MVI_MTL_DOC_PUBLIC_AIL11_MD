---
doctype: UserGuide
part: "2D related information"
chapter: Generating_graphics
section: Writing_text
module_tag: gra
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / graphics / Writing text"
---

# Writing text

To perform text annotations, use [`MgraText`](../../Reference/gra/MgraText.md), which writes a string at the specified position in a given buffer. The position is interpreted according to the specified units and associated coordinate system set using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md).

> **Note:** [`MgraText`](../../Reference/gra/MgraText.md) can also add text to a 2D graphics list. For more information, see [2D graphics list](S06_Graphics_list.md).

Text is drawn using the current foreground color of the 2D graphics context. The 2D graphics context also assigns the following settings to text:

- **Background mode.** This determines whether to fill the background of the text. The default is to fill the background. To change the default (for example, to make it transparent), use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_BACKGROUND_MODE`](../../Reference/gra/MgraControl.md).
- **Background color.** This determines the color used behind text. The default background color value is zero (typically corresponds to black). To change the default, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_BACKCOLOR`](../../Reference/gra/MgraControl.md).
- **Font.** This determines the text's font. You can choose between a bitmap font, or a TrueType font installed on your computer. To change the text font, use [`MgraFont`](../../Reference/gra/MgraFont.md).
  You can specify a TrueType font using [`MgraFont`](../../Reference/gra/MgraFont.md). You can use the default TrueType font of your operating system by passing [`M_FONT_DEFAULT_TTF`](../../Reference/gra/MgraFont.md), or you can specify a font and style by passing an **AIL_TEXT** string according to the following format: _Family_:_Weight_:_Slant_ (for example, "_Arial_:_Bold_:_Italic_"). You can also pass an **AIL_TEXT** string with the path to a TrueType font file (for example, "C:\myDirectory\myTrueTypeFont.ttf "). If a specified TrueType font does not support a character code that needs to be drawn, you can have Aurora Imaging Library search for a suitable font to draw the character using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_FONT_AUTO_SELECT`](../../Reference/gra/MgraControl.md).
  You can specify the size of TrueType fonts using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_FONT_SIZE`](../../Reference/gra/MgraControl.md).
  > **Note:** With bitmap fonts, it is not possible to specify an exact font size other than the three provided default sizes ([`M_FONT_DEFAULT_LARGE`](../../Reference/gra/MgraFont.md), [`M_FONT_DEFAULT_MEDIUM`](../../Reference/gra/MgraFont.md), and [`M_FONT_DEFAULT_SMALL`](../../Reference/gra/MgraFont.md)). However, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_FONT_X_SCALE`](../../Reference/gra/MgraControl.md) and [`M_FONT_Y_SCALE`](../../Reference/gra/MgraControl.md), or [`MgraFontScale`](../../Reference/gra/MgraFontScale.md), you can scale any of the three default bitmap sizes to a more suitable size.
- **Horizontal and vertical alignment.** This determines the alignment of the text in the horizontal and vertical direction. The defaults are left-aligned and top-aligned, with respect to the string's specified X- and Y-position. To change the defaults, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_TEXT_ALIGN_HORIZONTAL`](../../Reference/gra/MgraControl.md) and [`M_TEXT_ALIGN_VERTICAL`](../../Reference/gra/MgraControl.md).
  *[Image: TEXT_ALIGNMENT.png]*
- **Text border.**This allows you to draw a border around the block of text that you want to print. You can draw a border above, below, to the left, or to the right of the text, or any combination of the four directions. The default is to draw no borders. To change the defaults, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_TEXT_BORDER`](../../Reference/gra/MgraControl.md).
- **Text direction.**This allows you to specify the direction to draw the text. The default is to draw the text from left to right. To change the defaults, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_TEXT_DIRECTION`](../../Reference/gra/MgraControl.md).

For an example of how to perform multiple text annotations with differing fonts, see _mgratext.cpp_ in Aurora Imaging Example Launcher.
