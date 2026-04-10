---
doctype: Reference
module: 3dim
function: M3dimStat
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimStat"
---

# M3dimStat

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

> Compute a variety of statistics on a point cloud, depth map, or 3D geometry.

## Syntax

```c
void M3dimStat(
    AIL_ID    StatContext3dimId,                //in
    AIL_ID    AilObjectId,                      //in
    AIL_ID    StatResult3dimOrGeometry3dgeoId,  //out
    AIL_INT64 ControlFlag                       //in
)
```

## Description

This function calculates a variety of statistics for the 3D points in a point cloud, for the real-world coordinates that correspond to pixels in a depth map, or for a 3D geometry. The specified statistics 3D image processing context establishes which statistics to calculate and how to calculate them. If you need to calculate one type of statistic and use default settings, you can use a predefined context; otherwise, use [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) to set up a custom context that you can control using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). For example, to calculate bounding box statistics for a point cloud or depth map, you can first enable the calculation (using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOUNDING_BOX`](../../Reference/3dim/M3dimControl.md) and [`M_ENABLE`](../../Reference/3dim/M3dimControl.md)), and then have the algorithm compute the axis-aligned box that contains most of the points but rejects outliers ([`M_BOUNDING_BOX_ALGORITHM`](../../Reference/3dim/M3dimControl.md) set to [`M_ROBUST`](../../Reference/3dim/M3dimControl.md)). Note that, if you use the default [`M_STAT_CONTEXT_BOUNDING_BOX`](../../Reference/3dim/M3dimStat.md) context, [`M3dimStat`](../../Reference/3dim/M3dimStat.md) computes the bounding box of all valid points. Results are stored in the specified 3D statistics result buffer.

For a 3D geometry, you can calculate bounding box and centroid statistics only.

Note that, when you specify a destination 3D geometry, bounding box and centroid statistics are written directly to the specified 3D geometry object, which establishes the geometry as a 3D box or 3D point, respectively.

After calling [`M3dimStat`](../../Reference/3dim/M3dimStat.md), you can obtain the statistics from the result buffer using [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md).

> **Note:** Note that there must be a sufficient number of valid points in the source container or image buffer for the statistics to be calculated. This means that there must be at least 2 valid points for [`M_STAT_CONTEXT_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimStat.md) calculations, and at least 1 valid point for all other statistics types. In the case of an insufficient number of valid points, the result is typically 0; however, PCA or fixturing matrices return the identity matrix, and PCA axes return an axis-aligned coordinate system with vector coordinates (1,0,0), (0,1,0), (0,0,1). Also note that, for moments, if the specified powers in X, Y, and Z are all 0, the result is 1; otherwise, the result is 0.

## Parameters

### `StatContext3dimId` *(in, AIL_ID)*

Specifies a previously allocated statistics 3D image processing context (used to evaluate multiple statistical calculations), or a predefined statistics 3D image processing context (used to evaluate a single statistical calculation).

*For specifying the statistics context*

| Value | Description |
| --- | --- |
| `M_STAT_CONTEXT_BOUNDING_BOX` | Specifies a predefined statistics 3D image processing context with the [`M_BOUNDING_BOX`](../../Reference/3dim/M3dimControl.md) control type ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md).

