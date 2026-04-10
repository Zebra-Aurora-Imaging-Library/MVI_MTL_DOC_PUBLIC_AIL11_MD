---
doctype: Reference
module: 3dmeas
function: M3dmeasFindMarker
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasFindMarker"
---

# M3dmeasFindMarker

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

> Search for markers in a depth map.

## Syntax

```c
void M3dmeasFindMarker(
    AIL_ID    Context3dmeasId,           //in
    AIL_ID    DepthMapOrContainerBufId,  //in
    AIL_ID    Result3dmeasId,            //out
    AIL_INT64 ControlFlag                //in
)
```

## Description

This function searches for markers in the specified depth map and writes the results in the specified result buffer. You can retrieve results using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md). Markers denote the location of transitions along a profile. Edge transitions go from a lower Z-value to a higher Z-value (or vice versa). Note that invalid transitions can also be identified, either in the middle of an invalid region, or where the data goes from valid to invalid (or vice versa).

To find markers along a specified path in the supplied depth map, you must use a path 3D measurement context. You must add a path to the path 3D measurement context, using [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md). Setting a path is useful when you want to find an object but you don't know its exact location. You can set a path that crosses through possible locations to detect height changes along the line. In this case, [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) extracts a single profile along the path and finds the markers in the profile.

If you want to verify if an object is at an expected location or has an expected shape, you can find markers perpendicular to a template. To set a template where you expect a height change to occur, you must use a template 3D measurement context. You must add a template that follows an expected edge of your object to the template 3D measurement context, using [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md). In this case, [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) extracts profiles at regular intervals perpendicular to the template and finds the best marker close to the template in each profile.

If your template runs along a column(s) of the depth map, you can use a profile 3D measurement context instead, in which case the profile extraction step is skipped and the find marker operation is performed on each row (profile) of the depth map; you don't explicitly define the template. If you supply the profiles, the 3D measurement module can find the specified number of markers along each profile.

If you use a template 3D measurement context, you can use [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) after calling [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) to fit a geometry to the found markers to compare its position with the defined template. Alternatively, you can find the markers from the template's perpendicular profiles and perform the fit simultaneously, using [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) with a template 3D measurement context and a depth map. When the find and fit are performed simultaneously, all possible markers are considered and the ones that provide the best fit are kept.

Use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) to configure profile extraction settings and marker search settings before calling this function.

## Parameters

### `Context3dmeasId` *(in, AIL_ID)*

Specifies the 3D measurement context. The context must have been allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIND_MARKER_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), [`M_FIND_MARKER_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), or [`M_FIND_MARKER_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md).

### `DepthMapOrContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the depth map container or depth map image buffer.

*For specifying the depth map container or depth map image buffer identifier*

| Value | Description |
| --- | --- |
| `Depth map container identifier` | Specifies the identifier of a depth map container.

The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

The container must not have a region component. Using a container with a region component will cause an error. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer.

The image buffer must be an 8-bit, 16-bit, or 32-bit unsigned, 1-band buffer, and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `Result3dmeasId` *(out, AIL_ID)*

Specifies the identifier of the 3D measurement result buffer. The result buffer must have been allocated using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) with [`M_FIND_MARKER_PROFILE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), [`M_FIND_MARKER_PATH_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), or [`M_FIND_MARKER_TEMPLATE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), and must match the specified context.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
