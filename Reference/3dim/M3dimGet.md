---
doctype: Reference
module: 3dim
function: M3dimGet
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimGet"
---

# M3dimGet

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

> Get valid points or other component values from a container's point cloud or depth map, or get values from a depth map image.

## Syntax

```c
AIL_INT M3dimGet(
    AIL_ID       ContainerOrImageBufId,      //in
    AIL_INT64    Component,                  //in
    AIL_INT      ArraySize,                  //in
    AIL_INT64    Options,                    //in
    AIL_DOUBLE * DstCoord1OrPackedArrayPtr,  //out
    AIL_DOUBLE * DstCoord2ArrayPtr,          //out
    AIL_DOUBLE * DstCoord3ArrayPtr           //out
)
```

## Description

This function returns a component's values that correspond to valid points in the source point cloud or depth map container, or it returns valid values in a depth map image, excluding invalid points and points outside a region of interest (if the depth map image buffer is associated with an ROI).

When copying from a floating-point buffer to an integer array, the values are truncated. If the source buffer depth is greater than that of the destination array, the most significant bits are truncated when the data is copied into the destination.

For a depth map image, you can limit this function's results to a region of the depth map image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

To get all the coordinates stored in a component, regardless of the corresponding confidence score, use [`MbufGet`](../../Reference/buf/MbufGet.md).

## Parameters

### `ContainerOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source point cloud container, depth map container, or depth map image buffer.

### `Component` *(in, AIL_INT64)*

Specifies the component from which to get values. The specified component must be of the same size as that of [`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGet.md), and must be an 8-bit, 16-bit or 32-bit image buffer. If the required component is not the same size, use [`MbufGet`](../../Reference/buf/MbufGet.md) instead.

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
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGet.md) component of the container. 3D image processing functions will only operate on points inside the ROI. Only depth map containers can have a region component.A region component is associated with a range component in the same container when there is only one range component and no other region components in that container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. Each pixel of a scatter map indicates how much of the light hitting an object at that location is detected scattering beneath the object's surface. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component stores information of an unknown type. |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGet.md) component of the container. Coordinates associated with the confidence value 0 are considered invalid data and will not be used by 3D image processing functions. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the component stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/3dim/M3dimGet.md)component of the container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D position information. This is a 3-band buffer that stores coordinates of 3D points. |

### `ArraySize` *(in, AIL_INT)*

Specifies the maximum number of elements that the arrays can hold.

### `Options` *(in, AIL_INT64)*

Specifies additional options.

*For specifying the storage format*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PACKED` | Specifies to store the component data in a packed format. |
| `M_PLANAR` *(default)* | Specifies to store the component data in a planar format. |

### `DstCoord1OrPackedArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the user array in which to store the retrieved data. This array can hold the values of the specified point cloud or depth map's component, or a packed array of all the data. For instance, a depth map's 3 bands of coordinate data, if packed, is stored in the array in an interleaved manner (for example, XYZ XYZ XYZ...).

### `DstCoord2ArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the user array in which to store the retrieved data. If the component is 3-band and [`Options`](../../Reference/3dim/M3dimGet.md) is set to [`M_PLANAR`](../../Reference/3dim/M3dimGet.md), the pointer passed to [`DstCoord2ArrayPtr`](../../Reference/3dim/M3dimGet.md) must be valid. If the component is 1-band or [`Options`](../../Reference/3dim/M3dimGet.md) is set to [`M_PACKED`](../../Reference/3dim/M3dimGet.md), set [`DstCoord2ArrayPtr`](../../Reference/3dim/M3dimGet.md) to `M_NULL`.

### `DstCoord3ArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the user array in which to store the retrieved data. If the component is 3-band and [`Options`](../../Reference/3dim/M3dimGet.md) is set to [`M_PLANAR`](../../Reference/3dim/M3dimGet.md), the pointer passed to [`DstCoord3ArrayPtr`](../../Reference/3dim/M3dimGet.md) must be valid. If the component is 1-band or [`Options`](../../Reference/3dim/M3dimGet.md) is set to [`M_PACKED`](../../Reference/3dim/M3dimGet.md), set [`DstCoord3ArrayPtr`](../../Reference/3dim/M3dimGet.md) to `M_NULL`.

## Return Value

**Type:** `AIL_INT`

The returned value is the number of entries written into the array(s). If [`DstCoord1OrPackedArrayPtr`](../../Reference/3dim/M3dimGet.md), [`DstCoord2ArrayPtr`](../../Reference/3dim/M3dimGet.md) and [`DstCoord3ArrayPtr`](../../Reference/3dim/M3dimGet.md) are set to `M_NULL`, [`M3dimGet`](../../Reference/3dim/M3dimGet.md) returns the required size of the array(s) for subsequent calls to the function.
