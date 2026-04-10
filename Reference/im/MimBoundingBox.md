---
doctype: Reference
module: im
function: MimBoundingBox
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimBoundingBox"
---

# MimBoundingBox

> Compute the bounding box that contains valid pixels according to a given condition.

## Syntax

```c
void MimBoundingBox(
    AIL_ID     SrcImageBufId,            //in
    AIL_INT64  Condition,                //in
    AIL_DOUBLE CondLow,                  //in
    AIL_DOUBLE CondHigh,                 //in
    AIL_INT64  BoxDefinitionType,        //in
    AIL_INT *  TopLeftXPtr,              //out
    AIL_INT *  TopLeftYPtr,              //out
    AIL_INT *  BottomRightXOrLengthPtr,  //out
    AIL_INT *  BottomRightYOrLengthPtr,  //out
    AIL_INT64  ControlFlag               //in
)
```

## Description

This function computes the bounding box that contains valid pixels according to a given condition, based on a lower limit and an upper limit. For example, you can define a condition such that pixels are valid only within a specified value range.

You can specify to return the bounding box in one of two ways: using coordinates of 2 opposite corners, or using coordinates of 1 corner and the width and height (in pixel units).

You can limit this function's results to a region of the source image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be set to the Aurora Imaging Library identifier of a 1-band image buffer.

### `Condition` *(in, AIL_INT64)*

Specifies the condition under which pixels are considered valid.

*For specifying the condition*

| Value | Description |
| --- | --- |
| `M_EQUAL` | Specifies that only pixels with values equal to [`CondLow`](../../Reference/im/MimBoundingBox.md) are considered valid. [`CondHigh`](../../Reference/im/MimBoundingBox.md) must be set to 0. |
| `M_GREATER` | Specifies that only pixels with values greater than [`CondLow`](../../Reference/im/MimBoundingBox.md) are considered valid. [`CondHigh`](../../Reference/im/MimBoundingBox.md) must be set to 0. |
| `M_GREATER_OR_EQUAL` | Specifies that only pixels with values greater than or equal to [`CondLow`](../../Reference/im/MimBoundingBox.md) are considered valid. [`CondHigh`](../../Reference/im/MimBoundingBox.md) must be set to 0. |
| `M_IN_RANGE` | Specifies that pixel values between [`CondLow`](../../Reference/im/MimBoundingBox.md) and [`CondHigh`](../../Reference/im/MimBoundingBox.md), inclusive, are considered valid. |
| `M_LESS` | Specifies that only pixels with values less than [`CondHigh`](../../Reference/im/MimBoundingBox.md) are considered valid. [`CondLow`](../../Reference/im/MimBoundingBox.md) must be set to 0. |
| `M_LESS_OR_EQUAL` | Specifies that only pixels with values less than or equal to [`CondHigh`](../../Reference/im/MimBoundingBox.md) are considered valid. [`CondLow`](../../Reference/im/MimBoundingBox.md) must be set to 0. |
| `M_NOT_EQUAL` | Specifies that only pixels with values not equal to [`CondLow`](../../Reference/im/MimBoundingBox.md) are considered valid. [`CondHigh`](../../Reference/im/MimBoundingBox.md) must be set to 0. |
| `M_OUT_RANGE` | Specifies that pixels with values less than [`CondLow`](../../Reference/im/MimBoundingBox.md), or greater than [`CondHigh`](../../Reference/im/MimBoundingBox.md), are considered valid. |

### `CondLow` *(in, AIL_DOUBLE)*

Specifies the lower limit of the selected condition.

### `CondHigh` *(in, AIL_DOUBLE)*

Specifies the upper limit of the selected condition.

### `BoxDefinitionType` *(in, AIL_INT64)*

Specifies the way in which the bounding box is returned.

*For specifying the way in which the bounding box is returned*

| Value | Description |
| --- | --- |
| `M_BOTH_CORNERS` | Specifies to return the bounding box using the coordinates of its top-left and bottom-right corners. |
| `M_CORNER_AND_DIMENSION` | Specifies to return the bounding box using the coordinates of its top-left corner, and the box's width and height. |

### `TopLeftXPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the X-coordinate of the top-left corner of the bounding box.

### `TopLeftYPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the Y-coordinate of the top-left corner of the bounding box.

### `BottomRightXOrLengthPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the X-coordinate of the bottom-right corner or the width of the bounding box.

### `BottomRightYOrLengthPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the Y-coordinate of the bottom-right corner or the height of the bounding box.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to **M_DEFAULT**.
