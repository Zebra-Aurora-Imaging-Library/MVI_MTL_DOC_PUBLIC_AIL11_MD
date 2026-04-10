---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Customizing_write_operation_settings
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Customizing write operation settings"
---

# Customizing write operation settings

This section provides information to consider and describes settings that you might have to change for write operations.

It is strongly recommended that you define your codes as completely as possible using the available control settings, since it will facilitate reading them later on. When writing a code, ensure that you are familiar with the requirements and restrictions of your chosen code type; otherwise, the code will not be generated and an Aurora Imaging Library error will result.

## Destination buffer size

The destination image buffer of the write operation should be large enough to hold the encoded string. For a given code type, cell size, and string, you can inquire about the minimum buffer size required by first calling [`McodeWrite`](../../Reference/code/McodeWrite.md) with its image buffer parameter set to [`M_NULL`](../../Reference/code/McodeWrite.md) and then using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_WRITE_SIZE_X`](../../Reference/code/McodeGetResult.md) and [`M_WRITE_SIZE_Y`](../../Reference/code/McodeGetResult.md).

## Special characters

If you need to encode unprintable characters, such as a carriage return or tab, use a code type that supports them. If you use [`McodeWrite`](../../Reference/code/McodeWrite.md) with [`M_ESCAPE_SEQUENCE`](../../Reference/code/McodeWrite.md), you can use ASCII codes to encode all unprintable characters. In the string to encode, the unprintable character's ASCII code must be in hexadecimal format and be preceded by \x (for example, \x0D for ASCII 13, which is the carriage return).

> **Note:** If you want to encode the "\" character in escape sequence mode, type "\\".

The encoded string for a Codabar code type must be numeric, but can contain the following characters: minus (-), dollar sign ($), colon (:), slash (/), period (.), and plus (+). In addition, the string must start and end with any of the following characters: a, b, c, or d. The encoded string for a Maxicode code type with the [`M_ENC_MODE2`](../../Reference/code/McodeControl.md) or [`M_ENC_MODE3`](../../Reference/code/McodeControl.md) encoding scheme, must respect the structured carrier message format (the portion of the string that contains postal code, country code, and class of service information).

## Foreground color

Setting the foreground color is essential for write operations. To set the foreground color, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md). The default foreground color is black.

## Cell size, number, and shape

In most cases, you should not have to specify the cell size when writing codes; the default setting ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) set to [`M_DEFAULT`](../../Reference/code/McodeControl.md)) is sufficient. In this case, the code is resized so as to just fit into the destination image of the operation, when possible. During a write operation, you can use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) to force the cell size; the cell size is used to determine the size of the generated code. If you specify the required cell size with [`McodeControl`](../../Reference/code/McodeControl.md), you should ensure that the destination image has an appropriate size. See [Destination buffer size](S10_Customizing_write_operation_settings.md).

> **Note:** The [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md) control type is not used in write operations.

During a write operation, the number of cells will be automatically chosen to minimize the code written. To specify the number of cells in a 2D code, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) and [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md). If you specify more cells than are necessary to generate the code, fill characters will automatically be added. If you specify fewer cells than are necessary to generate the code, you will get an Aurora Imaging Library error. Specifying the number of cells is mostly useful when writing PDF417, Truncated PDF417, Data Matrix, and QRCodes code types; the Maxicode code type has a fixed number of cells. If you specify fewer cells than are necessary to generate a DotCode, a code with the same aspect ratio at an appropriate size will be generated. For the PDF417 code type, [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) must equal `17_c_ + 35`, where `_c_` is the number of columns, and 35 represents the number of cells required for the start and stop patterns. For the Truncated PDF417 code type, [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) must equal `17_c_ + 18`, where `_c_` is the number of columns, and 18 represents the number of cells required for the start patterns.

For Aztec, Data Matrix, DotCode, QR code, and Micro QR code types, you can specify the shape and size of the dots used to draw foreground cells by calling [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SHAPE`](../../Reference/code/McodeControl.md) and [`M_DOT_SIZE`](../../Reference/code/McodeControl.md), respectively. [`M_DOT_SIZE`](../../Reference/code/McodeControl.md) is useful for replicating real world situations where ink might bleed into neighboring cells, or not fill the cells. By default, the dot size is the same as the cell size.

*[Image: McodeDotShapeAndSizeExample.png]*

## Bearer bars

Note that Aurora Imaging Library does not generate bar codes with bearer bars. To generate a code with bearer bars, generate the required code and then draw a rectangle at the top and bottom of the code using [`MgraRectFill`](../../Reference/gra/MgraRectFill.md); the minimum width of the bearer bar (height of the rectangle) must respect the specifications of your code type.

## Writing EAN 14 code types

Strings encoded with the EAN 14 code type should all start with "(01)", which helps to identify the encoding type of the string. If, for some reason, the string to encode does not start with "(01)", [`McodeWrite`](../../Reference/code/McodeWrite.md) will add it automatically. The string to encode as an EAN 14 code can be either 13 or 14 digits in length. The optional fourteenth digit is a check digit calculated based on the first 13 digits according to Modulo 10. If the fourteenth digit is provided, Aurora Imaging Library will validate it. If it is not provided, however, Aurora Imaging Library will calculate it and add it.

## Writing GS1-128 code types

Strings encoded with the GS1-128 code type typically start with a two-digit number inside parentheses (for example, (13)). The parentheses surrounding the number will not be written as part of the encoded string, as long as the number matches one of the valid application identifiers for GS1-128 (EAN/UCC_128). If this number is invalid, the parentheses are written as normal characters in the code.
