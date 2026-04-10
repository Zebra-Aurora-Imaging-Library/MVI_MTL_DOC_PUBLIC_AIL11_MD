---
doctype: Reference
module: im
function: MimGet
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimGet"
---

# MimGet

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

> Gets arrays of values from an image processing context.

## Syntax

```c
AIL_INT MimGet(
    AIL_ID    ContextImId,  //in
    AIL_INT64 GetType,      //in
    AIL_INT   ArraySize,    //in
    void *    Param1Ptr,    //out
    void *    Param2Ptr,    //out
    AIL_INT64 ControlFlag   //in
)
```

## Description

This function gets array(s) of values from an image processing context.

The number of arrays and the nature of the values depend on the image processing context used and on the specified type of information to get. Before you can inquire the content of the array(s), they must be set using [`MimPut`](../../Reference/im/MimPut.md), unless otherwise specified.

To determine the size of the arrays required to store the requested values, call this function and set [`Param1Ptr`](../../Reference/im/MimGet.md) and [`Param2Ptr`](../../Reference/im/MimGet.md) to [`M_NULL`](../../Reference/im/MimGet.md). Alternatively, when dealing with a dead pixel correction image processing context, to determine the size of the arrays required to store the X- and Y- coordinates of the dead pixels, you can also use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_XY_DEAD_PIXELS_ARRAY_SIZE`](../../Reference/im/MimInquire.md). When dealing with a rearrangement image processing context, to determine the size of the arrays required to store the X- and Y- coordinates of the areas to move from the source to the destination and to determine the number of areas to be copied, you can also use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_XY_DESTINATION_ARRAY_SIZE`](../../Reference/im/MimInquire.md), [`M_XY_SIZE_ARRAY_SIZE`](../../Reference/im/MimInquire.md), and [`M_XY_SOURCE_ARRAY_SIZE`](../../Reference/im/MimInquire.md), respectively.

## Parameters

### `ContextImId` *(in, AIL_ID)*

Specifies the identifier of the image processing context. The image processing context must have been previously allocated with [`MimAlloc`](../../Reference/im/MimAlloc.md).

### `GetType` *(in, AIL_INT64)*

Specifies the type of information in the array(s).

### `ArraySize` *(in, AIL_INT)*

Specifies the number of values in the array(s).

### `Param1Ptr` *(out, *void)*

Specifies the address of the array in which to store the first set of values. Note that this parameter can be set to `M_NULL` when trying to establish the required size for the arrays.

### `Param2Ptr` *(out, *void)*

Specifies the address of the array in which to store the second set of values. This parameter can be set to `M_NULL` when trying to establish the required size for the arrays.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For getting arrays of value(s) from an image processing context

The following [`ContextImId`](../../Reference/im/MimGet.md),[`GetType`](../../Reference/im/MimGet.md), [`Param1Ptr`](../../Reference/im/MimGet.md), and [`Param2Ptr`](../../Reference/im/MimGet.md) settings can be specified to get values from an image processing context.

---

### `Augmentation context ID`

Specifies an augmentation context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimAugment`](../../Reference/im/MimAugment.md) operations.  You need not use [`MimPut`](../../Reference/im/MimPut.md) to inquire about an augmentation context.

#### `M_AUG_OPERATIONS_ENABLED`

Retrieves the list of the operations that were enabled for [`MimAugment`](../../Reference/im/MimAugment.md). Operations are enabled using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_..._OP`](../../Reference/im/MimControl.md).

---

### `Dead pixel correction image processing context ID`

Specifies a dead pixel correction image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_DEAD_PIXEL_CONTEXT`](../../Reference/im/MimAlloc.md) and used in [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) operations.

#### `M_XY_DEAD_PIXELS`

Retrieves the X- and Y-coordinates of the dead pixels in the source image, relative to the pixel coordinate system.

---

### `Rearrangement image processing context ID`

Specifies a rearrangement image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_REARRANGE_CONTEXT`](../../Reference/im/MimAlloc.md) and used in [`MimRearrange`](../../Reference/im/MimRearrange.md) operations.

#### `M_XY_DESTINATION`

