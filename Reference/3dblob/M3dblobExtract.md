---
doctype: Reference
module: 3dblob
function: M3dblobExtract
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobExtract"
---

# M3dblobExtract

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

> Populate a container with the specified blob(s).

## Syntax

```c
void M3dblobExtract(
    AIL_ID    SrcContainerBufId,  //in
    AIL_ID    Result3dblobId,     //out
    AIL_INT64 LabelOrIndex,       //in
    AIL_ID    DstContainerBufId,  //in
    AIL_INT64 Options,            //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function copies the specified blob(s) from the source to the destination container. This is useful when you want to calculate a feature or perform an operation that is too complex for [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md), such as fitting a geometry to a specific blob ([`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md)), or creating a mesh for a specific blob ([`M3dimMesh`](../../Reference/3dim/M3dimMesh.md)).

> **Note:** Note that this function never copies an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component to the destination container, even if you specify to extract all blobs.

## Parameters

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud container from which to extract the blobs. This must be the same container previously passed to [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md), and its confidence component must not have been modified since then. If the segmentation was performed using a label image instead, this can be any container as long as the size of the container's range component is the same as the label image.

### `Result3dblobId` *(out, AIL_ID)*

Specifies the identifier of the 3D blob analysis result buffer in which the blob segmentation is stored. The result buffer must have been previously allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md).

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of the blob to extract, or specifies to extract all blobs. This parameter must be set to one of the following values:

*For specifying the blob label or index*

| Value | Description |
| --- | --- |
| `M_BLOB_INDEX` | Specifies the index of the blob. |
| `M_BLOB_LABEL` | Specifies the label of the blob. |
| `M_ALL_BLOBS` | Specifies to extract all blobs. |

### `DstContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the destination point cloud container in which to save the extracted blobs.

### `Options` *(in, AIL_INT64)*

Specifies how to organize the destination point cloud.

*For specifying how to organize the destination point cloud*

| Value | Description |
| --- | --- |
| `M_AUTO` | Chooses the fastest option. This depends on the size and density of the blob(s) relative to the source container. |
| `M_SAME` | Specifies to copy all the source container's components (except any mesh component) into the destination container; no change is made to the size of the components. |
| `M_SHRINK` | Specifies to copy, from source to destination, the smallest region that holds all valid points of the extracted blob(s), in all components (except any mesh component).

Note that the region is a rectangular region within the bands of the 2D data buffers that hold point cloud data. |
| `M_SHRINK_VERTICALLY` | Specifies to copy all the source container's components (except any mesh component) into the destination container, maintaining the source's size in X (that is, the width of its range component), but shrinking the size in Y to the smallest interval that contains all extracted blob points. You can use [`M_SHRINK_VERTICALLY`](../../Reference/3dblob/M3dblobExtract.md) to merge consecutive scans in continuous scanning applications. |
| `M_UNORGANIZED` | Specifies to store results in an unorganized point cloud. Components in the destination container will store results of the extract operation in 1D, unorganized buffers, ordered in the same manner as [`M_PIXEL_X`](../../Reference/3dblob/M3dblobGetResult.md) and [`M_PIXEL_Y`](../../Reference/3dblob/M3dblobGetResult.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies which components will be affected by the operation.

*For specifying whether to apply the operation to all components*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the operation will only affect the core components of a 3D-processable container. These components are listed in the description of [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md).

All other components are copied to the destination container unmodified. |
| `M_APPLY_TO_ALL_COMPONENTS` | Specifies that the operation will affect the core components of a 3D-processable container, as well as any other component (including custom components) that has the same dimension as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md). These components must not have a region of interest (ROI) associated with them. For example, using a component that is an image buffer with an ROI will cause an error.

> **Note:** Note that if a custom component has an associated calibration context, the component's information is extracted, but its calibration context is discarded.

If the resulting operation yields an empty container with 0 valid points, the components with the same dimensions are not created and will not exist in the destination container. |

> **Note:** The confidence score for non-extracted blob points is set to zero.
