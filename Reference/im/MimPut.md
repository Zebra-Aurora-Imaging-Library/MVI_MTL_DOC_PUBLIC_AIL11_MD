---
doctype: Reference
module: im
function: MimPut
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimPut"
---

# MimPut

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

> Puts arrays of values into an image processing context.

## Syntax

```c
void MimPut(
    AIL_ID       ContextImId,  //out
    AIL_INT64    PutType,      //in
    AIL_INT      ArraySize,    //in
    const void * Param1Ptr,    //in
    const void * Param2Ptr,    //in
    AIL_INT64    ControlFlag   //in
)
```

## Description

This function puts arrays of values into a specified image processing context.

The number of arrays and the nature of the values depend on the image processing context used and on the specified type of information to set. After setting the array(s), their values can be inquired using [`MimGet`](../../Reference/im/MimGet.md).

## Parameters

### `ContextImId` *(out, AIL_ID)*

Specifies the identifier of the image processing context. The image processing context must have been previously allocated with [`MimAlloc`](../../Reference/im/MimAlloc.md).

### `PutType` *(in, AIL_INT64)*

Specifies the type of information in the array(s).

### `ArraySize` *(in, AIL_INT)*

Specifies the number of values in the array(s).

### `Param1Ptr` *(in, *void)*

Specifies the address of the first array of values.

### `Param2Ptr` *(in, *void)*

Specifies the address of the second array of values.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For setting array(s) of values into an image processing context

The following [`ContextImId`](../../Reference/im/MimPut.md), [`PutType`](../../Reference/im/MimPut.md), [`Param1Ptr`](../../Reference/im/MimPut.md), and [`Param2Ptr`](../../Reference/im/MimPut.md) parameter settings can be specified to put values into an image processing context.

---

### `Dead pixel correction image processing context ID`

Specifies a dead pixel correction image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_DEAD_PIXEL_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) operations.

#### `M_XY_DEAD_PIXELS`

Specifies the X- and Y-coordinates of the dead pixels in the source image, relative to the pixel coordinate system.  Note that you should only use this setting if you have not specified an image that identifies the dead pixels, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEAD_PIXELS`](../../Reference/im/MimControl.md).

---

### `Rearrangement image processing context ID`

Specifies a rearrangement image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_REARRANGE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimRearrange`](../../Reference/im/MimRearrange.md) operations.

#### `M_XY_DESTINATION`

Specifies the X- and Y-offsets of the areas in the destination image buffer into which to copy the source areas, relative to the pixel coordinate system.

#### `M_XY_SIZE`

Specifies the width and height of the areas to copy, relative to the pixel coordinate system.  > **Note:** Note that this settings is only available when in rectangle mode ([`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md) set to [`M_RECTS`](../../Reference/im/MimControl.md)).

#### `M_XY_SOURCE`

Specifies the X- and Y-offsets of the areas to copy from the source image buffer into the destination image buffer, relative to the pixel coordinate system.

---

### `Unwarp along path image processing context ID`

Specifies an unwarp along path image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_UNWARP_ALONG_PATH_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) operations.

#### `M_XY_PATH`

Specifies the X- and Y-coordinates of the path in the source image, relative to the pixel coordinate system. To set the width of the path, use [`MimControl`](../../Reference/im/MimControl.md) with [`M_SINGLE_WIDTH`](../../Reference/im/MimControl.md).

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

## Remarks

> Note that some of the values listed above are not available in Aurora Imaging Library Lite. See the value's corresponding operation function for Aurora Imaging Library Lite availability.

When in line mode ([`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md) set to [`M_LINES`](../../Reference/im/MimControl.md)), all values in this array must be set to 0, because [`MimRearrange`](../../Reference/im/MimRearrange.md) can only copy entire rows in [`M_LINES`](../../Reference/im/MimControl.md) mode. For example, when dealing with 3 areas, the X-offset array must contain the following: [0, 0, 0].
