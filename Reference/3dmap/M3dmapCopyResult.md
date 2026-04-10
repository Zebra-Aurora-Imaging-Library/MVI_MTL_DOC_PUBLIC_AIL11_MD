---
doctype: Reference
module: 3dmap
function: M3dmapCopyResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmap / M3dmapCopyResult"
---

# M3dmapCopyResult

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

> Copy a group of results from a 3D alignment result buffer or 3D reconstruction result buffer into a container, an image buffer, or a 3D transformation matrix.

## Syntax

```c
void M3dmapCopyResult(
    AIL_ID    SrcResult3dmapId,  //in
    AIL_INT   SrcLabelOrIndex,   //in
    AIL_ID    DstAilObjectId,    //out
    AIL_INT64 CopyType,          //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function copies a group of results (for example, a point cloud) from a 3D reconstruction result buffer into a container or an image buffer. This function also copies calibration and alignment parameters from a 3D alignment result buffer into a 3D transformation matrix.

## Parameters

### `SrcResult3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D reconstruction result buffer containing point cloud or laser line data, or the 3D alignment result buffer containing calibration and alignment parameters. The 3D alignment result buffer must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_ALIGN_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). The 3D reconstruction result buffer must have been previously allocated using[`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md)with [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), or [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md).

### `SrcLabelOrIndex` *(in, AIL_INT)*

Specifies the point cloud(s) in the specified 3D reconstruction result buffer to copy. Only 3D reconstruction result buffers allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) have point clouds. For all other 3D reconstruction result buffer types, set this parameter to `M_DEFAULT`.

*For specifying the point cloud(s)*

| Value | Description |
| --- | --- |
| `M_POINT_CLOUD_INDEX` | Specifies an existing point cloud with the given index. |
| `M_POINT_CLOUD_LABEL` | Specifies an existing point cloud with the given label. |
| `M_ALL` | Specifies all point clouds in the specified 3D reconstruction result buffer.

> **Note:** [`M_ALL`](../../Reference/3dmap/M3dmapCopyResult.md) is supported only when [`CopyType`](../../Reference/3dmap/M3dmapCopyResult.md) is set to [`M_POINT_CLOUD_UNORGANIZED`](../../Reference/3dmap/M3dmapCopyResult.md).

> **Note:** Note that the destination container can hold only 1 point cloud. If [`M_ALL`](../../Reference/3dmap/M3dmapCopyResult.md) is specified, the point clouds are merged into one. |

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the destination container, image buffer, or transformation matrix in which to save the copied information.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to copy reflectance values, when the [`SrcResult3dmapId`](../../Reference/3dmap/M3dmapCopyResult.md) parameter is set to an [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer. If the source is not an [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, set this parameter to [`M_DEFAULT`](../../Reference/3dmap/M3dmapCopyResult.md).

*For specifying the control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to copy intensity values to the destination container's [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) component, which is created if not previously existing in the destination container. Note that, if no intensity values are available in the source point cloud, then no reflectance component is created. |
| `M_NO_REFLECTANCE` | Specifies not to copy intensity values. |

## Parameter Associations

### For specifying the copy type and destination object for a source 3D reconstruction result buffer or 3D alignment result buffer

> **Note:** Note that these copy types are only available after a successfully completed alignment operation (when [`M_STATUS`](../../Reference/3dmap/M3dmapGetResult.md) is [`M_COMPLETE`](../../Reference/3dmap/M3dmapGetResult.md)) or if a status error is returned (when [`M_STATUS`](../../Reference/3dmap/M3dmapGetResult.md) is [`M_RESULTS_Z_NEED_BACKGROUND`](../../Reference/3dmap/M3dmapGetResult.md), [`M_MAYBE_IMPRECISE`](../../Reference/3dmap/M3dmapGetResult.md), or [`M_CHAMFER_NOT_FOUND`](../../Reference/3dmap/M3dmapGetResult.md)).

---

### `3D alignment result buffer identifier`

Specifies the identifier of a 3D alignment result buffer ([`M_ALIGN_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md)) that stores results generated using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).

#### `M_RIGID_MATRIX`

Specifies to copy the full rigid transformation matrix that aligns the working coordinate system with the alignment object's coordinate system, depending on the settings of [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_XY_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_X_POSITION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_Y_POSITION`](../../Reference/3dmap/M3dmapControl.md), and [`M_ALIGN_Z_POSITION`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the alignment matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_ROTATION_MATRIX`

