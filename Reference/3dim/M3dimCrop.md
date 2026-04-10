---
doctype: Reference
module: 3dim
function: M3dimCrop
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimCrop"
---

# M3dimCrop

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

> Crop points in a point cloud based on a specified 3D geometry or mask image buffer.

## Syntax

```c
void M3dimCrop(
    AIL_ID    SrcContainerBufId,          //in
    AIL_ID    DstContainerBufId,          //out
    AIL_ID    ImageBufOrGeometry3dgeoId,  //in
    AIL_ID    Matrix3dgeoId,              //in
    AIL_INT64 OrganizationType,           //in
    AIL_INT64 ControlFlag                 //in
)
```

## Description

This function crops 3D points in the source container's point cloud, based on a specified 3D geometry mask or image buffer. You can optionally specify a transformation matrix to fixture the source point cloud before the cropping operation.

Source components are copied into the destination container where the destination's confidence component determines a point's viability after the crop operation. Specifically, rejected points (those cropped out) are assigned a 0 confidence value in the destination confidence component. Corresponding points in the destination range component are considered cropped out.

Note that normals and reflectance (or intensity) components are not required, but the crop operation is applied to these components if they are present in the source container.

If the crop is based on a 3D geometry, the default operation keeps points located inside the specified geometry, rejecting all others.

If the crop is based on an image buffer, the mask is applied differently based on the type of image buffer:

- When using an uncalibrated image buffer, points are mapped to the same location in the mask image buffer as the point's location in the range component of its point cloud container.
- When using a uniformly calibrated image buffer, points are projected orthogonally onto the mask.
- When using a 3D-based calibrated image buffer, points are projected onto the mask following the frustum of the camera's view, as represented by the associated camera calibration information.

Points are kept only if they correspond to non-zero pixels in the mask image buffer; any points corresponding to zero pixels are rejected. Note that you can specify to invert the crop operation (for example, to keep points located outside, instead of inside, the specified geometry). For more information on how different image buffers can affect how the crop is performed, see [Cropping using a mask image](../../UserGuide/C35_3D_Image_processing/S05_Cropping_or_masking_points.md).

You can specify how the destination container is organized (for example, as an unorganized point cloud).

If a mesh component exists in the source point cloud container, it is also affected by the crop operation. Any cropped point that correspond to the vertex of a triangular face results in that face's removal. This can result in a hole in the mesh.

## Parameters

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the source container containing a 3D-processable point cloud. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container in which to store the resulting point cloud. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

### `ImageBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the 3D geometry object or image buffer used to crop the points.

*For specifying the mask*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane.

Points are kept above this plane, unless [`M_INVERSE`](../../Reference/3dim/M3dimCrop.md) is specified; see the [`ControlFlag`](../../Reference/3dim/M3dimCrop.md) parameter. |
| `3D geometry object identifier` | Specifies the 3D geometry object identifier.

The 3D geometry object must have been previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include box, cylinder, plane, and sphere. |
| `Image buffer identifier` | Specifies the mask image buffer. The mask must be a 1-band, 8-bit, 16-bit or 32-bit unsigned image buffer, and must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. You can use a calibrated or uncalibrated image buffer.

When cropping using an uncalibrated image buffer, the point cloud's points are mapped to the same location in the mask image buffer as the point's location in the range component of its point cloud container. A point is not kept if it corresponds to a zero pixel in the mask. The image buffer's size in X and Y must be the same as the source container's range component. To inquire the range component's size, use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md), [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md), and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md).

When cropping using a calibrated mask, the point cloud's points are projected onto the mask image. A point is not kept if it is projected onto a zero pixel in the mask. The projection is orthogonal for a uniformly calibrated mask. That is, world coordinate values in the point cloud correspond linearly to pixel positions in the 2D mask image onto which the point cloud is projected. If a calibrated mask image is of type [`M_3D_CALIBRATION`](../../Reference/cal/McalInquire.md), point projection is not orthogonal, but instead follows the frustum of the camera's view, as represented by the associated camera calibration information.

To keep points that correspond to zero pixels and crop out the others, set the [`ControlFlag`](../../Reference/3dim/M3dimCrop.md) parameter to [`M_INVERSE`](../../Reference/3dim/M3dimCrop.md). |

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the optional transformation matrix used to fixture the source container before applying the cropping criteria.

*For specifying the identifier of a 3D geometry transformation matrix*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to ignore this parameter. |
| `M_IDENTITY_MATRIX` | Specifies to use an identity matrix. This means that the source point cloud will not be transformed or rotated in any way before the cropping operation. |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The transformation matrix must be any valid affine transformation matrix, where the last row is (0,0,0,1). That is, if you call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_AFFINE`](../../Reference/3dgeo/M3dgeoInquire.md), the function returns [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md). |

### `OrganizationType` *(in, AIL_INT64)*

Specifies how to organize the resulting point cloud.

*For specifying the organization type*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_SAME`](../../Reference/3dim/M3dimCrop.md). |
| `M_SAME` | Specifies to copy all the source container's components into the destination container; points set to 0 in the destination confidence component indicate cropped points. |
| `M_SHRINK` | Specifies to copy, from source to destination, the smallest region that holds all valid points (remaining after the crop operation) in all components; points set to 0 in the destination confidence component indicate cropped points.

Note that the region is a rectangular region within the bands of the 2D data buffers that hold point cloud data. |
| `M_UNORGANIZED` | Specifies to store results in an unorganized point cloud. Components in the destination container will store results of the crop operation in 1D, unorganized buffers. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies which points to keep, relative to the cropping object set with [`ImageBufOrGeometry3dgeoId`](../../Reference/3dim/M3dimCrop.md).

*For specifying which points to keep*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to keep points that are inside the specified 3D geometry object, above the specified plane, or not set to 0 in the specified mask, depending on the cropping object set with [`ImageBufOrGeometry3dgeoId`](../../Reference/3dim/M3dimCrop.md).

> **Note:** Note that, for a plane geometry, above is in the direction of the plane normal vector. To inquire this vector, use [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_NORMAL_...`](../../Reference/3dgeo/M3dgeoInquire.md). |
| `M_INVERSE` | Specifies to keep points that are outside the specified 3D geometry object, below the specified plane, or set to 0 in the specified mask, depending on the cropping object set with [`ImageBufOrGeometry3dgeoId`](../../Reference/3dim/M3dimCrop.md).

> **Note:** Note that, for a plane geometry, below is in the opposite direction of the plane normal vector. To inquire this vector, use [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_NORMAL_...`](../../Reference/3dgeo/M3dgeoInquire.md). |

*For specifying to apply the cropping operation to all components*

| Value | Description |
| --- | --- |
| `M_APPLY_TO_ALL_COMPONENTS` | Specifies that the operation will affect the core components of a 3D-processable container, as well as any other component (including custom components) that has the same dimension as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md). These components must not have a region of interest (ROI) associated with them. For example, using a component that is an image buffer with an ROI will cause an error.

> **Note:** Note that if a custom component has an associated calibration context, the component's information is cropped, but its calibration context is ignored and discarded.

If the resulting operation yields an empty container with 0 valid points, the components with the same dimensions are not created and will not exist in the destination container. |
