---
doctype: Reference
module: ocr
function: MocrSaveFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrSaveFont"
---

# MocrSaveFont

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

> Save an existing OCR font context to disk.

## Syntax

```c
void MocrSaveFont(
    AIL_CONST_TEXT_PTR FileName,         //in
    AIL_INT64          Operation,        //in
    AIL_ID             FontContextOcrId  //in
)
```

## Description

This function saves an existing OCR font context to disk using the Aurora Imaging Library font file format. The OCR font context's control, constraint, and/or font character data can all be saved; which data is saved depends on the value of the [`Operation`](../../Reference/ocr/MocrSaveFont.md) parameter.

The font character data contains the character representation of each character in the OCR font context. The control data includes controls used to specify the operational controls for a read/verify operation (such as [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md), and [`M_TOUCHING_CHAR`](../../Reference/ocr/MocrControl.md)) and those used to set the characteristics of the target characters (such as [`M_THICKEN_CHAR`](../../Reference/ocr/MocrControl.md), and [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md)). The constraint data specifies which characters can appear at given positions in the search string.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the font data. It is recommended that you use the MFO file extension for easier use with other Zebra Aurora software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, OCR font context files have an MFO extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `Operation` *(in, AIL_INT64)*

Specifies which data to save to disk. The [`Operation`](../../Reference/ocr/MocrSaveFont.md) parameter can take one of the following values:

*For specifying the data to save*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Saves the same data as [`M_SAVE`](../../Reference/ocr/MocrSaveFont.md). |
| `M_SAVE` | Saves the OCR font context. |
| `M_SAVE_CONSTRAINT` | Saves only character constraint data. |
| `M_SAVE_CONTROL` | Saves only control data. |

### `FontContextOcrId` *(in, AIL_ID)*

Specifies the OCR font context to be saved.