Retrieves the X- and Y-offsets of the areas in the destination image buffer into which to copy the source areas, relative to the pixel coordinate system.

#### `M_XY_SIZE`

Retrieves the width and height of the areas to be copied, relative to the pixel coordinate system.

#### `M_XY_SOURCE`

Retrieves the X- and Y-offsets of the areas to be copied from the source image buffer into the destination image buffer, relative to the pixel coordinate system.

---

### `Unwarp along path image processing context ID`

Specifies an unwarp along path image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_UNWARP_ALONG_PATH_CONTEXT`](../../Reference/im/MimAlloc.md) and used in [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) operations.

#### `M_XY_PATH`

Retrieves the X- and Y-coordinates of the path in the source image, relative to the pixel coordinate system. To get the required array size, use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_PATH_ARRAY_SIZE`](../../Reference/im/MimInquire.md).

#### `M_XY_WORK_BOTTOM_GRID_VERTICAL_LINES`

Retrieves the X- and Y-coordinates of the bottom of the working vertical lines used to generate the grid in the source image, relative to the pixel coordinate system. These points lie along the bottom working path and are mapped to points along the bottom of the destination buffer. The bottom of the path is defined one-half-width below the working path relative to the direction of the path. To get the required array size, use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_DESTINATION_SIZE_X`](../../Reference/im/MimInquire.md). You can draw the vertical grid lines of the working path using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WORK_GRID_VERTICAL_LINES`](../../Reference/im/MimDraw.md).

#### `M_XY_WORK_BOTTOM_PATH`

Retrieves the X- and Y-coordinates of the bottom internal working path in the source image, relative to the pixel coordinate system. The bottom path is defined one-half-width below the working path relative to the direction of the path. To get the required array size, use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_WORK_PATH_ARRAY_SIZE`](../../Reference/im/MimInquire.md). You can draw the quadrilaterals formed from the intersection of the top and bottom working paths using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WORK_PATH`](../../Reference/im/MimDraw.md).

#### `M_XY_WORK_PATH`

Retrieves the X- and Y-coordinates of the internal working path in the source image, relative to the pixel coordinate system. To get the required array size, use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_WORK_PATH_ARRAY_SIZE`](../../Reference/im/MimInquire.md). You can draw the working path using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WORK_PATH`](../../Reference/im/MimDraw.md).

#### `M_XY_WORK_TOP_GRID_VERTICAL_LINES`

Retrieves the X- and Y-coordinates of the top of the working vertical lines used to generate the grid in the source image, relative to the pixel coordinate system. These points lie along the top working path and are mapped to points along the top of the destination buffer. The top path is defined one-half-width above the working path relative to the direction of the path. To get the required array size, use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_DESTINATION_SIZE_X`](../../Reference/im/MimInquire.md). You can draw the vertical grid lines of the working path using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WORK_GRID_VERTICAL_LINES`](../../Reference/im/MimDraw.md).

#### `M_XY_WORK_TOP_PATH`

Retrieves the X- and Y-coordinates of the top internal working path in the source image, relative to the pixel coordinate system. The top path is defined one-half-width above the working path relative to the direction of the path. To get the required array size, use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_WORK_PATH_ARRAY_SIZE`](../../Reference/im/MimInquire.md). You can draw the quadrilaterals formed from the intersection of the top and bottom working paths using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WORK_PATH`](../../Reference/im/MimDraw.md).

### Combination Constants — For specifying the data type of Param1Ptr and Param2Ptr

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the specified array to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Specifies that the array is an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Specifies that the array is an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Specifies that the array is an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Specifies that the array is an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Specifies that the array is an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the number of entries in the array(s). If [`Param1Ptr`](../../Reference/im/MimGet.md) and [`Param2Ptr`](../../Reference/im/MimGet.md) are both set to [`M_NULL`](../../Reference/im/MimGet.md), [`MimGet`](../../Reference/im/MimGet.md) returns the size of the required arrays.

## Remarks

> Note that some of the values listed above are not available in Aurora Imaging Library Lite. See the value's corresponding operation function for Aurora Imaging Library Lite availability.
