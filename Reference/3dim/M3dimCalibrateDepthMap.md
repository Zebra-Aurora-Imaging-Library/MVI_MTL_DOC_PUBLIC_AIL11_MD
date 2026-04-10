---
doctype: Reference
module: 3dim
function: M3dimCalibrateDepthMap
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimCalibrateDepthMap"
---

# M3dimCalibrateDepthMap

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

> Calibrate a buffer based on the bounding box of a container's point cloud, or an axis-aligned 3D box geometry, to prepare the buffer to hold a depth or intensity map.

## Syntax

```c
void M3dimCalibrateDepthMap(
    AIL_ID     SrcContainerBufOrGeometry3dgeoId,     //in
    AIL_ID     DepthMapImageBufId,                   //out
    AIL_ID     IntensityMapImageBufId,               //out
    AIL_ID     CalibrationAilObjectOrMatrix3dgeoId,  //in
    AIL_DOUBLE PixelSizeAspectRatio,                 //in
    AIL_INT64  ZSign,                                //in
    AIL_INT64  ControlFlag                           //in
)
```

## Description

This function calibrates a buffer based on the bounding box of a container's point cloud, or an axis-aligned 3D box geometry. For a point cloud, the source container's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components are used to establish the bounding box. The calibration prepares the buffer to hold depth map or intensity map information. To generate a depth map from a point cloud or 3D geometry, or to generate an intensity map, use [`M3dimProject`](../../Reference/3dim/M3dimProject.md).

[`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md) sets the real-world to pixel unit scale for the image buffer, and sets the real-world to gray level scale ([`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md)) for depth values.

You can set this function's [`PixelSizeAspectRatio`](../../Reference/3dim/M3dimCalibrateDepthMap.md) to [`M_NULL`](../../Reference/3dim/M3dimCalibrateDepthMap.md) to ensure an unconstrained pixel aspect ratio, so that source data fits completely into the depth map.

If the [`PixelSizeAspectRatio`](../../Reference/3dim/M3dimCalibrateDepthMap.md) parameter is not set to [`M_NULL`](../../Reference/3dim/M3dimCalibrateDepthMap.md), there can be leftover, unused pixel rows or columns in the destination. You can specify where to place valid values in the destination buffer with the [`ControlFlag`](../../Reference/3dim/M3dimCalibrateDepthMap.md) parameter and [`M_DEFAULT`](../../Reference/3dim/M3dimCalibrateDepthMap.md) or [`M_CENTER`](../../Reference/3dim/M3dimCalibrateDepthMap.md). In the [`M_DEFAULT`](../../Reference/3dim/M3dimCalibrateDepthMap.md) case, the data aligns to the top left of the buffer, and the function calculates a pixel size such that the data extends at least the breadth of one dimension, and no data is lost along the other dimension. In the [`M_CENTER`](../../Reference/3dim/M3dimCalibrateDepthMap.md) case, valid data is centered in the buffer and extends the full breadth of one dimension with no loss of data along the other dimension. [`M3dimProject`](../../Reference/3dim/M3dimProject.md) will set pixels in unused rows or columns to the invalid pixel value. Note that the calculated pixel size does not distort the world data and the overall aspect ratio of source information is maintained. Also note that a projected point retains its source coordinate values, regardless of whether the corresponding pixel is centered ([`M_CENTER`](../../Reference/3dim/M3dimCalibrateDepthMap.md)) or not ([`M_DEFAULT`](../../Reference/3dim/M3dimCalibrateDepthMap.md)). This means that the depth map's top-left pixel will have different world XY-coordinates, depending on the [`ControlFlag`](../../Reference/3dim/M3dimCalibrateDepthMap.md) setting. See [Specifying data placement in the destination image buffer](../../UserGuide/C35_3D_Image_processing/S15_Generating_a_fully_corrected_depth_map.md) for more information.

