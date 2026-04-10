---
doctype: Reference
module: im
function: MimFilterBinomial
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimFilterBinomial"
---

# MimFilterBinomial

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

> Perform binomial filtering.

## Syntax

```c
void MimFilterBinomial(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT   KernelSizeX,    //in
    AIL_INT   KernelSizeY,    //in
    AIL_INT   Overscan,       //in
    AIL_INT64 ControlFlag     //in
)
```

## Description

This function performs binomial filtering on the specified source image using a specified kernel size in X and Y.

The binomial filter approximates a Gaussian filter using only integer kernel values, resulting in faster computation than other filtering operations. This function also allows for different dimensions in X and Y, unlike other filtering operations.

The dimensions of the kernel ([`KernelSizeX`](../../Reference/im/MimFilterBinomial.md) and [`KernelSizeY`](../../Reference/im/MimFilterBinomial.md)) must be odd integers to ensure there is a central pixel.

This function is useful, for example, to filter out false edges in an image while preserving major foreground edges.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `KernelSizeX` *(in, AIL_INT)*

Specifies the width of the kernel (X-size).

*For specifying the kernel width*

| Value | Description |
| --- | --- |
| `1 <= Value <= 63` | Specifies the width of the kernel. This must be an odd integer. |

### `KernelSizeY` *(in, AIL_INT)*

Specifies the height of the kernel (Y-size).

*For specifying the kernel height*

| Value | Description |
| --- | --- |
| `1 <= Value <= 63` | Specifies the height of the kernel. This must be an odd integer. |

### `Overscan` *(in, AIL_INT)*

Specifies the type of overscan used to handle the source image's bordering pixels.

*For specifying the type of overscan*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MIRROR` | Specifies that the border pixels of a source image are processed using overscan pixel values that mirror the source buffer pixel values. That is, the overscan pixel values will be a mirror copy of the source buffer's borders. For example:

*[Image: KernelOverscan.png]* |
| `M_TRANSPARENT` *(default)* | Specifies that the border pixels of a source image are processed using transparent overscan pixel values. That is, the overscan pixel values will be those of the parent buffer. If they are not available, a mirror type overscan is used instead. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
