---
doctype: Reference
module: 3dmap
function: M3dmapAddScan
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapAddScan"
---

# M3dmapAddScan

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

> Extract laser line data from an image and store this data in a 3D reconstruction result buffer.

## Syntax

```c
void M3dmapAddScan(
    AIL_ID    LaserContext3dmapId,        //in
    AIL_ID    Result3dmapId,              //out
    AIL_ID    LaserOrDepthMapImageBufId,  //in
    AIL_ID    IntensityImageBufId,        //in
    AIL_ID    ExtraInfoArrayId,           //in
    AIL_INT   PointCloudLabel,            //in
    AIL_INT64 ControlFlag                 //in
)
```

## Description

This function stores laser line data extracted from grabbed images into the specified result buffer.

If the specified result buffer is of type [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), you can use this data to calibrate a laser reconstruction setup using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) or [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md). If your camera has integrated hardware capable of extracting the laser line data, call [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with [`M_LINE_ALREADY_EXTRACTED`](../../Reference/3dmap/M3dmapAddScan.md) to skip the extraction process and store laser line data in the result buffer.

If the number of extracted laser lines is greater than the maximum number of lines that can be stored in the result buffer, specified using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_MAX_FRAMES`](../../Reference/3dmap/M3dmapControl.md), the result buffer's oldest values are overwritten with the new ones.

When specifying a 3D reconstruction result buffer allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) either create a new point cloud in the result buffer and add laser line data to it, or just add laser line data to the specified point cloud in the result buffer. To create a new point cloud, specify a point cloud label that does not currently exist in the specified 3D reconstruction result buffer.

Between each set of scans for different objects or for cumulative extraction mode, you can clear the content of the result buffer using [`M3dmapClear`](../../Reference/3dmap/M3dmapClear.md).

> **Note:** Note that the size and depth of the source image buffer containing the laser lines must not change when making multiple calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with the same destination result buffer.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer ([`Result3dmapId`](../../Reference/3dmap/M3dmapAddScan.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object. This is valid as long as the [`PointCloudLabel`](../../Reference/3dmap/M3dmapAddScan.md) parameter of the concurrent calls is set to a different index.

## Parameters

### `LaserContext3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D reconstruction context. The 3D reconstruction context must have been previously allocated on the required system using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md).

### `Result3dmapId` *(out, AIL_ID)*

Specifies the identifier of the 3D reconstruction result buffer in which to store the laser line data. The 3D reconstruction result buffer must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), or [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md).

### `LaserOrDepthMapImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the grabbed image of the laser line.

### `IntensityImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the intensity map, if using a 3D camera. If you are not using a 3D camera, set this parameter to `M_NULL`. If you use a 3D camera that does not provide intensity map data (or if you want to exclude it), you should also set this parameter to [`M_NULL`](../../Reference/3dmap/M3dmapAddScan.md).

### `ExtraInfoArrayId` *(in, AIL_ID)*

Reserved for future expansion. This parameter must be set to `M_NULL`.

### `PointCloudLabel` *(in, AIL_INT)*

Specifies the label of the point cloud in the result buffer in which to add the laser line data, if the result buffer has been allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). For all other types of 3D reconstruction result buffers, set this parameter to `M_DEFAULT`.

*For specifying the label of a point cloud*

| Value | Description |
| --- | --- |
| `M_POINT_CLOUD_LABEL` | Specifies the label of the point cloud.

If the result buffer does not contain a point cloud with the specified label, a new point cloud is created in the result buffer. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to one of the following values:

*To specify the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EXTRACT_LINE` *(default)* | Extracts the laser line data from the input image, corrects the data if the 3D reconstruction context is calibrated, and stores it in the result buffer. |
| `M_LINE_ALREADY_EXTRACTED` | Specifies that the uncorrected depth map and intensity map are given as input. Specify this control flag only if the 3D Reconstruction module should not perform the laser line extraction, as is the case for 3D cameras that perform laser line extraction. |