By default, the relative coordinate system is set to the same location as the absolute coordinate system. You can optionally set the relative coordinate system to a different location by specifying (with [`CalibrationAilObjectOrMatrix3dgeoId`](../../Reference/3dim/M3dimCalibrateDepthMap.md)) a calibration context, a calibrated Aurora Imaging Library object, or a transformation matrix object. If the destination is a depth map container, matrix information will be added to the [`M_COMPONENT_MATRIX_AIL`](../../Reference/buf/MbufAllocComponent.md) component of the container. Note that the pixel-to-world mapping of the specified calibration context or calibrated Aurora Imaging Library object is not used.

## Parameters

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the source point cloud container or 3D geometry object.

### `DepthMapImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or container with which to associate the calibration information for a depth map.

### `IntensityMapImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer with which to associate the calibration information for an intensity map. This image buffer must be a 1-band buffer and must have the same dimensions as the buffer specified for [`DepthMapImageBufId`](../../Reference/3dim/M3dimCalibrateDepthMap.md).

### `CalibrationAilObjectOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library object that defines the relationship between the relative and absolute coordinate systems in the depth map. The Aurora Imaging Library object can be a camera calibration context, a 3D reconstruction context, a 3D reconstruction result buffer, or any object that has camera calibration information, such as an image or result buffer. It can also be a transformation matrix object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). If the destination is a depth map container, matrix information will be added to the [`M_COMPONENT_MATRIX_AIL`](../../Reference/buf/MbufAllocComponent.md) component. If no calibration context or object associated with a camera calibration context is required, set this parameter to `M_NULL`.

### `PixelSizeAspectRatio` *(in, AIL_DOUBLE)*

Specifies how to adjust the X and Y pixel sizes.

*For specifying the pixel aspect ratio*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies to apply a pixel aspect ratio such that the source container's bounding box or the axis-aligned box fits exactly inside the dimensions of the destination image buffer or container. |
| `Value > 0.0` *(default)* | Specifies an aspect ratio for the destination pixels. The pixel size is computed such that the pixel's length in X divided by its length in Y equals the specified [`PixelSizeAspectRatio`](../../Reference/3dim/M3dimCalibrateDepthMap.md) value. |

### `ZSign` *(in, AIL_INT64)*

Specifies the sign of the Z-scale. The sign applies to [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md), which sets the length, in world units, of one gray level in a fully-corrected depth map.

*For specifying the Z-scale sign*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEGATIVE` | Specifies a negative value for [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md). A negative value indicates that larger Z-values are represented by darker depth map pixels, and the smaller the Z-value the brighter the pixel. |
| `M_POSITIVE` *(default)* | Specifies a positive value for [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md). A positive value indicates that larger Z-values are represented by brighter depth map pixels, and the smaller the Z-value the darker the pixel. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies where to place valid values in the depth map. When [`PixelSizeAspectRatio`](../../Reference/3dim/M3dimCalibrateDepthMap.md) is set to [`M_NULL`](../../Reference/3dim/M3dimCalibrateDepthMap.md), [`ControlFlag`](../../Reference/3dim/M3dimCalibrateDepthMap.md) must be set to [`M_DEFAULT`](../../Reference/3dim/M3dimCalibrateDepthMap.md).

*For specifying the control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that, once [`M3dimProject`](../../Reference/3dim/M3dimProject.md) is called, the point cloud or 3D geometry data is projected onto the depth map buffer such that it aligns to the top left of the buffer, and the data extends at least the breadth of one dimension, leaving space at the bottom or right. [`M3dimProject`](../../Reference/3dim/M3dimProject.md) will set pixels in unused rows or columns to the invalid pixel value. The source data maintains its aspect ratio. |
| `M_CENTER` | Specifies that, once [`M3dimProject`](../../Reference/3dim/M3dimProject.md) is called, the point cloud data is projected onto the depth map buffer such that it is centered horizontally or vertically in the buffer. [`M3dimProject`](../../Reference/3dim/M3dimProject.md) will set pixels in unused rows or columns to the invalid pixel value. The source data maintains its aspect ratio. |