Specifies to copy the full rotation transformation matrix that aligns the working coordinate system with the alignment object's coordinate system, depending on the settings of [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_XY_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), and [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the alignment matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_SHEAR_MATRIX`

Specifies to copy the transformation matrix that corrects the shear and scale.

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the alignment matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_TRANSFORMATION_MATRIX`

Specifies to copy the full transformation matrix that corrects the shear and scale, and aligns the working coordinate system with the alignment object's coordinate system, depending on the settings of [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_XY_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_X_POSITION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_Y_POSITION`](../../Reference/3dmap/M3dmapControl.md), and [`M_ALIGN_Z_POSITION`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the alignment matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_XY_RIGID_MATRIX`

Specifies to copy the rigid transformation matrix that aligns the working coordinate system with the alignment object's coordinate system, depending on the settings of [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_X_POSITION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_Y_POSITION`](../../Reference/3dmap/M3dmapControl.md), and [`M_ALIGN_Z_POSITION`](../../Reference/3dmap/M3dmapControl.md).  The transformation matrix enforces the Z-axis to be perpendicular to the floor or conveyor, with no enforcement on the X- and Y-axes. Although the X- and/or Y-axes might move, the transformation to align the X- and Y-axes with that of the alignment object is not included.  > **Note:** Note that for the alignment disk, this is equivalent to [`M_RIGID_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the alignment matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_XY_ROTATION_MATRIX`

Specifies to copy the rotation transformation matrix that aligns the working coordinate system with the alignment object's coordinate system, depending on the setting of [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md).  The transformation matrix enforces the Z-axis to be perpendicular to the floor or conveyor, with no enforcement on the X- and Y-axes. Although the X- and/or Y-axes might move, the transformation to align the X- and Y-axes with that of the alignment object is not included.  > **Note:** Note that for the alignment disk, this is equivalent to [`M_ROTATION_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the alignment matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_XY_TRANSFORMATION_MATRIX`

Specifies to copy the transformation matrix that corrects the shear and scale, and aligns the working coordinate system with the alignment object's coordinate system, depending on the settings of [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_X_POSITION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_Y_POSITION`](../../Reference/3dmap/M3dmapControl.md), and [`M_ALIGN_Z_POSITION`](../../Reference/3dmap/M3dmapControl.md).  The transformation matrix enforces the Z-axis to be perpendicular to the floor or conveyor, with no enforcement on the X- and Y-axes. Although the X- and/or Y-axes might move, the transformation to align the X- and Y-axes with that of the alignment object is not included.  > **Note:** Note that for the alignment disk, this is equivalent to [`M_TRANSFORMATION_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix object in which to copy the alignment matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `3D reconstruction depth corrected data result buffer identifier`

Specifies the identifier of a 3D reconstruction result buffer that stores results generated in [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) mode.

#### `M_INTENSITY_MAP`

Copies the intensity map generated using acquired laser line data. In an intensity map, the gray value of each pixel represents the luminous intensity of the laser line at this point. The size of the intensity map corresponds to the size of the uncorrected depth map obtained with [`M_UNCORRECTED_DEPTH_MAP`](../../Reference/3dmap/M3dmapCopyResult.md).

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the destination image buffer identifier.  The destination image buffer must be a 1-band, 8-bit or 16-bit unsigned buffer. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.  The X-size of the destination image buffer should be at least either the X-size or Y-size of the laser line images used to generate the data, depending on whether [`MimControl`](../../Reference/im/MimControl.md) with [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_VERTICAL`](../../Reference/im/MimControl.md) or [`M_HORIZONTAL`](../../Reference/im/MimControl.md), respectively. The Y-size of the destination image buffer must be at least as large as the number of scanned laser lines to accumulate in the buffer; this number's maximum possible value is set using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_MAX_FRAMES`](../../Reference/3dmap/M3dmapControl.md). Note that the number of scanned laser lines actually accumulated can be lower than this maximum.  Use [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_UNCORRECTED_DEPTH_MAP_SIZE_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_UNCORRECTED_DEPTH_MAP_SIZE_Y`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_INTENSITY_MAP_BUFFER_TYPE`](../../Reference/3dmap/M3dmapGetResult.md) to obtain the necessary buffer dimensions, data type, and depth. |

#### `M_PARTIALLY_CORRECTED_DEPTH_MAP`

Generates a partially corrected depth map. In a partially corrected depth map, the gray value of a pixel accurately represents real world depth, but any shape distortion (due to the camera's angle) is not corrected.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the destination image buffer identifier.  The destination image buffer must be a 1-band, 8-bit or 16-bit unsigned buffer. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.  The X-size of the destination image buffer should be at least either the X-size or Y-size of the laser line images used to generate the data, depending on whether [`MimControl`](../../Reference/im/MimControl.md) with [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_VERTICAL`](../../Reference/im/MimControl.md) or [`M_HORIZONTAL`](../../Reference/im/MimControl.md), respectively. The Y-size of the destination image buffer must be at least as large as the number of scanned laser lines to accumulate in the buffer; this number's maximum possible value is set using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_MAX_FRAMES`](../../Reference/3dmap/M3dmapControl.md). Note that the number of scanned laser lines actually accumulated can be lower than this maximum.  Use [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_Y`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_PARTIALLY_CORRECTED_DEPTH_MAP_BUFFER_TYPE`](../../Reference/3dmap/M3dmapGetResult.md) to obtain the necessary buffer dimensions, data type, and depth. |

#### `M_UNCORRECTED_DEPTH_MAP`

Copies the uncorrected depth map generated using acquired laser line data. In uncorrected depth maps, the gray value of a pixel is not scaled to the real depth in the world, and any shape distortion (due to the camera's angle) is not corrected.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the destination image buffer identifier.  The destination image buffer must be a 1-band, 8-bit or 16-bit unsigned buffer. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.  The X-size of the destination image buffer should be at least either the X-size or Y-size of the laser line images used to generate the data, depending on whether [`MimControl`](../../Reference/im/MimControl.md) with [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_VERTICAL`](../../Reference/im/MimControl.md) or [`M_HORIZONTAL`](../../Reference/im/MimControl.md), respectively. The Y-size of the destination image buffer must be at least as large as the number of scanned laser lines to accumulate in the buffer; this number's maximum possible value is set using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_MAX_FRAMES`](../../Reference/3dmap/M3dmapControl.md). Note that the number of scanned laser lines actually accumulated can be lower than this maximum.  Use [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_UNCORRECTED_DEPTH_MAP_SIZE_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_UNCORRECTED_DEPTH_MAP_SIZE_Y`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_UNCORRECTED_DEPTH_MAP_BUFFER_TYPE`](../../Reference/3dmap/M3dmapGetResult.md) to obtain the necessary buffer dimensions, data type, and depth. Use [`M_UNCORRECTED_DEPTH_MAP_FIXED_POINT`](../../Reference/3dmap/M3dmapGetResult.md) to obtain the fixed point used to encode the laser line Y position. |

---

### `3D reconstruction laser calibration result buffer identifier`

Specifies the identifier of a 3D reconstruction result buffer that stores images of laser line displacement at specified heights during the 3D reconstruction calibration process.

---

### `3D reconstruction point cloud result buffer identifier`

Specifies the identifier of a 3D reconstruction result buffer that stores results generated in [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) mode.

#### `M_POINT_CLOUD`

Copies all points, preserving the point cloud's organizational type.  > **Note:** All copied point coordinate values are respective to the relative coordinate system, unless the [`M_ABSOLUTE_COORDINATE_SYSTEM`](../../Reference/3dmap/M3dmapCopyResult.md) combination value is specified.

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the destination container identifier.  The destination container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. If a range component and a confidence component already exist in the container, they will be overwritten. If the container is empty, range and confidence components are created to hold the copied information. |

#### `M_POINT_CLOUD_UNORGANIZED`

Copies only valid points, excluding points in the specified [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer that are set to **M_INVALID_POINT**. The point cloud's organizational type is not preserved.  > **Note:** Note that, when you use [`M_ALL`](../../Reference/3dmap/M3dmapCopyResult.md) to specify all point clouds in the [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, the point clouds are merged into a single point cloud, and then placed into the destination container.  > **Note:** All copied point coordinate values are respective to the relative coordinate system, unless the [`M_ABSOLUTE_COORDINATE_SYSTEM`](../../Reference/3dmap/M3dmapCopyResult.md) combination value is specified.

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the destination container identifier.  The destination container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. If a range component and a confidence component already exist in the container, they will be overwritten. If the container is empty, range and confidence components are created to hold the copied information. |

### Combination Constants — For copying points with respect to the absolute coordinate system

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to copy points with respect to the absolute coordinate system.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_COORDINATE_SYSTEM` | Specifies to copy points with respect to the absolute coordinate system. |
