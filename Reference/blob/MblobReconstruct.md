---
doctype: Reference
module: blob
function: MblobReconstruct
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / blob / MblobReconstruct"
---

# MblobReconstruct

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

> Reconstruct blobs (or blob holes) in an image buffer.

## Syntax

```c
void MblobReconstruct(
    AIL_ID    SrcImageBufId,   //in
    AIL_ID    SeedImageBufId,  //in
    AIL_ID    DstImageBufId,   //out
    AIL_INT64 Operation,       //in
    AIL_INT64 ProcMode         //in
)
```

## Description

This function copies (or reconstructs) blobs or blob holes from the source to the destination buffer, according to the specified operation and processing mode. By default, all non-zero pixels in the source buffer are considered to be part of a blob. Use the [`M_FOREGROUND_ZERO`](../../Reference/blob/MblobReconstruct.md) processing mode to inverse this behavior.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The source buffer must be a single band, packed binary, 8- or 16-bit unsigned buffer.

### `SeedImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to use as a seed image. The seed image buffer must be a single band, packed binary, 8- or 16-bit unsigned buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination (processed blobs) image buffer. The destination buffer must be a single band, packed binary, 8- or 16-bit unsigned buffer.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. This parameter can be set to one of the following values:

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_ERASE_BORDER_BLOBS` | All blobs that do not touch the borders of the source image are copied to the destination image buffer according to the selected processing mode. Pixels of the blobs that touch the border are replaced according to the selected processing mode.

*[Image: BlobReconstruction.png]*

Note that when the [`M_GRAYSCALE`](../../Reference/blob/MblobReconstruct.md) and [`M_FOREGROUND_ZERO`](../../Reference/blob/MblobReconstruct.md) processing modes are selected, the border blobs are filled with the average grayscale pixel value of the background. This operation is similar to a "border kill". |
| `M_EXTRACT_HOLES` | All holes within the blobs of the source buffer are copied to the destination buffer with their pixel values set to their corresponding blob's pixel values, according to the selected processing mode. A hole cannot touch any image border to be considered a hole.

*[Image: BlobReconstruction4.png]*

Note that, in the [`M_GRAYSCALE`](../../Reference/blob/MblobReconstruct.md) processing mode, the pixel values of holes copied to the destination buffer are set to the blob's average grayscale pixel value in the source buffer. Also, when the [`M_GRAYSCALE`](../../Reference/blob/MblobReconstruct.md) and [`M_FOREGROUND_ZERO`](../../Reference/blob/MblobReconstruct.md) processing modes are selected, the blobs are filled with the average grayscale pixel value of the background. |
| `M_FILL_HOLES` | All blobs in the source buffer are copied to the destination buffer according to the selected processing mode, and those blobs with holes are filled according to the processing mode. A hole must not touch the border of the image to be considered a hole.

*[Image: BlobReconstruction3.png]*

Note that when the [`M_GRAYSCALE`](../../Reference/blob/MblobReconstruct.md) processing mode is selected, holes are filled with the average grayscale pixel value of the corresponding blob. |
| `M_RECONSTRUCT_FROM_SEED` | All blobs in the source buffer that have at least one corresponding foreground seed pixel in the seed buffer are copied to the destination buffer, according to the selected processing mode. Blobs that are not seeded are replaced according to the selected processing mode.

*[Image: BlobReconstruction2.png]*

Note that when the [`M_GRAYSCALE`](../../Reference/blob/MblobReconstruct.md) and [`M_FOREGROUND_ZERO`](../../Reference/blob/MblobReconstruct.md) processing modes are selected, blobs in the source image that are not seeded are filled with the average grayscale value of the background. Also, the value of the seed pixels must be strictly zero for this operation to be performed properly.

Note that the seed image and the destination image must have identical dimensions. |

### `ProcMode` *(in, AIL_INT64)*

Specifies the processing mode to use.

*For specifying the processing mode to use*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default processing mode (which corresponds to [`M_BINARY`](../../Reference/blob/MblobReconstruct.md) + [`M_8_CONNECTED`](../../Reference/blob/MblobReconstruct.md)). |
| `M_BINARY` | Specifies that non-zero pixel values will be treated as ones (1) during processing, and the resulting non-zero pixels copied to the destination buffer will be set to the maximum value of that buffer (for example, 0xff for an 8-bit buffer). Note, in general, the [`M_BINARY`](../../Reference/blob/MblobReconstruct.md) processing mode is faster. |
| `M_GRAYSCALE` | Specifies that the values of pixels copied to the destination buffer will be changed to the values of corresponding pixels in the source buffer. |

*For selecting the type of connectivity*

| Value | Description |
| --- | --- |
| `M_4_CONNECTED` | Specifies that blobs are computed on a four connected lattice. |
| `M_8_CONNECTED` *(default)* | Specifies that blobs are computed on an eight connected lattice. |

*For selecting the foreground pixels*

| Value | Description |
| --- | --- |
| `M_FOREGROUND_ZERO` | Specifies that the pixel values of blobs will consist of zero values and the pixels of the background will consists of non-zero values; that is, the inverse of the usual blob pixel value definition. |

*For accelerating blob reconstruction with seed images*

| Value | Description |
| --- | --- |
| `M_SEED_PIXELS_ALL_IN_BLOBS` | Optimizes the reconstruction process. Use this value in cases when all seed pixels in the seed buffer have corresponding blob pixels in the source buffer. This condition often exists when the seed buffer is an eroded (see [`MimErode`](../../Reference/im/MimErode.md)) version of the source buffer. |
