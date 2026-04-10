---
doctype: Reference
module: ocr
function: MocrCalibrateFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrCalibrateFont"
---

# MocrCalibrateFont

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

> Calibrate the OCR font context's character size and target spacing to match the characters in a sample target image.

## Syntax

```c
void MocrCalibrateFont(
    AIL_ID             ImageBufId,           //in
    AIL_ID             FontContextOcrId,     //out
    AIL_CONST_TEXT_PTR String,               //in
    AIL_DOUBLE         TargetCharSizeXMin,   //in
    AIL_DOUBLE         TargetCharSizeXMax,   //in
    AIL_DOUBLE         TargetCharSizeXStep,  //in
    AIL_DOUBLE         TargetCharSizeYMin,   //in
    AIL_DOUBLE         TargetCharSizeYMax,   //in
    AIL_DOUBLE         TargetCharSizeYStep,  //in
    AIL_INT64          Operation             //in
)
```

## Description

This function automatically calibrates the X and Y size and spacing of the font of a specified OCR font context to match that of a string in the sample target image. Note that these values can also be set manually with [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md), [`M_TARGET_CHAR_SIZE_X`](../../Reference/ocr/MocrControl.md), and [`M_TARGET_CHAR_SIZE_Y`](../../Reference/ocr/MocrControl.md). This function can only calibrate an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context. You can change the type of context using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CONTEXT_CONVERT`](../../Reference/ocr/MocrControl.md).

The sample image must contain the string specified in the [`String`](../../Reference/ocr/MocrCalibrateFont.md) parameter, and both the size and spacing must be representative of the images to be read or verified with the calibrated OCR font context. This function might not return the expected results if the sample image contains more or fewer characters than the specified target string length ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_CHAR_NUMBER`](../../Reference/ocr/MocrControl.md)).

The better the sample image, the better the font calibration results. If a string cannot be located in the sample image, an error will be generated. Use [`MappGetError`](../../Reference/app/MappGetError.md) to learn the error code and related information.

You can limit the operation to a region of the image buffer, using a rectangular region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

[`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md) should be called after defining a new OCR font context or when the character size in the target images changes.

The resulting target values are the best match between the minimum and the maximum target character sizes specified in the parameters of this function. Font calibration will increment the minimum target character size by a step value until it reaches the maximum target character size. You can inquire the target character sizes and spacing with [`MocrInquire`](../../Reference/ocr/MocrInquire.md). For best results, use a range of +/- 1 pixel with, for example, a step of between 0.1 and 0.2 pixels. Note that the greater the number of steps between the minimum and the maximum target character sizes, the longer this operation will take.

## Parameters

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the sample target image buffer. The characters of the string in this sample image must be of the same type and size as that in the images to be read or verified using the OCR font context. The image buffer must be a 1-band buffer.

### `FontContextOcrId` *(out, AIL_ID)*

Specifies the identifier of the OCR font context to be calibrated.

### `String` *(in, AIL_CONST_TEXT_PTR)*

Specifies the character string present in the sample target image. This string must be null-terminated.

*For specifying the character string*

| Value | Description |
| --- | --- |
| `"String"` | Specifies the character string that is present in the sample target image. This string must be null-terminated. |

### `TargetCharSizeXMin` *(in, AIL_DOUBLE)*

Specifies the minimum width of the characters in the sample target image, in pixels.

*For specifying the minimum width of the characters*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Sets the minimum width, in pixels. Note that the specified value must satisfy the following: `(` [`TargetCharSizeXMin`](../../Reference/ocr/MocrCalibrateFont.md) `/` [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md) `) >= 0.25`. |

### `TargetCharSizeXMax` *(in, AIL_DOUBLE)*

Specifies the maximum width of the characters in the sample target image, in pixels.

*For specifying the maximum width of the characters*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Sets the maximum width, in pixels. Note that the specified value must satisfy the following: `(` [`TargetCharSizeXMax`](../../Reference/ocr/MocrCalibrateFont.md) `/` [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md) `) &lt;= 4.0`. |

### `TargetCharSizeXStep` *(in, AIL_DOUBLE)*

Specifies the increment to use when searching for the character width between [`TargetCharSizeXMin`](../../Reference/ocr/MocrCalibrateFont.md) and [`TargetCharSizeXMax`](../../Reference/ocr/MocrCalibrateFont.md). Note that very small step sizes should only be used within small ranges, otherwise application performance can be adversely affected.

### `TargetCharSizeYMin` *(in, AIL_DOUBLE)*

Specifies the minimum height of the characters in the sample target image, in pixels.

*For specifying the minimum height of the characters*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Sets the minimum height, in pixels. Note that the specified value must satisfy the following: `(` [`TargetCharSizeYMin`](../../Reference/ocr/MocrCalibrateFont.md) `/` [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md) `) >= 0.25`. |

### `TargetCharSizeYMax` *(in, AIL_DOUBLE)*

Specifies the maximum height of the characters in the sample target image, in pixels.

*For specifying the maximum height of the characters*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Sets the maximum height, in pixels. Note that the specified value must satisfy the following: `(` [`TargetCharSizeYMax`](../../Reference/ocr/MocrCalibrateFont.md) `/` [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md) `) &lt;= 4.0`. |

### `TargetCharSizeYStep` *(in, AIL_DOUBLE)*

Specifies the increment to use when searching for the character height between [`TargetCharSizeYMin`](../../Reference/ocr/MocrCalibrateFont.md) and [`TargetCharSizeYMax`](../../Reference/ocr/MocrCalibrateFont.md). Note that very small step sizes should only be used within small ranges, otherwise application performance can be adversely affected.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to be performed. Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
