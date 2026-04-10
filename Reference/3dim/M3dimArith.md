---
doctype: Reference
module: 3dim
function: M3dimArith
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimArith"
---

# M3dimArith

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

> Perform a point-to-point arithmetic operation on depth maps and 3D geometries.

## Syntax

```c
void M3dimArith(
    AIL_ID    Src1ImageBufOrGeometry3dgeoId,  //in
    AIL_ID    Src2ImageBufOrGeometry3dgeoId,  //in
    AIL_ID    DstImageBufId,                  //out
    AIL_ID    MaskBufId,                      //in
    AIL_INT64 Operation,                      //in
    AIL_INT64 SignMode,                       //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function performs the specified point-to-point arithmetic operation on two depth maps or one depth map and one 3D geometry, storing the resulting depth map in the specified destination container or image buffer. Note that [`M3dimArith`](../../Reference/3dim/M3dimArith.md) cannot be called with two 3D geometry operands. Alternatively, you can use this function to invert a depth map's Z-values, in which case only a single source depth map is required.

> **Note:** Note that 3D geometries of type [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md) and [`M_POINT`](../../Reference/3dgeo/M3dgeoInquire.md) are not supported for this function.

All image operands must be fully corrected depth maps for this operation. To determine if you have a fully corrected depth map, call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md) and ensure that it returns [`M_TRUE`](../../Reference/cal/McalInquire.md). Note that if the two operands are images, their respective camera calibration information can vary in Z, but not in X or Y.

All operations are performed using the Z-world coordinates corresponding to the gray levels of the image operands or the surface point of a 3D geometry operand. The resulting Z-world coordinates are then converted back to gray levels, according to the camera calibration information of the destination buffer. If the Z-scale and Z-offset of the destination are such that the 3D point falls outside the vertical span of the destination buffer, the destination pixel will be saturated to either 0 or the maximum value of the buffer, minus 1 (since the maximum value of a depth map represents missing data).

Some pixels of the destination buffer can be left unchanged by providing a mask image buffer with the corresponding pixels set to 0.

By default, the destination buffer does not have to be calibrated before calling [`M3dimArith`](../../Reference/3dim/M3dimArith.md). The function will fill the destination buffer and associate it with the necessary camera calibration information. You can, however, specify to use the existing camera calibration information of the destination buffer, by passing [`M_USE_DESTINATION_SCALES`](../../Reference/3dim/M3dimArith.md) to the [`ControlFlag`](../../Reference/3dim/M3dimArith.md) parameter. If the destination is a container and you pass [`M_USE_DESTINATION_SCALES`](../../Reference/3dim/M3dimArith.md) to the [`ControlFlag`](../../Reference/3dim/M3dimArith.md) parameter, the container must be a 3D-processable depth map container.

If either or both operands contain missing data (represented using the maximum value of the depth map buffer), missing data might be written to the destination buffer, depending on the value passed to the [`Operation`](../../Reference/3dim/M3dimArith.md) parameter.

## Parameters

### `Src1ImageBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies the first source container, image buffer, or 3D geometry object.

*For specifying the first source container, image buffer, or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the first operand to be the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the first operand to be a 3D geometry object.

The 3D geometry object must have been previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) and must have been successfully defined.

> **Note:** Note that 3D geometries of type [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md) and [`M_POINT`](../../Reference/3dgeo/M3dgeoInquire.md) are not supported for this function. |
| `Depth map container identifier` | Specifies the first operand to be a depth map container.

The container must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)), and must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). This container must not have a region component. Using a container with a region component will cause an error. |
| `Image buffer identifier` | Specifies the first operand to be an image buffer that contains a fully corrected depth map.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). It must also have the same dimensions and bit depth as the destination buffer ([`DstImageBufId`](../../Reference/3dim/M3dimArith.md)). This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `Src2ImageBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies the second source container, image buffer, or 3D geometry object.

*For specifying the second source container, image buffer, or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the second operand to be the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the second operand to be a 3D geometry object.

The 3D geometry object must have been previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) and must have been successfully defined.

> **Note:** Note that 3D geometries of type [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md) and [`M_POINT`](../../Reference/3dgeo/M3dgeoInquire.md) are not supported for this function. |
| `Depth map container identifier` | Specifies the second operand to be a depth map container.

The container must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)), and must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). This container must not have a region component. Using a container with a region component will cause an error. |
| `Image buffer identifier` | Specifies the second operand to be an image buffer that contains a fully corrected depth map.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). It must also have the same dimensions and bit depth as the destination buffer ([`DstImageBufId`](../../Reference/3dim/M3dimArith.md)). This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

When both operands are depth map image buffers, their calibration information can vary in Z, but not in X and Y. That is, the [`Src2ImageBufOrGeometry3dgeoId`](../../Reference/3dim/M3dimArith.md) parameter must have the same camera calibration information for X and Y as the image buffer specified with [`Src1ImageBufOrGeometry3dgeoId`](../../Reference/3dim/M3dimArith.md). The operation is performed point-to-point; therefore, each respective pixel must represent the same area in the real world in X and Y. |

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container or image buffer in which to put the result of the arithmetic operation. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. If you pass [`M_USE_DESTINATION_SCALES`](../../Reference/3dim/M3dimArith.md) to the [`ControlFlag`](../../Reference/3dim/M3dimArith.md) parameter, the container must be a 3D-processable depth map container (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

### `MaskBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to use as a mask, if needed.

