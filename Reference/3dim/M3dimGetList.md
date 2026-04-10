---
doctype: Reference
module: 3dim
function: M3dimGetList
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimGetList"
---

# M3dimGetList

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

> Read the values in a source container or depth map image, at the specified positions, and place the values in the specified destination arrays.

## Syntax

```c
AIL_INT M3dimGetList(
    AIL_ID             ContainerOrImageBufId,      //in
    AIL_INT64          Component,                  //in
    AIL_INT            SrcArraySize,               //in
    const AIL_DOUBLE * SrcPixelXArrayPtr,          //in
    const AIL_DOUBLE * SrcPixelYArrayPtr,          //in
    AIL_INT64          Options,                    //in
    AIL_DOUBLE *       DstCoord1OrPackedArrayPtr,  //out
    AIL_DOUBLE *       DstCoord2ArrayPtr,          //out
    AIL_DOUBLE *       DstCoord3ArrayPtr,          //out
    AIL_INT32 *        IndexArrayPtr               //out
)
```

## Description

This function reads the values in a source container or depth map image buffer, at the specified positions, and places the values in the specified destination arrays. If the source is a container, you must specify from which component to copy the data. Each X/Y pair listed in the source arrays specifies the position of a point in its container's 2D grid organization (or pixel location in a depth map). Unlike [`MbufGetList`](../../Reference/buf/MbufGetList.md), this function has options for casting, storing the data in packed or planar format, and handling invalid points. It also returns the number of points written to the arrays.

Note that the source array(s) can specify positions with subpixel precision. In this case, the source buffer is typically an organized point cloud. To establish values for the destination array(s), you can specify to interpolate between neighboring values that surround each subpixel position in the container's specified component.

To determine the size of the arrays required to store the requested values, call this function and set [`DstCoord1OrPackedArrayPtr`](../../Reference/3dim/M3dimGetList.md), [`DstCoord2ArrayPtr`](../../Reference/3dim/M3dimGetList.md) and [`DstCoord3ArrayPtr`](../../Reference/3dim/M3dimGetList.md) to [`M_NULL`](../../Reference/3dim/M3dimGetList.md). The function will return the required array size(s).

## Parameters

### `ContainerOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source point cloud container, depth map container, or depth map image buffer.

### `Component` *(in, AIL_INT64)*

Specifies the component from which to get point information. The specified component must be the same size as that of [`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGetList.md), and must be an 8-bit, 16-bit or 32-bit image buffer. If the required component is not the same size, use [`MbufGetList`](../../Reference/buf/MbufGetList.md) instead.

*For specifying a component by its identifier or index*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_ID` | Specifies the component, in the container, that has the specified buffer identifier. |
| `M_COMPONENT_BY_INDEX` | Specifies the component, in the container, at the specified index. |

*For specifying a component by its component type*

| Value | Description |
| --- | --- |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the column index of each corresponding pixel and will result in a better representation of the camera's lens model when generating X-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the row index of each corresponding pixel and will result in a better representation of the camera's lens model when generating Y-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 254.You can use custom component types to identify components which store information that does not fit one of the standard component types. In addition, some cameras transmit components with custom component types. Refer to your camera manual to determine what type of information is stored in transmitted components. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. Each pixel of a disparity map indicates the apparent distance (typically measured in pixel units) between where an object appears in the left and right images captured by a stereoscopic camera. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. Metadata components are used by Aurora Imaging Library internally. Typically, you should ignore metadata components in your application. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. Unlike an intensity component, a multispectral component might include information about non-visible light. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. Each pixel of a reflectance map indicates how much of the light hitting an object at that location is reflected back. Typically, this is an intensity image of the light spectrum used by a 3D sensor to detect 3D distance/position information. Typically, if the map was generated by a laser profiler, each row indicates the detected intensity of the laser for a single scan. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGetList.md) component of the container. 3D image processing functions will only operate on points inside the ROI. Only depth map containers can have a region component.A region component is associated with a range component in the same container when there is only one range component and no other region components in that container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. Each pixel of a scatter map indicates how much of the light hitting an object at that location is detected scattering beneath the object's surface. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component stores information of an unknown type. |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGetList.md) component of the container. Coordinates associated with the confidence value 0 are considered invalid data and will not be used by 3D image processing functions. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the component stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGetList.md)component of the container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. This is a 3-band buffer that stores coordinates of 3D points. |

### `SrcArraySize` *(in, AIL_INT)*

Specifies the number of values in the source array(s).

### `SrcPixelXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of an array containing the X-positions of values to get. Each position is that of a point in its container's 2D grid organization (or pixel location in a depth map). Positions can be specified with subpixel precision.

