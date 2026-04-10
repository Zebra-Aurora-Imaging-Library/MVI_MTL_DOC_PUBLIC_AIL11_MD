---
doctype: Reference
module: 3dmet
function: M3dmetDistanceEx
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetDistanceEx"
---

# M3dmetDistanceEx

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

> Calculates the distance between a point cloud or depth map, and a point cloud, depth map, or 3D geometry object, and allows you to store results in a result buffer.

## Syntax

```c
void M3dmetDistanceEx(
    AIL_ID    DistanceContext3dmetId,        //in
    AIL_ID    SrcContainerOrImageBufId,      //in
    AIL_ID    RefAilObjectId,                //in
    AIL_ID    Result3dmetIdOrDstImageBufId,  //out
    AIL_INT64 ControlFlag                    //in
)
```

## Description

This function calculates various distance measurements between a point cloud or depth map, and a point cloud, depth map, or 3D geometry, and allows you to store results in a result buffer.

## Parameters

### `DistanceContext3dmetId` *(in, AIL_ID)*

Specifies the distance 3D metrology context for the distance computation.

*For specifying the distance 3D metrology context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default distance 3D metrology context of the current Aurora Imaging Library application.

> **Note:** The distance operation will use default values for all distance control types listed in [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). |
| `M_DISTANCE_CONTEXT_DIST_TO_NEIGHBOR` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the distance between the points of the point cloud or depth map, and their nearest-neighbor points of the reference point cloud or depth map. |
| `M_DISTANCE_CONTEXT_DIST_TO_NEIGHBOR_IN_DIRECTION_NORMAL` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md), and[`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DIRECTION_NORMAL`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the distance between the points of the source point cloud or depth map, and their nearest-neighbor point, in the direction of the source point's normal, in the reference object. |
| `M_DISTANCE_CONTEXT_DIST_TO_REF` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DISTANCE_TO_REFERENCE`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the distance between the points of the point cloud or depth map, and the reference object. |
| `M_DISTANCE_CONTEXT_IN_DIRECTION_NORMAL` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DIRECTION_NORMAL`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the distance between the points of the source point cloud, and the surface of the reference object in the normal direction of the source points. |
| `M_DISTANCE_CONTEXT_IN_DIRECTION_Z` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DIRECTION_Z`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the distance between the points of the point cloud or depth map, and their closest points the reference object in the Z-direction. |
| `M_DISTANCE_CONTEXT_MANHATTAN_DIST` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_MANHATTAN`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the distance between the points of the point cloud or depth map, and the points of the reference object using the Manhattan distance. |
| `M_DISTANCE_CONTEXT_SIGNED` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the signed distance between the points of the point cloud or depth map, and their closest point in the reference object. |
| `M_DISTANCE_CONTEXT_SIGNED_DIST_IN_DIRECTION_NORMAL` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md), and[`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DIRECTION_NORMAL`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the signed distance between the points of the point cloud, and their closest point in the reference object in the normal direction of the source points. |
| `M_DISTANCE_CONTEXT_SIGNED_DIST_IN_DIRECTION_Z` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md), and[`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DIRECTION_Z`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the signed distance between the points of the point cloud or depth map, and their closest point in the reference object in the Z-direction. |
| `M_DISTANCE_CONTEXT_SQUARED` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_SQUARED`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the squared distance between the points of the point cloud or depth map, and their closest point in the reference object. |
| `M_DISTANCE_CONTEXT_SQUARED_DIST_TO_REF` | Specifies a predefined distance 3D metrology context with all distance context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_SQUARED`](../../Reference/3dmet/M3dmetControl.md), and [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DISTANCE_TO_REFERENCE`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the squared distance between the points of the point cloud or depth map, and their closest point in the reference object. |
| `Distance 3D metrology context identifier` | Specifies the identifier of a distance 3D metrology context, previously allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DISTANCE_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md).

> **Note:** If a previously allocated context is specified, the function applies the distance control settings specified using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). |

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the source point cloud container, depth map container, or depth map image buffer.

*For specifying the point cloud or depth map*

| Value | Description |
| --- | --- |
| `Depth map container identifier` | Specifies the identifier of a depth map container.

The container must store data in a 3D-processable depth map format. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map.

The container must not have a region component. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer must not have a region of interest (ROI) associated with it. |
| `Point cloud container identifier` | Specifies the identifier of a point cloud container.

The container must be 3D-processable. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable point cloud. |

### `RefAilObjectId` *(in, AIL_ID)*

Specifies the reference object with respect to which distances will be measured. The reference object specified depends on the pairing type specified using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md).

*For specifying the reference Aurora Imaging Library object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. The XY-plane can be used as the reference object when the pairing type corresponds to [`M_DISTANCE_TO_REFERENCE`](../../Reference/3dmet/M3dmetControl.md) or [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md). |
| `3D geometry object identifier` | Specifies the identifier of a reference 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include box, cylinder, line, plane, point, and sphere. A 3D geometry object can be used as the reference object when the pairing type corresponds to [`M_DISTANCE_TO_REFERENCE`](../../Reference/3dmet/M3dmetControl.md) or [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md). In the latter case, the 3D geometry cannot be a point geometry. |
| `Depth map container identifier` | Specifies the identifier of a depth map container. A depth map container can be used as the reference object when the pairing type corresponds to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md).

The container must store data in a 3D-processable depth map format. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map.

The container must not have a region component. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer. A depth map image buffer can be used as the reference object when the pairing type corresponds to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md).

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer is allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md), and must not have a region of interest (ROI) associated with it. |
| `Point cloud container identifier` | Specifies the identifier of a point cloud container. A point cloud container can be used as the reference object when the pairing type corresponds to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md) or [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md). In the latter case, the 3D geometry cannot be a point geometry.

The container must be 3D-processable. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable point cloud. The container must also have at least a range and a confidence component. |

### `Result3dmetIdOrDstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the container, image buffer, or a calculate 3D metrology result buffer in which to store the calculated distances.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
