---
doctype: Reference
module: 3dim
function: M3dimCopy
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimCopy"
---

# M3dimCopy

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

> Copy the extent box of a depth map into a 3D geometry.

## Syntax

```c
void M3dimCopy(
    AIL_ID    SrcDepthMapImageBufId,  //in
    AIL_ID    DstGeometry3dgeoId,     //out
    AIL_INT64 CopyType,               //in
    AIL_INT64 ControlFlag             //in
)
```

## Description

This function copies the specified depth map's extent box, calculated from the depth map buffer's size and calibration information, into the specified 3D geometry object.

## Parameters

### `SrcDepthMapImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source depth map container or depth map image buffer.

### `DstGeometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the destination 3D geometry object.

### `CopyType` *(in, AIL_INT64)*

Specifies how to copy the depth map's extent box into the 3D geometry object.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the copy type

---

### `M_EXTENT_BOX`

Specifies to copy the depth map's extent box into the 3D geometry object.  The extent box represents the maximum box that a depth map can span, irrespective of its data. The box defines the minimum/maximum X-, Y-, and Z-coordinates that would be representable in the depth map buffer, based on the buffer's size and calibration information.

#### `Depth map container ID from which to copy`

Specifies the identifier of the depth map container from which to copy the extent box.  The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

| Value | Description |
| --- | --- |
| `3D geometry object ID in which to copy` | Specifies the identifier of the destination 3D geometry object in which to copy the depth map's extent box.  The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that, after calling [`M3dimCopy`](../../Reference/3dim/M3dimCopy.md), the destination 3D geometry object will return type [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md). |

#### `Depth map image buffer ID from which to copy`

Specifies the identifier of the depth map image buffer from which to copy the extent box.  The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer, and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). If the image buffer has a region of interest (ROI) associated with it, the ROI is ignored.

| Value | Description |
| --- | --- |
| `3D geometry object ID in which to copy` | Specifies the identifier of the destination 3D geometry object in which to copy the depth map's extent box.  The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that, after calling [`M3dimCopy`](../../Reference/3dim/M3dimCopy.md), the destination 3D geometry object will return type [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md). |
