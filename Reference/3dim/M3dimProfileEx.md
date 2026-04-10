---
doctype: Reference
module: 3dim
function: M3dimProfileEx
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dim / M3dimProfileEx"
---

# M3dimProfileEx

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

> Takes the uniform profile of a depth map along a specified slicing plane according to the settings of the profile 3D image processing context.

## Syntax

```c
void M3dimProfileEx(
    AIL_ID     ProfileContext3dimId,           //in
    AIL_ID     SrcDepthBufId,                  //in
    AIL_ID     DstContainerBufOrResult3dimId,  //out
    AIL_INT64  Param1,                         //in
    AIL_DOUBLE Param2,                         //in
    AIL_DOUBLE Param3,                         //in
    AIL_DOUBLE Param4,                         //in
    AIL_DOUBLE Param5,                         //in
    AIL_INT64  ControlFlag                     //in
)
```

## Description

This function extracts a uniform depth map profile along a specified slicing plane. The specified destination container or result buffer is filled with coordinates that make up the profile. You can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to specify settings, such as the thickness of the profile and the interpolation mode to use.

Pixels are sampled along a specified line and corresponding points are considered for the profile. The specified line effectively defines a slicing plane for the depth map, since the pixels' intensity values indicate depth (Z-values). In [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md), if the specified line crosses a valid depth map pixel, the corresponding point is included in the profile. Whereas, [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) can sample multiple source pixels for each profile point so that the thickness of the line is taken into consideration; you can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimControl.md) to specify a thickness perpendicular to the profile line, and all points corresponding to pixels within the specified thickness of the line will be considered when determining the value of the profile point.

This function always uniformly extracts points along the profile line defined by the start and end points; pixels are sampled along the line using a specified sampling distance ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimControl.md)) so that each point in the profile is at an equal distance away from the next.

> **Note:** Note that when the destination is a result buffer, the profile contains valid points only; no profile points are extracted from gaps in the depth map. Invalid points can exist in the profile only when the destination is a container.

You can retrieve results using [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md). To draw the profile, use [`M3dimDraw3d`](../../Reference/3dim/M3dimDraw3d.md). Alternatively, you can use [`MgraDots`](../../Reference/gra/MgraDots.md) or [`M3dgraDots`](../../Reference/3dgra/M3dgraDots.md) to draw the profile points.

## Parameters

### `ProfileContext3dimId` *(in, AIL_ID)*

Specifies the identifier of the profile 3D image processing context. The context must have been allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROFILE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

### `SrcDepthBufId` *(in, AIL_ID)*

Specifies the identifier of the source depth map container or depth map image buffer.

*For specifying the source depth map container or depth map image buffer identifier*

| Value | Description |
| --- | --- |
| `Depth map container identifier` | Specifies the identifier of a depth map container.

The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

The container must not have a region component. Using a container with a region component will cause an error. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer.

The image buffer must be an 8-bit, 16-bit, or 32-bit unsigned, 1-band buffer, and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `DstContainerBufOrResult3dimId` *(out, AIL_ID)*

Specifies the identifier of the destination container or profile 3D image processing result buffer.

*For specifying the destination container or profile result buffer identifier*

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the identifier of a container.

The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. |
| `Profile 3D image processing result buffer identifier` | Specifies the identifier of a profile 3D image processing result buffer.

The result buffer must have been previously allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md). |

### `Param1` *(in, AIL_INT64)*

Specifies the first parameter, which determines the units with which to interpret the [`Param2`](../../Reference/3dim/M3dimProfileEx.md), [`Param3`](../../Reference/3dim/M3dimProfileEx.md), [`Param4`](../../Reference/3dim/M3dimProfileEx.md), and [`Param5`](../../Reference/3dim/M3dimProfileEx.md) parameters.

*For specifying the input units*

| Value | Description |
| --- | --- |
| `M_PIXEL` | Specifies to interpret the values in pixel units. |
| `M_WORLD` | Specifies to interpret the values in world units. |

### `Param2` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the first point that defines the line along which to extract points.

### `Param3` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the first point that defines the line along which to extract points.

### `Param4` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the second point that defines the line along which to extract points.

### `Param5` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the second point that defines the line along which to extract points.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
