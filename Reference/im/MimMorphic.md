---
doctype: Reference
module: im
function: MimMorphic
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimMorphic"
---

# MimMorphic

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

> Perform a morphological transformation using a user-defined or predefined structuring element.

## Syntax

```c
void MimMorphic(
    AIL_ID    SrcContainerOrImageBufId,  //in
    AIL_ID    DstContainerOrImageBufId,  //out
    AIL_ID    StructElemBufId,           //in
    AIL_INT64 Operation,                 //in
    AIL_INT   NbIterationOrArea,         //in
    AIL_INT64 ProcMode                   //in
)
```

## Description

This function performs one of several morphological transformations on the specified source image or depth map container, using a user-defined or predefined structuring element.

If the source and destination are multi-band buffers, the same structuring element is applied to every band of the source buffer.

If the source is a depth map container, invalid pixels are copied unchanged into the destination buffer. Also, for the supported operations, the [`M_GRAYSCALE`](../../Reference/im/MimMorphic.md) processing mode is used.

## Parameters

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer or depth map container.

*For specifying the source image buffer or depth map container identifier*

| Value | Description |
| --- | --- |
| `Source depth map container identifier` | Specifies the identifier of the source depth map container.

The depth map container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

The container must not have a region component. Using a container with a region component will cause an error. |
| `Source image buffer identifier` | Specifies the identifier of the source image buffer.

The image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `DstContainerOrImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer in which to put the result of the operation. If [`SrcContainerOrImageBufId`](../../Reference/im/MimMorphic.md) is an image buffer, this parameter must be given an image buffer identifier. If [`SrcContainerOrImageBufId`](../../Reference/im/MimMorphic.md) is a depth map container, this parameter must be given the identifier of a depth map image buffer or depth map container.

*For specifying the destination image buffer, depth map image buffer, or depth map container identifier*

| Value | Description |
| --- | --- |
| `Destination depth map container identifier` | Specifies the identifier of the destination depth map container.

The depth map container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)), and must not be a child container. |
| `Destination depth map image buffer identifier` | Specifies the identifier of the destination depth map image buffer.

The depth map image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned image buffer with the same dimensions ([`SizeX`](../../Reference/buf/MbufAlloc2d.md) and [`SizeY`](../../Reference/buf/MbufAlloc2d.md)) as the source depth map container's range component. It must be 3D-corrected (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)), and must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |
| `Destination image buffer identifier` | Specifies the identifier of the destination image buffer.

The image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `StructElemBufId` *(in, AIL_ID)*

Specifies the structuring element to use. For all operations except area opening and area closing operations, you must specify the identifier of the structuring element buffer which defines the required structuring element.

*For specifying the structuring element*

| Value | Description |
| --- | --- |
| `M_3X3_CROSS` | Specifies a cross (+) structuring element whose neighborhood size is 3x3 pixels. In this case, there are 5 valid neighborhood pixels; the function ignores the pixels located at the four corners of the neighborhood. |
| `M_3X3_RECT` | Specifies a structuring element whose neighborhood size is 3x3 pixels. In this case, there are 9 valid neighborhood pixels. |
| `Structuring element buffer identifier` | Specifies a custom structuring element to use, allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) attribute. This structuring element should be loaded with structuring-element values, using [`MbufPut`](../../Reference/buf/MbufPut.md). These values can be set to **M_DONT_CARE** to specify that the corresponding source image pixels should not be taken into account during the operation. You can set the overscan type and the center of the structuring element, using [`MbufControl`](../../Reference/buf/MbufControl.md).

For window leveling operations, the structuring element buffer must contain only 0 or **M_DONT_CARE** values. The center structuring element value must not be an **M_DONT_CARE** value.

When the source is a depth map container, the structuring element buffer must contain only 0 or **M_DONT_CARE** values. The center structuring element value must be 0 (to avoid introducing new invalid pixels). For [`M_BOTTOM_HAT`](../../Reference/im/MimMorphic.md), [`M_CLOSE`](../../Reference/im/MimMorphic.md), [`M_OPEN`](../../Reference/im/MimMorphic.md), and [`M_TOP_HAT`](../../Reference/im/MimMorphic.md) operations, the structuring element value at position `_Size_ - _Center_ - 1` must also be 0, since it will be the center of the second operation. |

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform. Supported operations are given in the table below.

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_AREA_CLOSE` | Specifies an area close operation.

