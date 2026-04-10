---
doctype: Reference
module: cal
function: McalCalculateHandEye
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalCalculateHandEye"
---

# McalCalculateHandEye

> Calculate the transformations between elements of a robotics setup that uses a precalibrated camera/3D sensor (required for hand-eye calibration).

## Syntax

```c
void McalCalculateHandEye(
    AIL_ID         CalculateHandEyeContextCalId,  //in
    const AIL_ID * HandMatrix3dgeoIdArrayPtr,     //in
    const AIL_ID * EyeMatrix3dgeoIdArrayPtr,      //in
    AIL_ID         CalculateHandEyeResultCalId,   //out
    AIL_INT        NumPoses,                      //in
    AIL_INT64      ControlFlag                    //in
)
```

## Description

This function calculates the transformations between elements of a robotics setup that uses a precalibrated camera/3D sensor. You can use this function to calculate the transformations required to perform operations such as robotic hand-eye calibration. The function solves the system of matrices (AX=ZB) for the X and Z transformation matrices (note that X and Z do not refer to the X and Z-axes). You must provide both the A and B transformation matrices. Note that transformation matrix A is typically obtained using the robotic software.

The following are example descriptions for the AX=ZB transformation matrices; the matrices do not have to follow the descriptions listed below.

| *[Image: z_TableSpacer20x20.png]* | Transformation matrix[^one] |
| --- | --- |
| **A** [`HandMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md) (Must be provided) | **X** [`CalculateHandEyeResultCalId`](../../Reference/cal/McalCalculateHandEye.md) (Calculated and returned) | **Z** [`CalculateHandEyeResultCalId`](../../Reference/cal/McalCalculateHandEye.md) (Calculated and returned) | **B** [`EyeMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md) (Must be provided) |
| **Eye-in-hand calibration** Camera attached to robotic arm | Transform points from the tool coordinate system to the robot base coordinate system | Transforms points from the camera coordinate system to the tool coordinate system | Transforms points from the reference object coordinate system to the robot base coordinate system | Transform points from the camera coordinate system to the reference object coordinate system |
| **Eye-to-hand calibration** Camera stationary and robotic arm moves | Transforms points from the robot base coordinate system to the tool coordinate system | Transforms points from the camera coordinate system to the robot base coordinate system | Transforms points from the reference object coordinate system to the tool coordinate system | Transforms points from the camera coordinate system to the reference object coordinate system |

[^one]: In this table, the camera can be a 2D camera calibrated using a 3D camera calibration mode or a precalibrated 3D sensor.

This function is especially useful to establish the transformation to apply to points from a precalibrated 3D sensor, and to get their coordinates relative to the robot base coordinate system. Apply the transformation using[`M3dimMatrixTransformList`](../../Reference/3dim/M3dimMatrixTransformList.md). Note that when using a 2D camera, it is more efficient to calibrate the camera in a robotics mode ([`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)) and use [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md) to retrieve their coordinates in the robot base coordinate system.

To accurately calculate the X and Z matrices to calibrate the robotics system, you must pass information about a minimum of 3 poses (each pose consists of an A matrix and a corresponding B matrix) to both [`HandMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md) and [`EyeMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md), respectively. While you must pass a minimum of 3 poses, more poses improves the accuracy of the calibration. For each pose, you must change the position and orientation of the tool holding the camera or grid with respect to the robot base. More specifically, you must rotate the tool along at least two non-parallel axes.

You can retrieve your X or Z transformation matrix using [`McalCopyResult`](../../Reference/cal/McalCopyResult.md) with [`M_MATRIX_X`](../../Reference/cal/McalCopyResult.md) or [`M_MATRIX_Z`](../../Reference/cal/McalCopyResult.md), respectively.

## Parameters

### `CalculateHandEyeContextCalId` *(in, AIL_ID)*

Specifies the identifier of the calculate hand-eye context. The calculate hand-eye context must be allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_CALCULATE_HAND_EYE_CONTEXT`](../../Reference/cal/McalAlloc.md).

### `HandMatrix3dgeoIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the identifiers of the transformation matrices that all represent the A transformation matrix in AX = ZB from the various poses.

### `EyeMatrix3dgeoIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the identifiers of the transformation matrices that all represent the B transformation matrix in AX = ZB from the various poses.

### `CalculateHandEyeResultCalId` *(out, AIL_ID)*

Specifies the identifier of the calculate hand-eye result buffer in which to write the resulting X and Z transformation matrices. This result buffer must be allocated using [`McalAllocResult`](../../Reference/cal/McalAllocResult.md) with [`M_CALCULATE_HAND_EYE_RESULT`](../../Reference/cal/McalAllocResult.md).

### `NumPoses` *(in, AIL_INT)*

Specifies the number of robot poses provided to calculate the transformations between elements of a robotics setup (hand-eye calibration). The number of poses specifies the number of matrices in the arrays.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
