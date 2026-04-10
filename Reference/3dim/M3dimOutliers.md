---
doctype: Reference
module: 3dim
function: M3dimOutliers
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dim / M3dimOutliers"
---

# M3dimOutliers

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

> Invalidate outlier points in a point cloud or a depth map.

## Syntax

```c
void M3dimOutliers(
    AIL_ID    OutliersContext3dimId,      //in
    AIL_ID    SrcContainerOrImageBufId,   //in
    AIL_ID    Dst1ContainerOrImageBufId,  //out
    AIL_ID    Dst2ImageBufId,             //out
    AIL_INT64 ControlFlag                 //in
)
```

## Description

This function invalidates outlier points in a point cloud or a depth map. An outlier is a point identified as anomalous (for instance, due to noise), typically because it is too distant from neighboring points. Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to set the outlier mode ([`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md)) and other options for the operation.

[`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) sets the confidence of outlier points to zero; the points will still exist but are invalidated.

You can specify one or two destination objects. If specifying two, the first receives a copy of the source with outlier points invalidated; the second receives a mask image in which pixels that correspond to outlier points are set to a non-zero value, while the rest of the pixels are set to zero. If specifying only one destination object, set the other to [`M_NULL`](../../Reference/3dim/M3dimOutliers.md). Depending on the destination parameter that you set, the function will produce either a point cloud (or depth map) with outlier points invalidated, or an outlier mask image.

If you only create a mask image, you can examine the identified outliers before committing to invalidating them in the point cloud or depth map. For example, you can display outliers in a contrasting color and compare different masks created from the same point cloud or depth map. If the outcome is not satisfactory (for example, not enough outliers are identified or some are misidentified), you can adjust the outliers control settings and repeat the operation until the result is acceptable. Once you are satisfied, you can call [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) and specify a destination container that you are confident will have the true outlier points invalidated.

## Parameters

### `OutliersContext3dimId` *(in, AIL_ID)*

Specifies an outliers 3D image processing context.

*For specifying the outliers 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_OUTLIERS_CONTEXT_NUMBER_WITHIN_DISTANCE` | Specifies a predefined outliers 3D image processing context with all outliers context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimControl.md). Use this predefined context to determine outliers based on the number of neighboring points with the automatically calculated neighborhood distance. |
| `M_OUTLIERS_CONTEXT_PROBABILITY` | Specifies a predefined outliers 3D image processing context with all outliers context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimControl.md). Use this predefined context to determine outliers based on the density of neighboring points and a probability calculation. |
| `M_OUTLIERS_CONTEXT_ROBUST_STD_DEVIATION` | Specifies a predefined outliers 3D image processing context with all outliers context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, including [`M_DISTANCE_THRESHOLD_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_ROBUST_STD_DEVIATION`](../../Reference/3dim/M3dimControl.md). Use this predefined context to determine outliers based on the median and mean absolute deviation (MAD) of the local average distance distribution, which considers the average distance of all points to their closest neighbors.

This context uses the default outlier mode ([`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimControl.md)). |
| `M_OUTLIERS_CONTEXT_STD_DEVIATION` | Specifies a predefined outliers 3D image processing context with all outliers context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except [`M_DISTANCE_THRESHOLD_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_STD_DEVIATION`](../../Reference/3dim/M3dimControl.md). Use this predefined context to determine outliers based on the mean and standard deviation of the local average distance distribution, which considers the average distance of all points to their closest neighbors.

This context uses the default outlier mode ([`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimControl.md)). |
| `Outliers 3D image processing context identifier` | Specifies the identifier of an outliers 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_OUTLIERS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** If a previously allocated context is specified, the function applies the outliers control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source point cloud container, depth map container, or depth map image buffer.

### `Dst1ContainerOrImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container or depth map image buffer.

### `Dst2ImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which pixels that correspond to outlier points will be set to a non-zero value, creating an outlier mask image. The rest of the pixels are set to zero, indicating inliers or preexisting invalid points in the source object. The image buffer must have the same dimensions as the buffer specified for [`SrcContainerOrImageBufId`](../../Reference/3dim/M3dimOutliers.md). In addition, the image buffer must be an 8-bit, unsigned, 1-band buffer.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