> **Note:** Note that this operation is only available when the source is an image buffer. |
| `M_AREA_OPEN` | Specifies an area open operation.

> **Note:** Note that this operation is only available when the source is an image buffer. |
| `M_BOTTOM_HAT` | Specifies a bottom-hat operation. This operation is the equivalent to a close operation ([`M_CLOSE`](../../Reference/im/MimMorphic.md)), followed by a subtraction of the source from the results. |
| `M_CLOSE` | Specifies a close operation. This operation is equivalent to a dilation followed by an erosion. |
| `M_DILATE` | Specifies a dilation operation.

When performing a binary dilation, if any of the structuring element values match the corresponding neighborhood values, the center pixel is set to the maximum value of the buffer (for example, 0xff for an 8-bit buffer); otherwise, it remains unchanged. In effect, binary dilation adds layers to the objects.

When performing a grayscale dilation, this operation adds each structuring element value to the corresponding pixel value in the neighborhood, and then replaces the center pixel of the neighborhood with the maximum value from the resulting neighborhood values.

The pixels that correspond to **M_DONT_CARE** in the structuring element are ignored. |
| `M_ERODE` | Specifies an erosion operation.

When performing a binary erosion, if the structuring element does not match the corresponding neighborhood values exactly, the center pixel is set to zero; otherwise, it remains unchanged. In effect, binary erosion peels off layers of objects.

When performing a grayscale erosion, this operation subtracts each structuring element value from the corresponding pixel value in the neighborhood, and then replaces the center pixel of the neighborhood with the minimum value from the resulting neighborhood values.

The pixels that correspond to **M_DONT_CARE** in the structuring element are ignored. |
| `M_HIT_OR_MISS` | Specifies a hit or miss transformation.

> **Note:** Note that this operation is only available when the source is an image buffer. |
| `M_LEVEL` | Specifies a window leveling operation on a pixel neighborhood basis.

This operation finds the minimum and maximum pixel values in a neighborhood, and then creates a linear mapping such that these map to the minimum and maximum possible values of the image buffer. It then replaces the center pixel of the neighborhood with its corresponding value in the mapping.

If the destination image buffer is a floating-point buffer, each pixel will be set to a value between 0 and 1.

The pixels that correspond to **M_DONT_CARE**in the structuring element are ignored.

> **Note:** Note that this operation is only available when the source is an image buffer. |
| `M_MATCH` | Specifies a matching operation.

> **Note:** Note that this operation is only available when the source is an image buffer. |
| `M_OPEN` | Specifies an open operation. This operation is equivalent to an erosion, followed by a dilation. |
| `M_THICK` | Specifies a thickening operation.

> **Note:** Note that this operation is only available when the source is an image buffer. |
| `M_THIN` | Specifies a thinning operation.

> **Note:** Note that this operation is only available when the source is an image buffer. |
| `M_TOP_HAT` | Specifies a top-hat operation. This operation is the equivalent to an open operation ([`M_OPEN`](../../Reference/im/MimMorphic.md)) followed by a subtraction of the results from the source. |

*For specifying to combine multiple iterations of an operation*

| Value | Description |
| --- | --- |
| `M_ITER_COMBINED` | Specifies to combine multiple iterations of an operation into a larger equivalent structuring element and to perform the combined operation once. This can improve operation performance for multiple iterations if the equivalent structuring element is not too large.

> **Note:** Note that the resulting borders might be different than when not combining the iterations into a single operation. |

### `NbIterationOrArea` *(in, AIL_INT)*

Specifies a value that is dependent on the morphological operation chosen.

### `ProcMode` *(in, AIL_INT64)*

Specifies the processing mode to use. This parameter can be set to one of the following:

*For specifying the processing mode*

| Value | Description |
| --- | --- |
| `M_BINARY` | Treats non-zero pixels as ones (1) during processing. |
| `M_GRAYSCALE` | Uses the source's gray values for processing. The resulting pixels will be grayscale.

> **Note:** You must use this processing mode to perform an [`M_TOP_HAT`](../../Reference/im/MimMorphic.md), [`M_BOTTOM_HAT`](../../Reference/im/MimMorphic.md), or [`M_LEVEL`](../../Reference/im/MimMorphic.md) operation, or when the source is a depth map container. |
