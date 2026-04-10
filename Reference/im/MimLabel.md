---
doctype: Reference
module: im
function: MimLabel
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimLabel"
---

# MimLabel

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

> Label objects in an image buffer.

## Syntax

```c
void MimLabel(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT64 ProcMode        //in
)
```

## Description

This function labels all objects (or blobs) in the specified source image, starting from the top-left corner, with unique consecutive values beginning with the value 1. Each pixel in the same blob is given the same numerical value.

If you want to distinguish between touching blobs, you should separate the blobs. For example, you can use an erosion operation before performing the labeling operation.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies identifier of the data source of the operation. This parameter must be given an image buffer identifier. If the source buffer is not binary, all non-zero pixels are considered as part of an object or blob.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results. This parameter must be given an image buffer identifier. The destination buffer should be large enough to hold the maximum number of objects (blobs). For example, an 8-bit buffer can be used for a maximum of 254 blobs and a 16-bit buffer can be used for a maximum of 65534 blobs). If the destination buffer depth is too small, several blobs might be given the same value.

### `ProcMode` *(in, AIL_INT64)*

Specifies the type of connectivity (lattice) to use for processing. This parameter can be set to one of the following values:

*For specifying the type of connectivity to use*

| Value | Description |
| --- | --- |
| `M_4_CONNECTED` | Specifies that each pixel has 4 neighbors. If two blobs touch on the vertical and horizontal axis, they are considered one blob. |
| `M_8_CONNECTED` | Specifies that each pixel has 8 neighbors. If two blobs touch on the vertical, horizontal, or diagonal, they are considered one blob. |

*For the ProcMode parameter*

| Value | Description |
| --- | --- |
| `M_BINARY` | Specifies the binary processing mode.

If the source image is a binarized image (containing 0 or the maximum value of the buffer, achieved for example with [`MimBinarize`](../../Reference/im/MimBinarize.md) or [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md)), you can add [`M_BINARY`](../../Reference/blob/MblobReconstruct.md) to the connectivity mode to accelerate processing, particularly if the labeling is done by the Host. |
| `M_GRAYSCALE` *(default)* | Specifies the grayscale processing mode. |

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
