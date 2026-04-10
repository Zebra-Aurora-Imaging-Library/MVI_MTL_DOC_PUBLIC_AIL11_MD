---
doctype: Reference
module: cal
function: McalCopyResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalCopyResult"
---

# McalCopyResult

> Copy a group of results from a calculate hand-eye result buffer into a transformation matrix object.

## Syntax

```c
void McalCopyResult(
    AIL_ID    CalculateHandEyeResultCalId,  //in
    AIL_INT64 ResultIndex,                  //in
    AIL_ID    Matrix3dgeoId,                //out
    AIL_INT64 CopyType,                     //in
    AIL_INT64 ControlFlag                   //in
)
```

## Description

This function copies a group of results from a calculate hand-eye result buffer into a transformation matrix object.

You can only copy calculate hand-eye results after calling [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md).

## Parameters

### `CalculateHandEyeResultCalId` *(in, AIL_ID)*

Specifies the identifier of the source calculate hand-eye result buffer from which to copy. The calculate hand-eye result buffer must have been allocated using [`McalAllocResult`](../../Reference/cal/McalAllocResult.md) with [`M_CALCULATE_HAND_EYE_RESULT`](../../Reference/cal/McalAllocResult.md), and must contain the results of a call to [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md).

### `ResultIndex` *(in, AIL_INT64)*

Specifies a pose or an average of all poses from the source calculate hand-eye result buffer that the transformation matrix object will correspond to.

*For specifying the pose*

| Value | Description |
| --- | --- |
| `M_LAST` | Specifies to use the last pose from the source calculate hand-eye result buffer. |
| `M_MEAN` *(default)* | Specifies the use the average of all the poses from the source calculate hand-eye result buffer. |
| `0 <= Value < Num poses` | Specifies to use a specific pose from the source calculate hand-eye result buffer. |

### `Matrix3dgeoId` *(out, AIL_ID)*

Specifies the identifier of a transformation matrix object in which to copy. The matrix object must have been allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The type of result that is copied depends on the copy operation ([`CopyType`](../../Reference/cal/McalCopyResult.md)).

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_MATRIX_X` | Specifies to copy the transformation matrix X into the transformation matrix object. What this transformation matrix can be used for depends on what was passed as matrices to A ([`HandMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md)) and B ([`EyeMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md)) for the system of matrices AX = ZB when [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md)was called. For example, if the matrix that transforms points from the tool coordinate system to the robot base coordinate system was passed as A, and the matrix that transforms points from the camera coordinate system to the reference object coordinate system was passed as B, then matrix X would transform points from the camera coordinate system to the tool coordinate system.

> **Note:** Note that X and Z in this equation do not relate to the X and Z-axes. |
| `M_MATRIX_Z` | Specifies to copy the transformation matrix Z into the transformation matrix object. What this transformation matrix can be used for depends on what was passed as matrices to A ([`HandMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md)) and B ([`EyeMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md)) for the system of matrices AX = ZB when [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md)was called. For example, if the matrix that transforms points from the tool coordinate system to the robot base coordinate system was passed as A, and the matrix that transforms points from the camera coordinate system to the reference object coordinate system was passed as B, then matrix Z would transform points from the reference object coordinate system to the robot base coordinate system.

> **Note:** Note that X and Z in this equation do not relate to the X and Z-axes. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
