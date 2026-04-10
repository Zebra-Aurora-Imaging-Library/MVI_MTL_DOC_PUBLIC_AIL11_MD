---
doctype: Reference
module: 3dim
function: M3dimFilter
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimFilter"
---

# M3dimFilter

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

> Apply a smoothing filter to a point cloud.

## Syntax

```c
void M3dimFilter(
    AIL_ID    FilterContext3dimId,  //in
    AIL_ID    SrcContainerBufId,    //in
    AIL_ID    DstContainerBufId,    //out
    AIL_INT64 ControlFlag           //in
)
```

## Description

This function applies a smoothing filter to a point cloud. Filtering reduces noise by changing the positions of points, but does not eliminate them. If you want to invalidate outlier points, use [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md).

You can choose the type of smoothing operation to perform, either bilateral or moving least squares (MLS). Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_FILTER_MODE`](../../Reference/3dim/M3dimControl.md) to set the mode. The MLS filter applies smoothing equally to surface and edge points. The bilateral filter, on the other hand, smooths points on faces, leaving edge points much less affected.

The bilateral filter ([`M_SMOOTH_BILATERAL`](../../Reference/3dim/M3dimControl.md)) uses the source point cloud's normal values. If a normals component does not already exist in the source point cloud, [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) will calculate point normals and create a normals component, adding it to the destination container. In this case, use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_USE_SOURCE_NORMALS`](../../Reference/3dim/M3dimControl.md) set to [`M_FALSE`](../../Reference/3dim/M3dimControl.md). Note that this is more efficient than separately calculating a normals component using [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md). Nevertheless, the method used for computing normals could improve filter results. For more information, see [Smoothing a point cloud](../../UserGuide/C35_3D_Image_processing/S06_Smoothing_a_point_cloud_and_invalidating_outlier_points.md).

## Parameters

### `FilterContext3dimId` *(in, AIL_ID)*

Specifies a filter 3D image processing context.

*For specifying the filter 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `Filter 3D image processing context identifier` | Specifies the identifier of a filter 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILTER_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** The function applies the filter control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud container to use as the source. The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). The destination container must not be a child container.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
