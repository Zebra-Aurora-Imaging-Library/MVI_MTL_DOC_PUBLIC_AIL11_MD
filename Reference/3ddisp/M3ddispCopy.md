---
doctype: Reference
module: 3ddisp
function: M3ddispCopy
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispCopy"
---

# M3ddispCopy

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

> Copy data to or from a 3D display.

## Syntax

```c
void M3ddispCopy(
    AIL_ID    SrcAilObjectId,  //in
    AIL_ID    DstAilObjectId,  //out
    AIL_INT64 CopyType,        //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function copies data (such as a transformation matrix or background image) to or from a 3D display.

## Parameters

### `SrcAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the source object. Note that if the source object is not a 3D display, the destination object must be.

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the destination object. Note that if the destination object is not a 3D display, the source object must be.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Specifies if the 3D display should be refreshed.

*For specifying if the display should be updated*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. If a view is modified a hook will be generated. |
| `M_NO_HOOK` | Prevents calling the [`M_VIEW_CHANGE`](../../Reference/3ddisp/M3ddispHookFunction.md) hook after the function is completed.

> **Note:** This control flag is available for [`M_VIEW_MATRIX`](../../Reference/3ddisp/M3ddispCopy.md), with a 3D Display as the destination. |

## Parameter Associations

### For specifying the copy type

---

### `M_BACKGROUND_IMAGE`

Specifies to set the background image of a 3D display, or to copy the background image of a 3D display to an image buffer.

#### `3D display identifier from which to copy`

Specifies the identifier of the 3D display whose background to copy. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). An error is generated if the 3D display does not have a background image.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of the 8-bit unsigned image buffer in which to copy the background image. The image buffer must have the same width, height, and number of bands as the background image of the 3D display. You can determine the size and number of bands of the current background image buffer using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md)with [`M_BACKGROUND_IMAGE_SIZE_...`](../../Reference/3ddisp/M3ddispInquire.md). |

#### `Image buffer identifier from which to copy`

Specifies the identifier of the 1- or 3-band, 8-bit unsigned image buffer to copy to the background of the 3D display.

| Value | Description |
| --- | --- |
| `3D display identifier` | Specifies the identifier of the 3D display to which the image buffer is copied. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). |

---

### `M_RENDER`

Specifies to copy the resulting image of the current render to the destination buffer or container. The render is guaranteed to be up to date when [`M_UPDATE`](../../Reference/3ddisp/M3ddispControl.md) is set to [`M_ENABLE`](../../Reference/3ddisp/M3ddispControl.md). Otherwise, the image that is currently on the screen will be copied.

#### `3D display identifier from which to copy`

Specifies the identifier of the 3D display whose current render to copy. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the identifier of the Aurora Imaging Library container in which to copy. The result of this will be a container with one component ([`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md)with the appropriate size, band, and data type). If a component like this already exists in the container, it will be reused; otherwise, the container will be emptied and a new component will be allocated. |
| `Image buffer identifier` | Specifies the identifier of the Aurora Imaging Library image buffer in which to copy. It is recommended to copy to an 8-bit, 3-band, unsigned image buffer. If the destination is smaller in size than the source, the image is cropped to the size of the destination buffer. If the destination is larger in size than the source, exceeding areas of the buffer are unaffected. |

---

### `M_ROTATION_AXIS`

Specifies to set the auto-rotation axis of a 3D display, or to copy the auto-rotation axis of a 3D display to a 3D line geometry object. This is the axis that the view of the 3D display rotates around when [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md) is set to [`M_ENABLE`](../../Reference/3ddisp/M3ddispControl.md).

#### `3D display identifier from which to copy`

Specifies the identifier of the 3D display whose auto-rotation axis to copy. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

| Value | Description |
| --- | --- |
| `3D geometry object identifier` | Specifies the identifier of the 3D geometry object to define. The 3D geometry object is defined as an infinite line. |

#### `3D line geometry object identifier`

Specifies the identifier of the 3D line geometry object used to set the 3D display's view. The 3D line geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.

| Value | Description |
| --- | --- |
| `3D display identifier` | Specifies the identifier of the 3D display whose view to set. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). |

---

### `M_ROTATION_AXIS_CENTER`

Specifies to set the center position of the auto-rotation axis of a 3D display, or to copy this position from a 3D display to a 3D point geometry object. This is the point that the view of the 3D display rotates around when [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md) is set to [`M_ENABLE`](../../Reference/3ddisp/M3ddispControl.md).

#### `M_VIEW_INTEREST_POINT`

Specifies to set the 3D display's rotation axis center point to its interest point.

| Value | Description |
| --- | --- |
| `3D display identifier` | Specifies the identifier of the 3D display whose view to set. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). |

#### `3D display identifier from which to copy`

Specifies the identifier of the 3D display whose auto-rotation center point to copy. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

| Value | Description |
| --- | --- |
| `3D geometry object identifier` | Specifies the identifier of the 3D geometry object in which to copy the position. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md)The 3D geometry object is defined as a 3D point. |

#### `3D point geometry object identifier`

Specifies the identifier of the 3D point geometry object used to set the 3D display's auto-rotation center point. The 3D point geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point.

| Value | Description |
| --- | --- |
| `3D display identifier` | Specifies the identifier of the 3D display whose view to set. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). |

---

### `M_VIEW_MATRIX`

Specifies to copy the view's transformation matrix. If copied before the view of the 3D display is moved, you can use this matrix with [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)or [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)to restore the current position and orientation of the view.  The transformation matrix of a view is relative to a view with the viewpoint at (0,0,1), the interest point at (0,0,0), and an up vector of (0,1,0). This can also be thought of as a direct top-down view, with the up vector aligned with the Y-axis.

#### `M_IDENTITY_MATRIX`

Specifies to set the 3D display's view to the identity matrix. This can be used to reset the view's orientation.

| Value | Description |
| --- | --- |
| `3D display identifier` | Specifies the identifier of the 3D display whose view to set. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). |

#### `3D display identifier from which to copy`

Specifies the identifier of a 3D display from which to copy the view's orientation. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the view's orientation. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Transformation matrix object identifier`

Specifies the identifier of the transformation matrix object used to set the orientation of the 3D display's view. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

| Value | Description |
| --- | --- |
| `3D display identifier` | Specifies the identifier of the 3D display of which to set the view's orientation. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). |

### Combination Constants — Specifies the starting position

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to use the current viewpoint as the starting point for the transformation.

| Value | Description |
| --- | --- |
| `M_COMPOSE_WITH_CURRENT` | Specifies to use the current viewpoint as the starting point for the transformation. This setting can only be used when copying a transformation matrix object to a 3D display. |
