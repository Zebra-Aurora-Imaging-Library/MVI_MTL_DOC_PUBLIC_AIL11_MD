---
doctype: Reference
module: im
function: MimClose
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimClose"
---

# MimClose

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

> Perform a binary or grayscale closing-type morphological operation.

## Syntax

```c
void MimClose(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT   NbIteration,    //in
    AIL_INT64 ProcMode        //in
)
```

## Description

This function performs a binary or grayscale closing operation on the given source image for the specified number of iterations. A closing is a dilation followed by an erosion.

In binary mode, this function uses a 3x3 full rectangular structuring element; in grayscale mode, a 3x3 empty one.

The overscan pixels are automatically set to the lowest possible buffer value for the dilation and the highest possible buffer value for the erosion; this will produce the most accurate possible results for the image border pixels.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the resulting image. This parameter must be given an image buffer identifier.

### `NbIteration` *(in, AIL_INT)*

Specifies the number of times to iterate the operation.

### `ProcMode` *(in, AIL_INT64)*

Specifies the processing mode to use. This parameter can be set to the following:

*For specifying the processing mode*

| Value | Description |
| --- | --- |
| `M_BINARY` | Treats non-zero pixels as ones (1) during processing. The resulting non-zero pixels will have all bits set to one. |
| `M_GRAYSCALE` | Uses the source image's gray values for processing. The resulting buffer will also contain gray values. |
