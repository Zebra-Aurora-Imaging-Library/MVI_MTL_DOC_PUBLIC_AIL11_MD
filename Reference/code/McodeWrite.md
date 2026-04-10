---
doctype: Reference
module: code
function: McodeWrite
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeWrite"
---

# McodeWrite

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Encode an ASCII string into a symbol.

## Syntax

```c
void McodeWrite(
    AIL_ID             ModelCodeId,       //in
    AIL_ID             ImageBufId,        //out
    AIL_CONST_TEXT_PTR StringPtr,         //in
    AIL_INT64          ControlFlag,       //in
    AIL_ID             WriteResultCodeId  //out
)
```

## Description

This function encodes a null-terminated ASCII string into a symbol (code) using the code type and encoding scheme, specified by the code model, and draws the code into a specified image.

For a given code model and string, you can inquire the minimum buffer size required to draw the code, using [`McodeWrite`](../../Reference/code/McodeWrite.md) with its [`ImageBufId`](../../Reference/code/McodeWrite.md) parameter set to [`M_NULL`](../../Reference/code/McodeWrite.md), and then using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_WRITE_SIZE_X`](../../Reference/code/McodeGetResult.md) and [`M_WRITE_SIZE_Y`](../../Reference/code/McodeGetResult.md).

The control type settings of the specified code model determine how to perform the operation. Before performing a [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, you must set certain controls using [`McodeControl`](../../Reference/code/McodeControl.md) or verify that their default settings are appropriate, specifically:

| Control | Notes |
| --- | --- |
| [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md), [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) | Specifies the number of cells in the X and Y directions. Improves the robustness of the operation, for 2D code types only. If these settings are set to [`M_DEFAULT`](../../Reference/code/McodeControl.md), then the number of cells will be selected automatically so as to minimize the size of the resulting code. |
| [`M_CELL_SIZE_MODE`](../../Reference/code/McodeControl.md) | Specifies how to establish the cell size. |
| [`M_CELL_SIZE_VALUE`](../../Reference/code/McodeControl.md) | Specifies the cell size to use when [`M_CELL_SIZE_MODE`](../../Reference/code/McodeControl.md) is set to [`M_USER_DEFINED`](../../Reference/code/McodeControl.md). |
| [`M_DOT_SHAPE`](../../Reference/code/McodeControl.md) | Specifies the shape of the dots used to draw foreground cells. This control type can be set for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types. |
| [`M_DOT_SIZE`](../../Reference/code/McodeControl.md) | Specifies the size of the dot (or square) used to draw foreground cells. This control type can be set for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types. |
| [`M_ENCODING`](../../Reference/code/McodeControl.md) | Specifies the encoding scheme. This control type must be set for code types where [`M_ANY`](../../Reference/code/McodeControl.md) is not supported as the encoding scheme. |
| [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) | Specifies the type of error correction. This control type must be set for code types where [`M_ANY`](../../Reference/code/McodeControl.md) is not a supported error correction type. |
| [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md) | Specifies the foreground color of the code. This control type is essential for all code types. |

If you have associated the target image with a camera calibration context, you can specify that certain input settings be interpreted in world units. To do so, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md) set to [`M_WORLD`](../../Reference/code/McodeControl.md). Note that if you set this constant to [`M_WORLD`](../../Reference/code/McodeControl.md) but you don't pass [`McodeWrite`](../../Reference/code/McodeWrite.md) a calibrated target image, the function will generate an error.

## Parameters

### `ModelCodeId` *(in, AIL_ID)*

Specifies the identifier of the code model.

### `ImageBufId` *(out, AIL_ID)*

Specifies the 8-bit unsigned 1-band image buffer in which to write the symbol. Set this parameter to one of the following.

*For specifying the image buffer*

| Value | Description |
| --- | --- |
| `M_NULL` | Allows you to inquire about the minimum buffer size required. Using this value means that [`McodeWrite`](../../Reference/code/McodeWrite.md) will not write into an image buffer.

After calling [`McodeWrite`](../../Reference/code/McodeWrite.md) with [`M_NULL`](../../Reference/code/McodeWrite.md), call [`McodeGetResult`](../../Reference/code/McodeGetResult.md) to retrieve the minimum width ([`M_WRITE_SIZE_X`](../../Reference/code/McodeGetResult.md)) and height ([`M_WRITE_SIZE_Y`](../../Reference/code/McodeGetResult.md)) required for the image buffer. |
| `Image buffer identifier` | Specifies that [`McodeWrite`](../../Reference/code/McodeWrite.md) will write into the image buffer, and in which image buffer to write. This buffer must be 8-bit unsigned. In addition, it should be large enough to hold the symbol. |

### `StringPtr` *(in, AIL_CONST_TEXT_PTR)*

Specifies the address of the string to be encoded.

*For specifying the string of the code*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that a sample string will be encoded. This can be used if you need to test or use the functionality of [`McodeWrite`](../../Reference/code/McodeWrite.md) without needing to encode a specific string (for example, to draw the code type on a button in an interface). The sample string that is encoded depends on the control values set by [`McodeControl`](../../Reference/code/McodeControl.md). |
| `"String"` | Specifies the address of the string. You need to know the string specifications of the code to write.

Note the following restrictions for certain code types:

- If the code type is a Codabar code, the string must be numeric, but can contain the following ASCII characters: minus (-), dollar sign ($), colon (:), forward slash (/), period (.), and plus (+). In addition, the string must start and end with any of the following characters: a, b, c, or d.
- If the code type is an Interleaved 2 of 5 code, the string must have an even number of characters.
- If the code type is a Maxicode code, with [`M_ENCODING`](../../Reference/code/McodeControl.md) set to [`M_ENC_MODE2`](../../Reference/code/McodeControl.md) or [`M_ENC_MODE3`](../../Reference/code/McodeControl.md), the string must respect the structured carrier message format (the portion of the string that contains postal code, country code, and class of service information).
- If the code type is a composite code, the string must be passed in the following format: 1D string|2D string, where | is used as the separator.
- If the code type uses an [`M_ENC_MODE2`](../../Reference/code/McodeControl.md) or [`M_ENC_MODE3`](../../Reference/code/McodeControl.md) encoding scheme, the string is truncated so that only the string that can fit in a single code is written. Aurora Imaging Library does not support a Structured Append sequence of separate but logically linked ECC 200 codes.
- If the code type is a composite code, the FNC1 separator is represented by the unprintable character with ASCII value 29.

In addition, add parentheses around the application identifiers in your string. Note that this is required by the [`McodeWrite`](../../Reference/code/McodeWrite.md) operation; the parentheses are not actually coded. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter can be set to one or more of the following value: (for example,[`M_ESCAPE_SEQUENCE`](../../Reference/code/McodeWrite.md) + [`M_GS1_FORMAT`](../../Reference/code/McodeWrite.md)).

*For specifying the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the string to encode does not use ASCII character codes for unprintable characters. Unprintable characters are not encoded. In addition, the string is not written in GS1 format.

This value cannot be combined with any other value. |
| `M_ESCAPE_SEQUENCE` | Specifies that the string to encode uses ASCII character codes for unprintable characters.

You must use a code type that supports unprintable characters specified using ASCII codes. When typing the string to encode, the unprintable character's ASCII character code must be in hexadecimal format, preceded by \x (for example, \x0D for ASCII 13, which is the carriage return).

Note that if you want to encode the "\" character in escape sequence mode, type "\\". |
| `M_GS1_FORMAT` | Specifies that the string should be written as a GS1 code.

This value is only available when dealing with [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_CODE128`](../../Reference/code/McodeModel.md), [`M_EAN14`](../../Reference/code/McodeModel.md), [`M_GS1_128`](../../Reference/code/McodeModel.md), and [`M_QRCODE`](../../Reference/code/McodeModel.md) code types. |

### `WriteResultCodeId` *(out, AIL_ID)*

Specifies the identifier of the code write result buffer. The result buffer must have been allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_WRITE_RESULT`](../../Reference/code/McodeAllocResult.md).
