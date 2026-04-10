---
doctype: Reference
module: im
function: MimErode
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimErode"
---

# MimErode

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

> Perform a binary or grayscale erosion-type morphological operation.

## Syntax

```c
void MimErode(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT   NbIteration,    //in
    AIL_INT64 ProcMode        //in
)
```

## Description

This function performs a binary or grayscale erosion on the given source image for the specified number of iterations.

In binary mode, this function uses a 3x3 full rectangular structuring element; in grayscale mode, a 3x3 empty one.

The overscan pixels are automatically set to the highest possible buffer value, which will produce the most accurate possible results for the image border pixels.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `NbIteration` *(in, AIL_INT)*

Specifies the number of times to iterate the operation.

*For specifying the number of iterations to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that each blob should be eroded until it is about to disappear. This value is only available when [`ProcMode`](../../Reference/im/MimErode.md) is set to [`M_BINARY_ULTIMATE`](../../Reference/im/MimErode.md) or [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimErode.md).

[`M_DEFAULT`](../../Reference/im/MimErode.md) does not specify an explicit number of iterations to perform; instead, it indicates to the function that each blob should be eroded until it is about to disappear. For example, if [`M_DEFAULT`](../../Reference/im/MimErode.md) is specified and you have two rectangular blobs (120x6 and 150x17), the erosion will continue until the blobs are reduced to 116x2 and 134x1, respectively (the smallest they can be before they disappear).

> **Note:** Setting [`NbIteration`](../../Reference/im/MimErode.md) to [`M_DEFAULT`](../../Reference/im/MimErode.md) with any processing mode other than [`M_BINARY_ULTIMATE`](../../Reference/im/MimErode.md) or [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimErode.md) will cause an error. |
| `Value >= 0` | Specifies the number of iterations.

When this parameter is set to 0 and [`ProcMode`](../../Reference/im/MimErode.md) is set to [`M_BINARY`](../../Reference/im/MimErode.md), the source image is binarized and the result is copied into the destination image buffer.

When this parameter is set to 0 and [`ProcMode`](../../Reference/im/MimErode.md) is set to [`M_GRAYSCALE`](../../Reference/im/MimErode.md), the source image is copied into the destination image buffer. |

### `ProcMode` *(in, AIL_INT64)*

Specifies the processing mode to use. This parameter can be set to the following:

*For specifying the processing mode*

| Value | Description |
| --- | --- |
| `M_BINARY` | Performs a binary erosion on the white (non-zero) blobs in the image. In this case, non-zero pixels are treated as ones (1) during processing.

The resulting non-zero pixels will have all bits set to one. |
| `M_BINARY_ULTIMATE` | Performs a binary erosion on the white (non-zero) blobs, while preventing them from completely disappearing.

When in this processing mode, this operation performs a binary erosion until the number of iterations [`NbIteration`](../../Reference/im/MimErode.md) is reached or until the blob is about to disappear. If [`NbIteration`](../../Reference/im/MimErode.md) is not set to [`M_DEFAULT`](../../Reference/im/MimErode.md), the specified number of iterations serves as an upper bound limit, and the procedure continues until the specified number of iterations is reached or the blob is about to disappear, whichever comes first. If [`NbIteration`](../../Reference/im/MimErode.md) is set to [`M_DEFAULT`](../../Reference/im/MimErode.md), the procedure continues until the non-zero blob is about to disappear. Note that if the erosion splits any blobs, the process continues on each sub-blob evaluating them separately. |
| `M_BINARY_ULTIMATE_ACCUMULATE` | Performs a binary erosion on the white (non-zero) blobs, while preventing them from completely disappearing and keeping track of the number of iterations.

When in this processing mode, this operation performs a binary erosion until the number of iterations [`NbIteration`](../../Reference/im/MimErode.md) is reached or until the blob is about to disappear. If [`NbIteration`](../../Reference/im/MimErode.md) is not set to [`M_DEFAULT`](../../Reference/im/MimErode.md), the specified number of iterations serves as an upper bound limit, and the procedure continues until the specified number of iterations is reached or the blob is about to disappear, whichever comes first. If [`NbIteration`](../../Reference/im/MimErode.md) is set to [`M_DEFAULT`](../../Reference/im/MimErode.md), the procedure continues until the non-zero blob is about to disappear.

> **Note:** Note that if the erosion splits any blobs, the process continues on each sub-blob, evaluating them separately.

The resulting non-zero pixels will be set to the iteration number + 1. For example, consider an erosion with [`NbIteration`](../../Reference/im/MimErode.md) set to 2. After the first iteration, the resulting non-zero pixels will be set to 1 + 1 (iteration number) = 2. After the second iteration,the resulting non-zero pixels will be set to 1 + 2 (iteration number) = 3. For more information on erosion see [Image processing](../../UserGuide/C03_Image_processing/S06_Erosion_and_dilation.md).

For more information on binary ultimate accumulate erosion, see [Image processing](../../UserGuide/C03_Image_processing/S06_Erosion_and_dilation.md). |
| `M_GRAYSCALE` | Performs a grayscale erosion on the white (non-zero) blobs in the image. In this case, each pixel of the source image is replaced with the minimum value in its neighborhood. |
