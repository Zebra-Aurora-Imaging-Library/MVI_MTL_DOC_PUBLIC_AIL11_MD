---
doctype: Reference
module: im
function: MimThin
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimThin"
---

# MimThin

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

> Perform a binary or grayscale thinning operation on an image.

## Syntax

```c
void MimThin(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT   NbIteration,    //in
    AIL_INT64 ProcMode        //in
)
```

## Description

This function performs a binary or grayscale thinning on the specified source image for the specified number of iterations.

The overscan pixels are automatically set to the highest possible buffer value, which will produce the most accurate possible results for the image border pixels.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the resulting image. This parameter must be given an image buffer identifier.

### `NbIteration` *(in, AIL_INT)*

Specifies the number of times to iterate the operation. This parameter must be set to one of the values below.

*For specifying the number of times to iterate the operation*

| Value | Description |
| --- | --- |
| `M_TO_SKELETON` | Specifies that every object will be reduced to its skeleton. |
| `Value >= 0` | Specifies the number of iterations.

If this parameter is set to 0, and [`ProcMode`](../../Reference/im/MimThin.md) is set to [`M_BINARY`](../../Reference/im/MimThin.md), the source image is binarized and the result is copied into the destination image buffer.

If this parameter is set to 0, and [`ProcMode`](../../Reference/im/MimThin.md) is set to [`M_GRAYSCALE`](../../Reference/im/MimThin.md), the source image is copied into the destination image buffer. |

### `ProcMode` *(in, AIL_INT64)*

Specifies the processing mode to use. This parameter must be set to one of the values below.

*For specifying the processing mode to use*

| Value | Description |
| --- | --- |
| `M_BINARY` | Treats non-zero pixels as ones (1) during processing. The resulting non-zero pixels will have all bits set to one. |
| `M_BINARY2` | Treats non-zero pixels as ones (1) during processing and produces a result that better preserves the original topology of the objects in the source image. Similar to [`M_BINARY`](../../Reference/im/MimThin.md), the resulting non-zero pixels will also have all bits set to one. However, this mode produces a result that is more consistent with the one produced using the [`M_GRAYSCALE`](../../Reference/im/MimThin.md) processing mode. Note that this processing mode is slower than using the[`M_BINARY`](../../Reference/im/MimThin.md) or [`M_BINARY3`](../../Reference/im/MimThin.md) processing modes. |
| `M_BINARY3` | Treats non-zero pixels as ones (1) during processing and produces a faster result than other processing modes. Similar to [`M_BINARY`](../../Reference/im/MimThin.md), the resulting non-zero pixels will also have all bits set to one. However, this mode produces a result that is more consistent with the one produced using the [`M_GRAYSCALE`](../../Reference/im/MimThin.md) processing mode. Note that this processing mode does not preserve the original topology of objects in the source image as well as [`M_BINARY2`](../../Reference/im/MimThin.md). Overscan pixels are treated as zeroes instead of ones. |
| `M_GRAYSCALE` | Uses the source image's gray values for processing. The resulting pixels will be grayscale. |
