---
doctype: Reference
module: 3dim
function: M3dimConvertDepthMap
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimConvertDepthMap"
---

# M3dimConvertDepthMap

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

> Convert data from a depth map container to a depth map image, or vice versa.

## Syntax

```c
void M3dimConvertDepthMap(
    AIL_ID    SrcDepthMapBufId,           //in
    AIL_ID    SrcIntensityMapImageBufId,  //in
    AIL_ID    DstDepthMapBufId,           //out
    AIL_ID    DstIntensityMapImageBufId,  //out
    AIL_INT64 Operation,                  //in
    AIL_INT64 Options,                    //in
    AIL_INT64 ControlFlag                 //in
)
```

## Description

This function converts data from a specified depth map container or depth map image buffer into an equivalent depth map image buffer or depth map container. This function can optionally copy only the calibration information between depth map containers, from a depth map container to an image buffer, or from a depth map image buffer to a container. [`M3dimConvertDepthMap`](../../Reference/3dim/M3dimConvertDepthMap.md) can also copy intensity values from the source depth map container's [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) or [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md) component (if one exists) to a destination intensity map image buffer. If a source intensity map image buffer is provided, intensity values are copied to the destination container's [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) component, which is created if not previously existing in the destination container. You can optionally specify [`M_PROPAGATE_REGION`](../../Reference/3dim/M3dimConvertDepthMap.md) to propagate a region of interest (ROI) from the source depth map container's [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component (if one exists) to an ROI in the destination depth map image buffer, or vice versa.

## Parameters

### `SrcDepthMapBufId` *(in, AIL_ID)*

Specifies the identifier of the source depth map image buffer or depth map container.

### `SrcIntensityMapImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source intensity map image buffer.

### `DstDepthMapBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or container in which to store the converted data or calibration information for a depth map.

### `DstIntensityMapImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to store the converted data or calibration information for an intensity map.

### `Operation` *(in, AIL_INT64)*

Specifies whether to convert the data or copy the calibration information.

### `Options` *(in, AIL_INT64)*

Specifies whether to propagate the ROI.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the conversion

---

### `M_CALIBRATE`

Specifies to copy the calibration information from the source depth map container or image buffer to the destination.

#### `Source depth map container ID`

Specifies the identifier of the depth map container from which to copy the calibration information.  The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

#### `Source depth map image buffer ID`

Specifies the identifier of the depth map image buffer from which to copy the calibration information.  The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer, and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

---

### `M_CONVERT`

Specifies to convert the data from the depth map container into the equivalent depth map and intensity map images or vice versa.

#### `Source depth map container ID`

Specifies the identifier of the depth map container from which to convert the data.  The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies not to propagate the ROI. If an [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source, each pixel outside the ROI will be set to the invalid value in the destination depth map image buffer. |
| `M_PROPAGATE_REGION` | Specifies to propagate the ROI to an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI in the destination depth map image buffer. |

#### `Source depth map image buffer ID`

Specifies the identifier of the depth map image buffer from which to convert the data.  The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer, and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies not to propagate the ROI. If an ROI is associated with the source, each pixel outside the ROI will be given a confidence value of 0 in the destination depth map container. |
| `M_PROPAGATE_REGION` | Specifies to propagate the ROI to an [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component in the destination depth map container. |
