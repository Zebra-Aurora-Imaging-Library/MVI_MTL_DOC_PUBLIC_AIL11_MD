---
doctype: Reference
module: ocr
function: MocrReadString
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrReadString"
---

# MocrReadString

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

> Read an unknown string from a target image.

## Syntax

```c
void MocrReadString(
    AIL_ID ImageBufId,        //in
    AIL_ID FontContextOcrId,  //in
    AIL_ID OcrResultId        //out
)
```

## Description

This function reads an unknown string from the specified target image using the specified OCR font context. All existing font controls and constraints (which can be set with [`MocrControl`](../../Reference/ocr/MocrControl.md) and [`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md) respectively) are taken into account, and results are stored in the specified result buffer. Results can be read from the result buffer using the [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md) function.

This function assumes that the string to be read has the same length as specified in the font, and that the target string characters have the same type, size, and spacing as the characters in OCR font context used for font calibration (either automatically with[`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md) or manually with [`MocrControl`](../../Reference/ocr/MocrControl.md)). This is particularly important when using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context.

For best results, the angle of the string(s) to be read should be as close to 0 as possible.

You can limit the operation to a region of the image buffer, using a rectangular region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

When reading multiple strings, performing a search at an angle or using the [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, the target image should have a clearly-defined threshold between the characters and their background. Note that the threshold must preserve the shape of the characters. Broken and/or touching characters can degrade the results.

Note that before performing a read operation, the OCR font context must be preprocessed ([`MocrPreprocess`](../../Reference/ocr/MocrPreprocess.md)).

## Parameters

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image which contains the string to be read. The image buffer must be a 1-band buffer.

### `FontContextOcrId` *(in, AIL_ID)*

Specifies the identifier of the OCR font context to use to read the string from the target image.

### `OcrResultId` *(out, AIL_ID)*

Specifies the OCR result buffer in which to place results of the read operation.
