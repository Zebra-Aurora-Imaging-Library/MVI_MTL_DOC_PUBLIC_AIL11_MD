---
doctype: Reference
module: code
function: McodeDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeDraw"
---

# McodeDraw

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

> Draw specific features of results obtained from an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeWrite`](../../Reference/code/McodeWrite.md), or [`McodeDetect`](../../Reference/code/McodeDetect.md) operation.

## Syntax

```c
void McodeDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ResultCodeId,            //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   ResultIndex,             //in
    AIL_INT   ScanIndex,               //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of results, obtained from an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeWrite`](../../Reference/code/McodeWrite.md), or [`McodeDetect`](../../Reference/code/McodeDetect.md) operation, in the specified destination image buffer or 2D graphics list.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ResultCodeId` *(in, AIL_ID)*

Specifies the identifier of the code read, grade, or detect result buffer from which to extract features to draw. The code result buffer must have been previously allocated on the required system using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_READ_RESULT`](../../Reference/code/McodeAllocResult.md), [`M_CODE_GRADE_RESULT`](../../Reference/code/McodeAllocResult.md), [`M_CODE_WRITE_RESULT`](../../Reference/code/McodeAllocResult.md), or [`M_CODE_DETECT_RESULT`](../../Reference/code/McodeAllocResult.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw specific features of results.

### `Operation` *(in, AIL_INT64)*

Specifies the type of drawing operation to perform. Drawing operations can be added together to draw multiple features at a time. For example, to draw both the result occurrence's position and code symbol as it was read using [`McodeRead`](../../Reference/code/McodeRead.md), set the [`Operation`](../../Reference/code/McodeDraw.md) parameter to [`M_DRAW_POSITION`](../../Reference/code/McodeDraw.md)+[`M_DRAW_CODE`](../../Reference/code/McodeDraw.md).

*For specifying the type of drawing operation after an*

| Value | Description |
| --- | --- |
| `M_DRAW_CODE` | Draws the specified code occurrence as it was read by [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md).

This drawing operation is not supported for GS1 Databar, MicroPDF417, and composite code types. |
| `M_DRAW_DECODED_SCANS` | Draws the specified decoded scanline(s) that an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation used to read the specified code occurrence(s). The scanlines are drawn using their start and end points ([`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_DECODED_SCANS_START...`](../../Reference/code/McodeGetResult.md)and [`M_DECODED_SCANS_END...`](../../Reference/code/McodeGetResult.md)).

You can specify the scanline for which to draw results using [`ScanIndex`](../../Reference/code/McodeDraw.md).

This drawing operation is available for linear and composite code types.

For composite code types, only the scanlines in the 1D portion of the code occurrence(s) are drawn. |

*For specifying the type of drawing operation after an*

| Value | Description |
| --- | --- |
| `M_DRAW_POSITION` | Draws a cross-like symbol at the mid-point of the code occurrence. Note that for a Data Matrix code occurrence, after an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, the cross symbol is drawn in the middle of the top-left cell. For a composite code occurrence, the cross is drawn at the midpoint of the 1D part of the code. For a Maxicode code occurrence, the cross appears in the center of the bull's eye pattern. Regardless of the code type, the cross is drawn maintaining the angle of the occurrence. |

*For specifying the type of drawing operation after an*

| Value | Description |
| --- | --- |
| `M_DRAW_EXTENDED_AREA` | Draws a box around the specified code occurrence, including its extended area, as analyzed by the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

Note that this drawing operation is only available if [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) was enabled for the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

To establish the dimensions of occurrence with its extended area, use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_EXTENDED_AREA_TOP...`](../../Reference/code/McodeGetResult.md) and [`M_EXTENDED_AREA_BOTTOM...`](../../Reference/code/McodeGetResult.md).

This drawing operation is available for Aztec, Data Matrix, QR code, and Micro QR code types. |
| `M_DRAW_REFLECTANCE_PROFILE` | Draws the scan reflectance profile of the specified scanline(s) of the code occurrence, as analyzed by the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. You can specify the scanline for which to draw results using [`ScanIndex`](../../Reference/code/McodeDraw.md).

The result is not scaled to the size of the destination image buffer. For best results, the image buffer must have a height of at least 256. |
| `M_DRAW_SCAN_PROFILES` | Draws the specified scanline(s) analyzed in the code occurrence by the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. You can specify the scanline for which to draw results using [`ScanIndex`](../../Reference/code/McodeDraw.md).

The drawn scanline(s) represents only the region(s) from which the results were successfully located. |

*For specifying a drawing operation that is available after either an*

