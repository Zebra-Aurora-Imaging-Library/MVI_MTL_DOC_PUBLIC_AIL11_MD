---
doctype: Reference
module: 3dim
function: M3dimSample
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimSample"
---

# M3dimSample

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

> Subsample a point cloud to generate a less dense point cloud, sample a meshed point cloud to generate a denser point cloud, or sample a finite 3D geometry to generate a point cloud.

## Syntax

```c
void M3dimSample(
    AIL_ID    SampleContext3dimId,               //in
    AIL_ID    SrcContainerBufOrGeometry3dgeoId,  //in
    AIL_ID    DstContainerBufId,                 //out
    AIL_INT64 ControlFlag                        //in
)
```

## Description

This function reduces the number of points in a point cloud (subsampling), increases the number of points in a meshed point cloud (surface sampling), or creates a point cloud from a 3D geometry.

When subsampling a point cloud, the function subsamples the source container's point cloud ([`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components), and places the resulting less dense point cloud into the destination container. If an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container, it is subsampled and the new normals component is added to the destination container. Typically, the source container's [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) component (if it exists) is also subsampled. Note that subsampling always invalidates the mesh; regardless of the existence of a mesh structure ([`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component) associated with the source, the destination point cloud will not have a mesh structure.

You can choose the type of subsampling operation to perform, using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md). The grid operation ([`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md)) uses a lattice structure with a specified cell size to divide points into subgroups and selects a single point per cell. You can use [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) to calculate the lattice required to retain a particular fraction of points from the source point cloud. You can copy lattice results into a subsample 3D image processing context, using [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimCopyResult.md). When you call [`M3dimSample`](../../Reference/3dim/M3dimSample.md) with a subsample 3D image processing context containing lattice results, a grid operation is performed using the cell sizes of the lattice results ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_CELL_SIZE_...`](../../Reference/3dim/M3dimGetResult.md)).

When surface sampling a point cloud, the function samples the surface of a meshed point cloud, and places a denser point cloud into the destination container. New [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components are generated for the destination container, as well as a new [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component (regardless of whether an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container). In addition, if the source container has an [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) component, destination reflectance values are interpolated for a destination [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) component.

You can choose the mode of the surface sampling operation, using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SAMPLE_MODE`](../../Reference/3dim/M3dimControl.md). When using the grid mode ([`M_SURFACE_GRID`](../../Reference/3dim/M3dimControl.md)), regardless of the existence of a mesh structure ([`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component) associated with the source, the destination point cloud will not have a mesh structure; surface sampling using the grid mode invalidates the mesh. If an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the destination container, it is deleted. When using the tessellation mode ([`M_TESSELLATION`](../../Reference/3dim/M3dimControl.md)), the source container's [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) is surface sampled and the new mesh component is added to the destination container.

When surface sampling a finite 3D geometry, the function samples the geometry's surface, and generates the components [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md). An [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component is also generated for box, cylinder, and sphere 3D geometries, but not for a point or line. Note that infinite 3D geometries (such as an infinite cylinder, line, or plane) are not supported.

Note that, if the source container has both [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md) components, the reflectance component is sampled or subsampled and the intensity component is copied to the destination container unmodified, unless the reflectance component is not in the right format. In this case, the intensity component is subsampled or surface sampled, with the resulting [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md) component added to the destination container. If only 1 component of these types exist (reflectance or intensity), that component is subsampled or surface sampled.

> **Note:** Note that by default, any component in the source container that is not listed above is copied to the destination unmodified, when subsampling or surface sampling a point cloud. For subsampling, you can use [`M_APPLY_TO_ALL_COMPONENTS`](../../Reference/3dim/M3dimSample.md) to apply the operation to all other components in the container, including custom components. The components must have the same dimensions as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md); otherwise, they will be copied to the destination unmodified.

## Parameters

### `SampleContext3dimId` *(in, AIL_ID)*

Specifies a subsample or surface sample 3D image processing context.

*For specifying the subsample or surface sample 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `Subsample 3D image processing context identifier` | Specifies the identifier of a subsample 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** The function applies the subsample control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |
| `Surface sample 3D image processing context identifier` | Specifies the identifier of a surface sample 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SURFACE_SAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** The function applies the surface sample control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the source container containing a 3D-processable point cloud, or specifies the identifier of the source 3D geometry object.

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container in which to place the generated point cloud. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

### `ControlFlag` *(in, AIL_INT64)*

Specifies which components will be affected by the operation.

*For specifying whether to apply subsampling to all components*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the operation will only affect the core components of a 3D-processable container. These core components are listed in the description of [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md).

All other components are copied to the destination container unmodified. |
| `M_APPLY_TO_ALL_COMPONENTS` | Specifies that the operation will affect the core components of a 3D-processable container, as well as any other component (including custom components) that has the same dimension as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md). These components must not have a region of interest (ROI) associated with them. For example, using a component that is an image buffer with an ROI will cause an error.

> **Note:** Note that if a custom component has an associated calibration context, the component is subsampled, but its context is discarded.

If the resulting operation yields an empty container with 0 valid points, the components with the same dimensions are not created and will not exist in the destination container.

> **Note:** This option is only available for a subsample 3D image processing context. |
