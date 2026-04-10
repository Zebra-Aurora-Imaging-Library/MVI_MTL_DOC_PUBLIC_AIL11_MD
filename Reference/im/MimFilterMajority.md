---
doctype: Reference
module: im
function: MimFilterMajority
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimFilterMajority"
---

# MimFilterMajority

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

> Replace the value of each pixel with the value that occurs most often in its neighborhood.

## Syntax

```c
void MimFilterMajority(
    AIL_ID    MajorityFilterContextImId,  //in
    AIL_ID    SrcImageBufId,              //in
    AIL_ID    DstImageBufId,              //out
    AIL_ID    StructElemBufId,            //in
    AIL_INT64 ControlFlag                 //in
)
```

## Description

This function performs a majority filter operation on the specified source image, using a custom or predefined structuring element. The structuring element defines a neighborhood around each pixel of the source image buffer, and the value that appears most often in the neighborhood replaces the value of the central pixel.

When using a custom structuring element, you can specify weights for the pixels in the neighborhood. The weights define the number of times to count the value of the corresponding pixel when establishing the frequency of the different pixel values. If the weights in the custom structuring element buffer are not uniform, then certain neighborhood pixels will have more influence during the majority filter operation. For example, if you set the central value to 2, and all others to 1, then the central pixel's value will be counted twice during the majority filter operation. When using a custom majority filter image processing context, instead of passing a structuring element, you associate a weight image with the context ([`MimControl`](../../Reference/im/MimControl.md)with[`M_WEIGHT_IMAGE`](../../Reference/im/MimControl.md)). It is used similar to a structuring element, but allows you to save the weights with the context when calling [`MimSave`](../../Reference/im/MimSave.md) or [`MimStream`](../../Reference/im/MimStream.md).

By default, if two or more pixel values tie for the greatest frequency, the smallest value present among the tied values is used. If you are using a custom context, you can use [`MimControl`](../../Reference/im/MimControl.md) with [`M_TIE_BREAKER_MODE`](../../Reference/im/MimControl.md) to change the default behavior. If you have specified weights for the pixels in your neighborhood, it is possible that two pixel values are tied despite not occurring with equal frequency in the neighborhood. In this case, you can chose to break the tie based on the non-weighted frequency of the values in the source image rather than their values, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_USE_FREQUENCY_TIE_BREAKER`](../../Reference/im/MimControl.md).

If the source and destination are multi-band buffers, the same structuring element (neighborhood and weights) is applied to every band of the source buffer.

Once the majority filter image processing context is configured, preprocess the context by calling [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) with [`M_PREPROCESS`](../../Reference/im/MimFilterMajority.md). If the preprocess operation is not done explicitly (using [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md)), it will be done when [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) is first called. When preprocessing explicitly, the source and destination image buffers can be set to [`M_NULL`](../../Reference/im/MimFilterMajority.md). If, however, the source or destination image buffer is provided, it will be used in the preprocess operation, to better optimize future calls.

## Parameters

### `MajorityFilterContextImId` *(in, AIL_ID)*

Specifies the identifier of the majority filter image processing context.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `StructElemBufId` *(in, AIL_ID)*

Specifies the identifier of the structuring element that defines the neighborhood around each pixel, and, optionally, the weights of the pixels in the neighborhood.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to preprocess a custom image processing context or to execute the filtering operation.

## Parameter Associations

### For specifying the settings of the majority filter operation

---

### `M_DEFAULT`

Specifies to use a predefined majority filter image processing context that replaces the value of each pixel with the most common value in its neighborhood, as defined by the structuring element. You can optionally specify weights in the structuring element.  When using a predefined image processing context, you must set [`ControlFlag`](../../Reference/im/MimFilterMajority.md) to [`M_DEFAULT`](../../Reference/im/MimFilterMajority.md).

| Value | Description |
| --- | --- |
| `M_3X3_CROSS` | Specifies a cross (+) structuring element whose neighborhood size is 3x3 pixels. In this case, there are 5 valid neighborhood pixels; the function ignores the pixels located at the four corners of the neighborhood. |
| `M_3X3_RECT` | Specifies a structuring element whose neighborhood size is 3x3 pixels. In this case, there are 9 valid neighborhood pixels; the function does not ignore any pixels in the neighborhood. |
| `M_5X5_RECT` | Specifies a structuring element whose neighborhood size is 5x5 pixels. In this case, there are 25 valid neighborhood pixels; the function does not ignore any pixels in the neighborhood. |
| `Custom structuring element identifier` | Specifies to define the neighborhood using a custom structuring element. You must first allocate this structuring element, using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md). You can use [`MbufPut2d`](../../Reference/buf/MbufPut2d.md)to populate your custom structuring element buffer with structuring element values. Note that the custom structuring element must not be a floating-point buffer.  The structuring element defines the size and the weights of the neighborhood. If the weights in the custom structuring element buffer are not uniform, then certain neighborhood pixels will have more influence during the majority filter operation. For example, if you set the central value to 2, and all others to 1, then the central pixel's value will be counted twice during the majority filter operation. |

---

### `Custom majority filter image processing context identifier`

Specifies a custom majority filter image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_FILTER_MAJORITY_CONTEXT`](../../Reference/im/MimAlloc.md).  When using a custom majority filter image processing context, you must set [`StructElemBufId`](../../Reference/im/MimFilterMajority.md) to [`M_NULL`](../../Reference/im/MimFilterMajority.md). To specify the neighborhood (with weights, if applicable), you must pass the identifier of an image buffer to [`MimControl`](../../Reference/im/MimControl.md)with [`M_WEIGHT_IMAGE`](../../Reference/im/MimControl.md) prior to calling this function.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to execute the majority filter operation. If you have not yet preprocessed your custom image processing context, it will be preprocessed upon the first call to [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md). |
| `M_PREPROCESS` | Specifies to preprocess your custom image processing context. Preprocessing the context can speed up subsequent calls to [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md). |