*For specifying the mask identifier*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to write to all pixels in the destination buffer; no mask will be used. |
| `Image buffer identifier` | Specifies the identifier of the image buffer to use as a mask in the arithmetic operation. Pixels in the destination buffer corresponding to pixels set to 0 in the mask buffer are left unchanged, while the other destination pixels will be the result of the arithmetic operation.

The image buffer must be a 1-band, binary, 8-bit, or 16-bit buffer, and must have the same dimensions as the destination buffer ([`DstImageBufId`](../../Reference/3dim/M3dimArith.md)).

This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

*For specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_DIST_NN` | Calculates the absolute difference between the Z-coordinates of corresponding pixels of a depth map and an operand, considering the neighborhood pixels. Typically, this is performed when comparing two depth maps of highly similar objects or features, which are slightly misaligned. **M_DIST_NN()** compensates for the misalignment.

This is a more robust version of the [`M_SUB_ABS`](../../Reference/3dim/M3dimArith.md) operation, which calculates the Z-coordinate difference without considering the neighborhood of pixels.

If either operand is missing data for a certain pixel, the corresponding destination pixel is set to the value for missing data (255, 65535, or 4294967295 for an 8-, 16-, or 32-bit depth map, respectively). |
| `M_DIST_NN_SIGNED` | Calculates the signed difference between the Z-coordinates of corresponding pixels of a depth map and an operand, considering the neighborhood pixels. Typically, this is performed when comparing two depth maps of highly similar objects or features, which are slightly misaligned. **M_DIST_NN_SIGNED()** compensates for the misalignment.

This is a more robust version of the [`M_SUB`](../../Reference/3dim/M3dimArith.md) operation, which calculates the Z-coordinate difference without considering the neighborhood of pixels.

**M_DIST_NN_SIGNED()** is equivalent to **M_DIST_NN()**, except that it returns a signed distance. If operand 1 has a higher Z-coordinate than operand 2, the returned value is positive. Otherwise the returned value is negative.

If either operand is missing data for a certain pixel, the corresponding destination pixel is set to the value for missing data (255, 65535, or 4294967295 for an 8-, 16-, or 32-bit depth map, respectively). |
| `M_ADD` | Adds the Z-coordinate of the first operand to the Z-coordinate of the second operand.

`_Destination_ = (_Operand 1_ + _Operand 2 _)`

If either operand is missing data, the destination is set to the value for missing data. |
| `M_FLIP_Z` | Inverts a depth map's Z values; bright values become dark, and dark values become bright. Note that any missing values are not inverted.

`_Destination_ = (_Maximum value of source buffer_ - _Operand 1 _)`

If _Operand 1 _ is missing data, the destination is set to the value for missing data.

> **Note:** Note that you cannot use a mask when performing this operation. |
| `M_MAX` | Compares the Z-coordinate of the first operand with the Z-coordinate of the second operand, and takes the greater of the two.

`_Destination_ = max(_Operand 1_, _Operand 2 _)`

If both operands are missing data, the destination is set to the value for missing data. If a single operand is missing data, the destination is set to the value of the other operand. |
| `M_MIN` | Compares the Z-coordinate of the first operand with the Z-coordinate of the second operand, and takes the lesser of the two.

`_Destination_ = min(_Operand 1_, _Operand 2 _)`

If both operands are missing data, the destination is set to the value for missing data. If a single operand is missing data, the destination is set to the value of the other operand. |
| `M_REPLACE_MISSING_DATA` | Returns an image that is the same as the first source depth map, except that any missing values are replaced by their corresponding values from the second source.

> **Note:** This operation is available only when the first source is a depth map. |
| `M_SUB` | Subtracts the Z-coordinate of the second operand from the Z-coordinate of the first operand.

`_Destination_ = (_Operand 1_ - _Operand 2 _)`

If either operand is missing data, the destination is set to the value for missing data. |
| `M_SUB_ABS` | Subtracts the Z-coordinate of the second operand from the Z-coordinate of the first operand, and takes the absolute value of the result. This operation is equivalent to taking the absolute value of the [`M_SUB`](../../Reference/3dim/M3dimArith.md) operation.

`_Destination_ = | _Operand 1_ - _Operand 2 _|`

If either operand is missing data, the destination is set to the value for missing data. |
| `M_VALIDITY_MAP` | Returns an image corresponding to the validity of each pixel in the two operands.

| Pixel in operand 1 | Pixel in operand 2 | Returned to corresponding pixel in the destination |
| --- | --- | --- |
| **M_INVALID_POINT** | **M_INVALID_POINT** | 0 | **M_NO_SRC_VALID_LABEL** |
| Valid point | **M_INVALID_POINT** | 85 | **M_ONLY_SRC1_VALID_LABEL** |
| **M_INVALID_POINT** | Valid point | 170 | **M_ONLY_SRC2_VALID_LABEL** |
| Valid point | Valid point | 255 | **M_BOTH_SRC_VALID_LABEL** |

Note that the [`DstImageBufId`](../../Reference/3dim/M3dimArith.md) parameter must be given an image buffer identifier. |

### `SignMode` *(in, AIL_INT64)*

Specifies which Z-coordinate to select for geometries with more than 1 Z-coordinate for a given pair of X- and Y-coordinates. When the two sources are depth maps, or when performing an [`M_FLIP_Z`](../../Reference/3dim/M3dimArith.md) operation, the [`SignMode`](../../Reference/3dim/M3dimArith.md) parameter must be set to [`M_DEFAULT`](../../Reference/3dim/M3dimArith.md).

*For specifying the sign mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX_Z` *(default)* | Specifies to use the surface point with the largest Z-coordinate value. |
| `M_MIN_Z` | Specifies to use the surface point with the smallest Z-coordinate value. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to handle the camera calibration information of the destination image buffer.

