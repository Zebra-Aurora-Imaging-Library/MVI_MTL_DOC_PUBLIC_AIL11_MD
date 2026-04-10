---
doctype: Reference
module: 3dgra
function: M3dgraAdd
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraAdd"
---

# M3dgraAdd

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

> Add a point cloud graphic, generated from a point cloud container or depth map image buffer/container, to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraAdd(
    AIL_ID    List3dgraId,            //in
    AIL_INT64 ParentLabel,            //in
    AIL_ID    ContainerOrImageBufId,  //in
    AIL_INT64 ControlFlag             //in
)
```

## Description

This function takes a point cloud container, depth map container, or fully corrected depth map image buffer and generates a point cloud graphic, adding it to the specified 3D graphics list. This allows you, for example, to view the point cloud or depth map on a 3D display. Note that the point cloud container or depth map image buffer/container must be allocated for display ([`M_DISP`](../../Reference/buf/MbufAlloc2d.md)). Unlike [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md), this function allows you to position the point cloud graphic relative to a parent graphic.

Note that if using a point cloud container with a mesh component, a meshed point cloud graphic is added.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the newly generated point cloud graphic. When you add a point cloud graphic to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the specified parent must be the root node.

Unlike other 3D graphics functions that create static 3D graphics, the 3D graphic created by [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) is, by default, linked to the 3D-displayable point cloud container or depth map image buffer/container and updates dynamically. If the linked container or depth map is freed, the corresponding node (and its children) in the graphics list is automatically removed. To create an unlinked point cloud graphic, set [`ControlFlag`](../../Reference/3dgra/M3dgraAdd.md) to [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md); in this case, the 3D graphics list is not updated dynamically.

The 3D graphic created by [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) has its own coordinate system that represents the position and orientation of the 3D graphic with respect to its parent's coordinate system. Initially, the position and orientation of 3D graphic's coordinate system is the identity matrix. This means that the 3D graphic's position and orientation is the same as the position and orientation of its parent's coordinate system. To change the position and orientation of a 3D graphic, use [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

[`M3dimFix`](../../Reference/3dim/M3dimFix.md) with [`M_RANGE_FINITE`](../../Reference/3dim/M3dimFix.md)+[`M_NORMALS_FINITE`](../../Reference/3dim/M3dimFix.md)+[`M_MESH_VALID_POINTS`](../../Reference/3dim/M3dimFix.md) is internally performed for point cloud graphics. Just like automatic compensation, this does not modify the source point cloud.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call functions in this module concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraAdd.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data. Point cloud containers and depth map image buffers/containers are **not thread-safe** and must only be accessed by one thread at a time. You must ensure the source point cloud container or depth map image buffer/container is not modified from another thread while the add is being performed. Note that this does not impact subsequent modifications to the point cloud container or depth map image buffer/container as long as they are modified from one thread at a time.

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

## Parameters

### `List3dgraId` *(in, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the point cloud graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the point cloud graphic.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraAdd.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the point cloud graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `ContainerOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-displayable container or fully corrected depth map image buffer from which to generate the point cloud graphic.

*For specifying the container or depth map image buffer*

| Value | Description |
| --- | --- |
| `buffer identifier` | Specifies the Aurora Imaging Library identifier of a fully corrected depth map image buffer with the [`M_DISP`](../../Reference/buf/MbufAlloc2d.md) attribute. The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer and must be 3D-corrected (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). |
| `Container identifier` | Specifies the Aurora Imaging Library identifier of a 3D-displayable point cloud or depth map container. You can inquire whether a container is 3D-displayable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to add the point cloud graphic to the 3D graphics list.

*For specifying whether the point cloud graphic is linked to the container or depth map image buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to generate a linked point cloud graphic from the specified point cloud container or depth map image buffer/container ([`ContainerOrImageBufId`](../../Reference/3dgra/M3dgraAdd.md)); any subsequent changes to the source point cloud or depth map are reflected in the point cloud graphic. |
| `M_NO_LINK` | Specifies to generate an unlinked point cloud graphic from the specified point cloud container or depth map image buffer/container ([`ContainerOrImageBufId`](../../Reference/3dgra/M3dgraAdd.md)); any subsequent changes to the source point cloud or depth map are not reflected in the point cloud graphic. |

## Return Value

**Type:** `AIL_INT64`

Returns the label of the point cloud graphic added to the 3D graphics list.