| Value | Description |
| --- | --- |
| `M_DRAW_BOX` | Draws a box around the code occurrence. This box is drawn according to the corner coordinates of the code occurrence. You can retrieve these coordinates using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_BOTTOM...`](../../Reference/code/McodeGetResult.md) and [`M_TOP...`](../../Reference/code/McodeGetResult.md). The box only contains the part of the code occurrence that was processed; for occurrences of 1D code types from an [`McodeRead`](../../Reference/code/McodeRead.md) operation performed in low accuracy ([`McodeControl`](../../Reference/code/McodeControl.md)with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_LOW`](../../Reference/code/McodeControl.md)), this means that the box might not encompass the entire code occurrence.

The box is drawn maintaining the angle and scale of the occurrence.

When dealing with composite code occurrences (only supported by [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), and [`McodeWrite`](../../Reference/code/McodeWrite.md) operations), the box is drawn around the 1D component of the code occurrence. To draw the box around the 2D component of a code occurrence, combine [`M_DRAW_BOX`](../../Reference/code/McodeDraw.md) with [`M_2D_COMPONENT`](../../Reference/code/McodeDraw.md).

> **Note:** Note that after an [`McodeDetect`](../../Reference/code/McodeDetect.md)operation, you must use this value alone; it cannot be used in combination with any other value. |
| `M_DRAW_QUIET_ZONE` | Draws a box around the specified code occurrence, including its quiet zone, as analyzed by the [`McodeRead`](../../Reference/code/McodeRead.md),[`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

The box is drawn maintaining the angle and scale of the occurrence.

To establish the dimensions of occurrence with its quiet zone, use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_QUIET_ZONE_TOP...`](../../Reference/code/McodeGetResult.md) and [`M_QUIET_ZONE_BOTTOM...`](../../Reference/code/McodeGetResult.md). The box only contains the part of the code occurrence that was processed; for occurrences of 1D code types from an [`McodeRead`](../../Reference/code/McodeRead.md) operation performed in low accuracy ([`McodeControl`](../../Reference/code/McodeControl.md)with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_LOW`](../../Reference/code/McodeControl.md)), this means that the box might not encompass the entire code occurrence or might include white space.

> **Note:** Note that this drawing operation is not available after an [`McodeDetect`](../../Reference/code/McodeDetect.md) operation. |

*For specifying the component of a composite code occurrence for which to perform the drawing after an*

| Value | Description |
| --- | --- |
| `M_LINEAR_COMPONENT` | Draws the grading result for the 1D component (linear component) of a composite code occurrence. |

### `ResultIndex` *(in, AIL_INT)*

Specifies the code occurrence for which to draw results.

*For specifying the occurrence index of the code result*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Performs the specified operation for all code occurrences in the code result buffer. |
| `0 <= OccurrenceIndex < M_NUMBER` | Specifies the index of the code occurrence in the code result buffer.

See [Retrieving results](../../UserGuide/C17_Codes/S11_Retrieving_results.md) for more information on code occurrence indices. |

### `ScanIndex` *(in, AIL_INT)*

Specifies the scanline(s) for which to draw results.

*For specifying the scanline*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

For a scanline-specific drawing operation, this value is the same as [`M_ALL`](../../Reference/code/McodeDraw.md). For a general drawing operation, this value is the same as [`M_GENERAL`](../../Reference/code/McodeDraw.md). |
| `M_ALL` | Specifies to perform the scanline-specific drawing operation for all scanlines of the specified code occurrence(s). |
| `M_GENERAL` | Specifies to perform the drawing operation without specifying a scanline.

> **Note:** Note that this value is not supported for the [`M_DRAW_REFLECTANCE_PROFILE`](../../Reference/code/McodeDraw.md) and [`M_DRAW_SCAN_PROFILES`](../../Reference/code/McodeDraw.md) drawing operations. |
| `0 <= Value < M_NUMBER_OF_DECODED_SCANS` | Specifies the index of the decoded scanline for which to perform the drawing operation, for the specified code occurrence.

> **Note:** Note that this value cannot be used if [`ResultIndex`](../../Reference/code/McodeDraw.md) is set to [`M_ALL`](../../Reference/code/McodeDraw.md) or [`M_DEFAULT`](../../Reference/code/McodeDraw.md). |
| `0 <= Value < M_NUMBER_OF_SCANS` | Specifies the index of the scanline for which to perform the drawing operation, for the specified code occurrence.

> **Note:** Note that this value cannot be used if [`ResultIndex`](../../Reference/code/McodeDraw.md) is set to [`M_ALL`](../../Reference/code/McodeDraw.md) or [`M_DEFAULT`](../../Reference/code/McodeDraw.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