*For specifying how to handle the camera calibration information of the destination image buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_FIT_SCALES`](../../Reference/3dim/M3dimArith.md).

This is the only possible value for [`M_FLIP_Z`](../../Reference/3dim/M3dimArith.md) and [`M_VALIDITY_MAP`](../../Reference/3dim/M3dimArith.md). |
| `M_FIT_SCALES` | Specifies to choose the Z-scale and Z-offset so that the values can be represented in the destination image buffer without saturation. This mode depends only on the possible range of the data, and not on the actual source depth map contents.

Note that the X- and Y-scales and offsets of the destination image buffer will be set to the same values as those of the source image(s). After calling [`M3dimArith`](../../Reference/3dim/M3dimArith.md), the destination image buffer will be fully corrected.

> **Note:** Note that in extreme cases, **M_DIST_NN()** and **M_DIST_NN_SIGNED()** can return saturated pixels because the Euclidean distance is calculated and not just the Z-coordinate distance. |
| `M_SET_WORLD_OFFSET_Z` | Specifies to keep the same Z-scale as the first source image and to set the Z-offset so that the destination image buffer Z-range covers the center of the possible range of the data. Since the Z-scale remains unaltered, it is possible for saturation to occur.

The X- and Y-scales and offsets of the destination image buffer will be set to the same values as those of the source image(s). After calling [`M3dimArith`](../../Reference/3dim/M3dimArith.md), the destination image buffer will be fully corrected. |
| `M_USE_DESTINATION_SCALES` | Specifies to use the camera calibration information of the destination image buffer. This means the destination must be fully corrected before calling [`M3dimArith`](../../Reference/3dim/M3dimArith.md), and have the same scales and offsets in X and Y as the source image(s). This can be done by copying the camera calibration information from another image using [`McalAssociate`](../../Reference/cal/McalAssociate.md), or by providing scales and offsets manually using [`McalUniform`](../../Reference/cal/McalUniform.md) or using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md) and [`M_WORLD_POS_Z`](../../Reference/cal/McalControl.md). [`M3dimArith`](../../Reference/3dim/M3dimArith.md) will leave the camera calibration information of the destination image buffer unchanged. |
| `M_USE_SOURCE1_SCALES` | Specifies to use the calibration information of the first source.

> **Note:** This option is not available when the [`Src1ImageBufOrGeometry3dgeoId`](../../Reference/3dim/M3dimArith.md) parameter is set to [`M_XY_PLANE`](../../Reference/3dim/M3dimArith.md). |
| `M_USE_SOURCE2_SCALES` | Specifies to use the calibration information of the second source.

> **Note:** This option is not available when the [`Src2ImageBufOrGeometry3dgeoId`](../../Reference/3dim/M3dimArith.md) parameter is set to [`M_XY_PLANE`](../../Reference/3dim/M3dimArith.md). |
