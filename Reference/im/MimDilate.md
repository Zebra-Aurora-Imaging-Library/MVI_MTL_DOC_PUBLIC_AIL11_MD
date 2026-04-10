---
doctype: Reference
module: im
function: MimDilate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimDilate"
---

# MimDilate

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

> Perform a binary or grayscale dilation-type morphological operation.

## Syntax

```c
void MimDilate(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT   NbIteration,    //in
    AIL_INT64 ProcMode        //in
)
```

## Description

This function performs a binary or grayscale dilation on the given source image for the specified number of iterations.

In binary mode, this function uses a 3x3 full rectangular structuring element; in grayscale mode, a 3x3 empty one.

The overscan pixels are automatically set to the lowest possible buffer value, which will produce the most accurate possible results for the image border pixels.

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
| `M_DEFAULT` | Specifies that each non-zero (white) blob should be dilated until its background is about to disappear. This value is only available when [`ProcMode`](../../Reference/im/MimDilate.md) is set to [`M_BINARY_ULTIMATE`](../../Reference/im/MimDilate.md) or [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimDilate.md).

[`M_DEFAULT`](../../Reference/im/MimDilate.md) does not specify an explicit number of iterations to perform, instead it indicates to the function that the blobs should be dilated until black holes and the background are about to disappear. For example, if [`M_DEFAULT`](../../Reference/im/MimDilate.md) is specified and you have two rectangular blobs with two black holes (120x6 and 150x17), the dilation will continue until the black holes are reduced to 116x2 and 134x1, respectively (the smallest they can be before they disappear), and the background is reduced to the point it is about to disappear.

> **Note:** Setting [`NbIteration`](../../Reference/im/MimDilate.md) to [`M_DEFAULT`](../../Reference/im/MimDilate.md) with any processing mode other than [`M_BINARY_ULTIMATE`](../../Reference/im/MimDilate.md) or [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimDilate.md) will cause an error. |
| `Value >= 0` | Specifies the number of iterations.

When this parameter is set to 0 and [`ProcMode`](../../Reference/im/MimDilate.md) is set to [`M_BINARY`](../../Reference/im/MimDilate.md), the source image is binarized and the result is copied into the destination image buffer.

When this parameter is set to 0 and [`ProcMode`](../../Reference/im/MimDilate.md) is set to [`M_GRAYSCALE`](../../Reference/im/MimDilate.md), the source image is copied into the destination image buffer. |

### `ProcMode` *(in, AIL_INT64)*

Specifies the processing mode to use. This parameter can be set to the following:

*For specifying the processing mode*

| Value | Description |
| --- | --- |
| `M_BINARY` | Performs a binary dilation on the white (non-zero) blobs in the image. In this case, non-zero pixels are treated as ones (1) during processing.

The resulting non-zero pixels will have all bits set to one. |
| `M_BINARY_ULTIMATE` | Performs a binary dilation on the white (non-zero) blobs, while preventing the black (zero) holes and background from completely disappearing.

When in this processing mode, this operation performs a binary dilation until the number of iterations [`NbIteration`](../../Reference/im/MimDilate.md) is reached or until the black holes and background are about to disappear. If [`NbIteration`](../../Reference/im/MimDilate.md) is not set to [`M_DEFAULT`](../../Reference/im/MimDilate.md), the specified number of iterations serves as an upper bound limit, and the procedure continues until the specified number of iterations is reached or the black holes and background are about to disappear, whichever comes first. If [`NbIteration`](../../Reference/im/MimDilate.md) is set to [`M_DEFAULT`](../../Reference/im/MimDilate.md), the procedure continues until the black holes and background are about to disappear.

> **Note:** Note that if the dilation merges any blobs, the process continues on the merged blob. The holes and the background, however, are still evaluated individually. |
| `M_BINARY_ULTIMATE_ACCUMULATE` | Performs a binary erosion on the white (non-zero) blobs in the negative of the image, while preventing these blobs from completely disappearing and keeping track of the number of iterations.

> **Note:** Note that the negative of the image is taken and the operation is switched to an erosion, so that each remaining pixel in a hole or the background can track the number of iterations needed to reach the current state.

The resulting non-zero pixels will be set to the iteration number + 1. For example, consider an erosion with [`NbIteration`](../../Reference/im/MimDilate.md) set to 2. After the first iteration, the resulting non-zero pixels will be set to 1 + 1 (iteration number) = 2. After the second iteration,the resulting non-zero pixels will have all bits set to 1 + 2 (iteration number) = 3. This also means that what was considered the foreground is now considered the background, represented in black (zero), and what was considered holes and the background is now non-zero (iteration count).

When in this processing mode, this operation performs a binary erosion on the negative of the image until the number of iterations [`NbIteration`](../../Reference/im/MimDilate.md) is reached or until the blobs are about to disappear. If [`NbIteration`](../../Reference/im/MimDilate.md) is not set to [`M_DEFAULT`](../../Reference/im/MimDilate.md), the specified number of iterations serves as an upper bound limit, and the procedure continues until the specified number of iterations is reached or until the blobs are about to disappear, whichever comes first. If [`NbIteration`](../../Reference/im/MimDilate.md) is set to [`M_DEFAULT`](../../Reference/im/MimDilate.md), the procedure continues until the blobs are about to disappear.

> **Note:** Note that if the erosion splits any blobs, the process continues on each sub-blob, evaluating them separately.

For more information on binary ultimate accumulate dilation, see [Image processing](../../UserGuide/C03_Image_processing/S06_Erosion_and_dilation.md). |
| `M_GRAYSCALE` | Performs a grayscale dilation on the white (non-zero) blobs. In this case, each pixel of the source image is replaced with the maximum value in its neighborhood. |
