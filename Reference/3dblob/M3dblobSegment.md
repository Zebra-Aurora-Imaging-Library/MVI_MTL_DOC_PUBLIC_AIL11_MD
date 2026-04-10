---
doctype: Reference
module: 3dblob
function: M3dblobSegment
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobSegment"
---

# M3dblobSegment

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

> Calculate and store a blob segmentation from a point cloud or label image.

## Syntax

```c
void M3dblobSegment(
    AIL_ID    SegmentationContext3dblobId,  //in
    AIL_ID    ContainerOrLabelImageBufId,   //in
    AIL_ID    Result3dblobId,               //out
    AIL_INT64 ControlFlag                   //in
)
```

## Description

This function calculates a blob segmentation and stores it in a 3D blob analysis result buffer. The blob segmentation is calculated from the specified point cloud or it is defined from the specified label image.

When you specify a point cloud, the blob segmentation is performed according to the settings of the segmentation 3D blob analysis context. To set up the context, use [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md).

When you specify a label image to define the blob segmentation, the label image is copied to the result buffer. For example, you can specify a label image created using the (2D) Blob Analysis module (specifically, a blob identifier image created using [`MblobLabel`](../../Reference/blob/MblobLabel.md)). Each pixel in the label image is set to a value that will identify corresponding points in a point cloud as belonging to a specific blob, using a label that is unique for each blob. Pixels with a value of zero represent points that are not part of any blob. Note that the location of each pixel in the label image corresponds to the point at the same location in the point cloud container. Also note that invalid points in the point cloud must have a label of zero; otherwise, functions that use the points might try to operate on the invalid points, causing an error.

The blob segmentation defines which points are considered to be in the same blob when [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) is called.

The result buffer will hold the blob segmentation as well as a list of blob labels, to which indices are assigned (in whatever order the blobs happen to be in the list). You can specify to relabel the blobs in consecutive order, using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_RELABEL_CONSECUTIVE`](../../Reference/3dblob/M3dblobControl.md), for either a context (when calling [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md)), or a segmentation result (after calling [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md)).

## Parameters

### `SegmentationContext3dblobId` *(in, AIL_ID)*

Specifies a segmentation 3D blob analysis context.

*For specifying the segmentation 3D blob analysis context identifier*

| Value | Description |
| --- | --- |
| `M_SEGMENTATION_CONTEXT_LABEL_IMAGE` | Specifies a predefined segmentation 3D blob analysis context with all segmentation context control types ([`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)) set to their default, except for [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) which is set to [`M_LABEL_IMAGE`](../../Reference/3dblob/M3dblobControl.md). Use this predefined context to copy an external label image into the 3D blob analysis result buffer. |
| `M_SEGMENTATION_CONTEXT_WHOLE_IMAGE` | Specifies a predefined segmentation 3D blob analysis context with all segmentation context control types ([`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)) set to their default, except for [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) which is set to [`M_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobControl.md). Use this predefined context to count all valid points as belonging to a single blob; any point whose confidence score is 0 will not be included as part of the blob. |
| `Segmentation 3D blob analysis context identifier` | Specifies the identifier of a segmentation 3D blob analysis context, previously allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_SEGMENTATION_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md).

> **Note:** If a previously allocated context is specified, the function applies the segmentation control settings specified using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md). |

### `ContainerOrLabelImageBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud container or image buffer from which to calculate the segmentation. You can specify either a container or a label image, depending on the segmentation context's [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md).

*For specifying the container or label image buffer identifier*

| Value | Description |
| --- | --- |
| `Label image buffer identifier` | Specifies the identifier of a label image buffer.

The label image must be a 1-band 8, 16, or 32-bit unsigned image buffer. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error. Points outside the ROI will not be part of any blob. |
| `Point cloud container identifier` | Specifies the identifier of a point cloud container.

The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). Note that the container must not be a child container on a Distributed Aurora Imaging Library remote system with a parent that contains multiple range or confidence components; otherwise, an error is generated. |

### `Result3dblobId` *(out, AIL_ID)*

Specifies the identifier of the segmentation 3D blob analysis result buffer. The result buffer must have been allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