For a point cloud or depth map, use this predefined context to calculate bounding box statistics using the default setting ([`M_ALL_POINTS`](../../Reference/3dim/M3dimControl.md)), which includes all valid points; no valid points exist outside the bounding box. To exclude outliers, use a custom context and compute a bounding box that contains most of the valid points instead of all of them, using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOUNDING_BOX_ALGORITHM`](../../Reference/3dim/M3dimControl.md) set to [`M_ROBUST`](../../Reference/3dim/M3dimControl.md).

Note that, if the destination object ([`StatResult3dimOrGeometry3dgeoId`](../../Reference/3dim/M3dimStat.md)) is a 3D geometry, this setting writes the calculated bounding box into the specified 3D geometry object directly. |
| `M_STAT_CONTEXT_CENTROID` | Specifies a predefined statistics 3D image processing context with the [`M_CENTROID`](../../Reference/3dim/M3dimControl.md) control type ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md).

Use this predefined context to calculate centroid statistics. The centroid is the center of mass of a point cloud, depth map, or 3D geometry. Centroid statistics include the X-, Y-, and Z-coordinates of the centroid.

Note that, if the destination object ([`StatResult3dimOrGeometry3dgeoId`](../../Reference/3dim/M3dimStat.md)) is a 3D geometry, this setting writes the calculated centroid into the specified 3D geometry object directly. |
| `M_STAT_CONTEXT_DISTANCE_TO_NEAREST_NEIGHBOR` | Specifies a predefined statistics 3D image processing context with the [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md) control type ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md).

Use this predefined context to calculate distance-to-nearest-neighbor statistics, which include the average distance, the maximum distance, and the minimum distance of 3D points or real-world coordinates to their nearest neighbor.

> **Note:** This predefined context is not available for 3D geometries. |
| `M_STAT_CONTEXT_NUMBER_OF_POINTS` | Specifies a predefined statistics 3D image processing context with the [`M_NUMBER_OF_POINTS`](../../Reference/3dim/M3dimControl.md) control type ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md).

Use this predefined context to calculate number-of-points statistics, which include the total number of points, the number of points that are missing data (invalid points), and the number of valid points.

> **Note:** This predefined context is not available for 3D geometries. |
| `M_STAT_CONTEXT_PCA` | Specifies a predefined statistics 3D image processing context with the [`M_PCA`](../../Reference/3dim/M3dimControl.md) control type ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md).

Use this predefined context to calculate principle component analysis (PCA) statistics.

> **Note:** This predefined context is not available for 3D geometries. |
| `M_STAT_CONTEXT_PCA_NORMALS` | Specifies a predefined statistics 3D image processing context with the [`M_PCA`](../../Reference/3dim/M3dimControl.md) control type set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md), the [`M_PCA_MODE`](../../Reference/3dim/M3dimControl.md) control type set to [`M_ORDINARY`](../../Reference/3dim/M3dimControl.md), and the [`M_COMPONENT_OF_INTEREST`](../../Reference/3dim/M3dimControl.md) control type set to [`M_COMPONENT_NORMALS_AIL`](../../Reference/3dim/M3dimControl.md) ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)).

Use this predefined context to calculate principle component analysis (PCA) statistics on a point cloud's unit normal vectors.

> **Note:** This predefined context is not available for depth maps or 3D geometries. |
| `M_STAT_CONTEXT_SEMI_ORIENTED_BOX` | Specifies a predefined statistics 3D image processing context with the [`M_BOUNDING_BOX`](../../Reference/3dim/M3dimControl.md) control type set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md) and the [`M_BOX_ORIENTATION`](../../Reference/3dim/M3dimControl.md) control type set to [`M_SEMI_ORIENTED`](../../Reference/3dim/M3dimControl.md) ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)).

Use this predefined context to calculate semi-oriented bounding box statistics, whereby the minimum volume bounding box is axis-aligned in Z but not in X and Y. To change the axis with which the semi-oriented bounding box is aligned, use a custom context and specify the axis using [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md). For a point cloud or depth map, specifying this predefined context calculates semi-oriented bounding box statistics using the default algorithm ([`M_ALL_POINTS`](../../Reference/3dim/M3dimControl.md)), which includes all valid points; no valid points exist outside the semi-oriented bounding box. To exclude outliers, use a custom context and compute a semi-oriented bounding box that contains most of the valid points instead of all of them, using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOUNDING_BOX_ALGORITHM`](../../Reference/3dim/M3dimControl.md) set to [`M_ROBUST`](../../Reference/3dim/M3dimControl.md). |
| `M_STAT_CONTEXT_SURFACE_AREA` | Specifies a predefined statistics 3D image processing context with the [`M_SURFACE_AREA`](../../Reference/3dim/M3dimControl.md) control type ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to [`M_ENABLE`](../../Reference/3dim/M3dimControl.md).

Use this predefined context to calculate the surface area of a mesh.

> **Note:** This predefined context is only available for 3D-processable point cloud and depth map containers that have an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component. |
| `Statistics 3D image processing context identifier` | Specifies the identifier of a statistics 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** In this case, use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to enable the statistics type(s) and specify additional control settings. |

### `AilObjectId` *(in, AIL_ID)*

Specifies the identifier of the source container containing a 3D-processable point cloud or depth map, the identifier of the source depth map image buffer, or the identifier of the source 3D geometry object.

*For specifying the source container, image buffer, or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `Source 3D geometry object identifier` | Specifies the identifier of the source 3D geometry object.

The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include box, cylinder, line, point, and sphere. Note that infinite 3D geometries (such as an infinite cylinder, an infinite line, or a plane) are not supported. |
| `Source depth map container identifier` | Specifies the identifier of the source depth map container.

The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)). |
| `Source image buffer identifier` | Specifies the identifier of the source image buffer.

The source image buffer must be a 1-band, 8-, 16-, or 32-bit unsigned buffer, and must be fully corrected (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). The source image buffer can have a region of interest (ROI) associated with it, but it must be a raster region. Using an image buffer with a non-raster type of ROI will cause an error. |
| `Source point cloud container identifier` | Specifies the identifier of the source point cloud container.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)), and must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). |

### `StatResult3dimOrGeometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the statistics 3D image processing result buffer, or the identifier of a 3D geometry object. The result buffer must have been allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/3dim/M3dimAllocResult.md). The 3D geometry object must have been allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
