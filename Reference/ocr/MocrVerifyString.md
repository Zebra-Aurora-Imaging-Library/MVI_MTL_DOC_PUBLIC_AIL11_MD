---
doctype: Reference
module: ocr
function: MocrVerifyString
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrVerifyString"
---

# MocrVerifyString

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

> Verify a known string in an image.

## Syntax

```c
void MocrVerifyString(
    AIL_ID             ImageBufId,        //in
    AIL_ID             FontContextOcrId,  //in
    AIL_CONST_TEXT_PTR String,            //in
    AIL_ID             OcrResultId        //out
)
```

## Description

This function determines whether a known string is present in the target image, and evaluates the quality of the string. This operations can be performed more quickly with [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md) than with [`MocrReadString`](../../Reference/ocr/MocrReadString.md). After verification, the specified result buffer can be read with the [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md) function.

> **Note:** If string location is disabled (using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SKIP_STRING_LOCATION`](../../Reference/ocr/MocrControl.md)), use a child buffer to limit the search area for best results.

The verification can only be performed on a single string and not multiple strings. When dealing with multiple lines of text in a target image, the returned string is the first one found within the target image. Note that this is not necessarily the first string within the target image. The string is located according to its OCR font context type, constraints, and processing controls. Once located, the string can then be read and compared against the validation string provided by this function.

You can limit the operation to a region of the image buffer, using a rectangular region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

For a more robust search, use an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context. You can change the type of context using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CONTEXT_CONVERT`](../../Reference/ocr/MocrControl.md).

> **Note:** Blank characters cannot be verified and will be ignored.

## Parameters

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the target image containing the string to be verified. The image buffer must be a 1-band buffer.

### `FontContextOcrId` *(in, AIL_ID)*

Specifies the identifier of the OCR font context to use to verify the string in the target image.

### `String` *(in, AIL_CONST_TEXT_PTR)*

Specifies the character string to be verified in the sample target image.

*For specifying the ASCII characters*

| Value | Description |
| --- | --- |
| `"String"` | Specifies the character string to be verified in the sample target image. This string must be null-terminated.

Note that blank characters cannot be verified and will cause an Aurora Imaging Library error. |

### `OcrResultId` *(out, AIL_ID)*

Specifies the OCR result buffer in which to place the verification results.