### `SrcPixelYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of an array containing the Y-positions of values to get. Each position is that of a point in its container's 2D grid organization (or pixel location in a depth map). Positions can be specified with subpixel precision.

### `Options` *(in, AIL_INT64)*

Specifies additional options.

*For specifying default options*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_PLANAR`](../../Reference/3dim/M3dimGetList.md) + [`M_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimGetList.md) + [`M_SKIP_INVALID_POINTS`](../../Reference/3dim/M3dimGetList.md). |

*For specifying the storage format*

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies to store the data in a packed format. |
| `M_PLANAR` *(default)* | Specifies to store the data in a planar format. |

*For specifying how to interpret subpixel positions in the source array(s)*

| Value | Description |
| --- | --- |
| `M_BILINEAR` | Specifies bilinear interpolation. The value placed in the user-defined array is determined by taking a weighted average of the 4 values (2x2) that surround the subpixel position.

> **Note:** If any of the 4 surrounding values is invalid, [`M_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimGetList.md) is used instead. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. The value placed in the user-defined array is that of the value of the pixel closest to the subpixel position. |

*For specifying how to deal with invalid points*

| Value | Description |
| --- | --- |
| `M_INCLUDE_INVALID_POINTS` | Specifies to include invalid points. If a point is invalid, the function writes an unspecified value in the data output array(s) and -1 in the source-index array ([`IndexArrayPtr`](../../Reference/3dim/M3dimGetList.md)). For example, if the input confidence = [1, 0, 1, 0, 1], and the input component = [3, 1, 4, 1, 5], then the data output array = [3, ?, 4, ?, 5], with indices = [0, -1, 2, -1, 4]. |
| `M_SKIP_INVALID_POINTS` *(default)* | Specifies to skip invalid points so that the output arrays contain only valid points. For example, if the input confidence = [1, 0, 1, 0, 1], and the input component = [3, 1, 4, 1, 5], then the data output array = [3, 4, 5], with indices = [0, 2, 4]. |

### `DstCoord1OrPackedArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of a user-defined array in which to store the retrieved data. This array can hold the values of the specified point cloud or depth map's component, or a packed array of all the data. For instance, the range component's 3 bands of coordinate data, if packed, is stored in the array in an interleaved manner (for example, XYZ XYZ XYZ...).

### `DstCoord2ArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of a user-defined array in which to store the retrieved data. If the component is 3-band and [`Options`](../../Reference/3dim/M3dimGetList.md) is set to [`M_PLANAR`](../../Reference/3dim/M3dimGetList.md), the pointer passed to [`DstCoord2ArrayPtr`](../../Reference/3dim/M3dimGetList.md) must be valid. If the component is 1-band or [`Options`](../../Reference/3dim/M3dimGetList.md) is set to [`M_PACKED`](../../Reference/3dim/M3dimGetList.md), set [`DstCoord2ArrayPtr`](../../Reference/3dim/M3dimGetList.md) to `M_NULL`.

### `DstCoord3ArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of a user-defined array in which to store the retrieved data. If the component is 3-band and [`Options`](../../Reference/3dim/M3dimGetList.md) is set to [`M_PLANAR`](../../Reference/3dim/M3dimGetList.md), the pointer passed to [`DstCoord3ArrayPtr`](../../Reference/3dim/M3dimGetList.md) must be valid. If the component is 1-band or [`Options`](../../Reference/3dim/M3dimGetList.md) is set to [`M_PACKED`](../../Reference/3dim/M3dimGetList.md), set [`DstCoord3ArrayPtr`](../../Reference/3dim/M3dimGetList.md) to `M_NULL`.

### `IndexArrayPtr` *(out, *AIL_INT32)*

Specifies the address of a user-defined array in which to store either the index of the element in the input array(s), or -1 for elements that correspond to invalid points.

## Return Value

**Type:** `AIL_INT`

The returned value is the number of entries written into the destination array(s). If [`DstCoord1OrPackedArrayPtr`](../../Reference/3dim/M3dimGetList.md), [`DstCoord2ArrayPtr`](../../Reference/3dim/M3dimGetList.md) and [`DstCoord3ArrayPtr`](../../Reference/3dim/M3dimGetList.md) are set to `M_NULL`, [`M3dimGetList`](../../Reference/3dim/M3dimGetList.md) returns the required size of the array(s) for subsequent calls to the function.

> **Note:** Note that you can specify an array of the required type; Aurora Imaging Library automatically casts the data into the appropriate type.
