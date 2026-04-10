---
doctype: Reference
module: 3dreg
function: M3dregCalculate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregCalculate"
---

# M3dregCalculate

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

> Perform the registration operation on the given point clouds and write the resulting transformation matrices into the pairwise 3D registration result buffer.

## Syntax

```c
void M3dregCalculate(
    AIL_ID         Context3dregId,          //in
    const AIL_ID * ContainerBufIdArrayPtr,  //in
    AIL_INT        NumContainers,           //in
    AIL_ID         Result3dregId,           //out
    AIL_INT64      ControlFlag              //in
)
```

## Description

This function calculates the optimal registration (alignment) of the overlapping region shared between each point cloud and its reference point cloud, and calculates the transformation needed to achieve this registration. Once the transformation that maps each point cloud's working coordinate system onto its reference point cloud's working coordinate system is calculated, all point clouds can be mapped onto the global coordinate system.

Every point cloud is associated with a registration element in the 3D registration context, and that registration element provides the rough location of the point cloud relative to another point cloud (the reference point cloud), or in the global coordinate system (for example, the third container in the array is associated with the third registration element).

This function takes points in one point cloud, pairs them with points in the other point cloud, and tries to minimize the root-mean-square (RMS) error of the distance between all the paired points with each iteration of the registration operation. The iterations continue, trying to reduce the RMS error in each iteration, until at least one stop condition occurs. You can specify the stop conditions using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_MAX_ITERATIONS`](../../Reference/3dreg/M3dregControl.md), [`M_RMS_ERROR_THRESHOLD`](../../Reference/3dreg/M3dregControl.md), and [`M_RMS_ERROR_RELATIVE_THRESHOLD`](../../Reference/3dreg/M3dregControl.md). You can also use [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_STOP_CALCULATE`](../../Reference/3dreg/M3dregControl.md) to stop the execution of [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md).

> **Note:** Note that, by default, the pairwise comparison for the registration operation uses the [`M_POINT_TO_PLANE`](../../Reference/3dreg/M3dregControl.md) error minimization metric, which requires the normal vector of each point. If the source point clouds do not have normals components, these components are temporarily generated using default settings ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)). If you require the [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md) components for future operations, or you want to use non-default settings for their generation, you can pass the point clouds to [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md) before calling [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md).

During the first iteration of the registration operation, a point cloud is transformed to its rough location. The rough location is specified using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md), and the preregistration mode is set using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md).

Similar to the pairwise 3D registration context, the pairwise 3D registration result buffer contains registration result elements, each of which stores the results of the registration operation for each point cloud. Each point cloud in the array of containers is associated with the registration result element with the same index. Results include the calculated transformation matrix which aligns the working coordinate system of a point cloud to its reference (retrieved using [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) with [`M_REGISTRATION_MATRIX`](../../Reference/3dreg/M3dregCopyResult.md)), as well as status information (retrieved using [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_STATUS`](../../Reference/3dreg/M3dregGetResult.md)).

## Parameters

### `Context3dregId` *(in, AIL_ID)*

Specifies the identifier of the pairwise 3D registration context to use for the registration operation. The context must have been previously allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md).

### `ContainerBufIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the identifiers of the point cloud containers. The point clouds must be 3D-processable; use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) to ensure that the containers contain 3D-processable point clouds. The number of containers supplied in the array should be less than or equal to the number of registration elements in the registration context.

### `NumContainers` *(in, AIL_INT)*

Specifies the size of the array passed to [`ContainerBufIdArrayPtr`](../../Reference/3dreg/M3dregCalculate.md).

*For specifying the number of containers in the array*

| Value | Description |
| --- | --- |
| `2 <= Value < M3dregInquire(ParamId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the size of the array. |

### `Result3dregId` *(out, AIL_ID)*

Specifies the pairwise 3D registration result buffer identifier in which to store results. The result buffer must have been previously allocated using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
