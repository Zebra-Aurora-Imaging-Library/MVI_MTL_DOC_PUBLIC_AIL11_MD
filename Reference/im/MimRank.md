---
doctype: Reference
module: im
function: MimRank
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimRank"
---

# MimRank

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

> Perform a rank filter on the pixels in an image.

## Syntax

```c
void MimRank(
    AIL_ID    SrcImageBufId,    //in
    AIL_ID    DstImageBufId,    //out
    AIL_ID    StructElemBufId,  //in
    AIL_INT   Rank,             //in
    AIL_INT64 ProcMode          //in
)
```

## Description

This function performs a rank filter operation on the specified source buffer. It replaces each pixel with the pixel in its neighborhood whose value is the specified _rankth_ value relative to others. It uses the specified structuring element as a mask. This mask determines the neighborhood size and which pixels in the neighborhood to ignore.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the resulting image. This parameter must be given an image buffer identifier.

### `StructElemBufId` *(in, AIL_ID)*

Specifies the identifier of the structuring element buffer to use as a mask. You can either define a custom structuring element as a mask or use a predefined mask buffer.

*For the structuring element buffer*

| Value | Description |
| --- | --- |
| `M_3X3_CROSS` | Applies a cross (+) mask and sets the neighborhood size to 3x3 pixels. In this case, there are 5 valid neighborhood pixels; the cross mask makes the function ignore the pixels located at the four corners of the neighborhood. |
| `M_3X3_RECT` | Applies no mask and sets the neighborhood size to 3x3 pixels. In this case, there are 9 valid neighborhood pixels. |
| `M_5X5_RECT` | Applies no mask and sets the neighborhood size to 5x5 pixels. In this case, there are 25 valid neighborhood pixels. |
| `Structuring element buffer identifier` | Applies a mask specified by the custom structuring element, having been previously allocated with [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) attribute. This structuring element is to be loaded with structuring-element values, using [`MbufPut`](../../Reference/buf/MbufPut.md). The structuring element buffer dimensions are used as the neighborhood size. Values in the structuring element set to **M_DONT_CARE** represent neighborhood pixels not to be considered in the ranking operation. All other values in the structuring element must be set to 1, representing neighborhood pixels to be considered in the ranking operation. |

*For specifying the overscan setting*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_DISABLE` | Specifies to disable overscanning for this operation. You should disable overscanning to accelerate the operation for applications in which the overscan data is not important in the resulting buffer. |
| `M_OVERSCAN_ENABLE` *(default)* | Specifies to enable overscanning for this operation. The overscan uses pixels from the ancestor of the source buffer. If the source buffer is not a child buffer, or if the point falls outside the ancestor buffer, a mirror type overscan is used. A mirror overscan specifies that the overscan pixels will be a mirror copy of the source buffer's border pixels. |
| `M_OVERSCAN_FAST` | Specifies that Aurora Imaging Library will automatically select the overscan that optimizes speed, according to the specified operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.

Note that when using [`M_OVERSCAN_FAST`](../../Reference/im/MimWarp.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |

### `Rank` *(in, AIL_INT)*

Specifies which of the pixel values to select after valid neighborhood values are sorted in increasing order; valid neighborhood pixel values are those that are not masked-out.

*For specifying which pixel to select*

| Value | Description |
| --- | --- |
| `M_MEDIAN` | Specifies that [`MimRank`](../../Reference/im/MimRank.md) performs a median filter (that is, each pixel is replaced with the median neighborhood value). |
| `0 < Value <= number of valid neighborhood pixels` | Specifies the rank of the pixel value to select. If the number of valid pixels is less than the given rank, the number of valid pixels is used as the rank. |

### `ProcMode` *(in, AIL_INT64)*

Specifies the processing mode to use. This parameter can be set to one of the following:

*For specifying the processing mode to use*

| Value | Description |
| --- | --- |
| `M_BINARY` | Treats non-zero pixels as ones (1) during processing. The resulting non-zero pixels will have all bits set to one. |
| `M_GRAYSCALE` | Uses the source image's gray values for processing. The resulting buffer will also contain gray values. Performance between this and [`M_GRAYSCALE_BIG_STRUCT_ELEMENT`](../../Reference/im/MimRank.md) should be analyzed on your own applications. |
| `M_GRAYSCALE_BIG_STRUCT_ELEMENT` | Uses the source image's gray values for processing. The resulting buffer will also contain gray values. This processing mode is specially designed for large structuring elements. The source image must be an 8-bit, unsigned buffer. Performance between this and [`M_GRAYSCALE`](../../Reference/im/MimRank.md) should be analyzed on your own applications. |
