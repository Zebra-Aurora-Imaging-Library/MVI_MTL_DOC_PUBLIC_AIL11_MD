---
doctype: Reference
module: buf
function: MbufCreateComponent
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufCreateComponent"
---

# MbufCreateComponent

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Create a component in the specified container.

## Syntax

```c
AIL_ID MbufCreateComponent(
    AIL_ID    ContainerBufId,  //in
    AIL_INT   SizeBand,        //in
    AIL_INT   SizeX,           //in
    AIL_INT   SizeY,           //in
    AIL_INT   Type,            //in
    AIL_INT64 Attribute,       //in
    AIL_INT64 ControlFlag,     //in
    AIL_INT   Pitch,           //in
    void **   ArrayOfDataPtr,  //in
    AIL_INT64 ComponentType,   //in
    AIL_ID *  ComponentIdPtr   //out
)
```

## Description

This function creates a data buffer using memory at a specified location, and makes it a component of a specified container. The buffer is allocated on the same system as the container.

> **Note:** This function should be used with caution because, when using physical addresses, it provides direct access to any of your computer's memory mapped devices; when using logical addresses, memory protection errors could result.

Typically, you should leave buffer allocation, data loading, and memory control to Aurora Imaging Library ([`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md), [`MbufGetColor`](../../Reference/buf/MbufGetColor.md), [`MbufPutColor`](../../Reference/buf/MbufPutColor.md)), since Aurora Imaging Library might require special memory attributes (such as non-paged memory) or alignment to associate the buffer component with the same system as the container.

You must allocate the appropriate memory before calling [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md)(except when [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufCreateComponent.md)is specified), and you must free the created buffer when no longer required, either using[`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md)or by freeing its container using [`MbufFree`](../../Reference/buf/MbufFree.md), before freeing the memory. Freeing a buffer created with [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md) will free the internal structure required for a mapped buffer, but it will not free the memory used.

If creating a 3-band buffer, be as specific as possible when setting the buffer attributes (using the [`Attribute`](../../Reference/buf/MbufCreateComponent.md) parameter) and make sure to specify the color format of the buffer. The more information known about the associated buffer, the better the results.

### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Note that to create a buffer mapped to on-board memory, it must be shared memory. See your _Hardware-specific Notes_ for more information regarding shared memory. |

## Parameters

### `ContainerBufId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library identifier of the container in which to create the buffer component. The buffer will be allocated on the same system as the container.

### `SizeBand` *(in, AIL_INT)*

Specifies the number of (x,y) surfaces (also called bands) that the buffer should have to contain each color or other type of data.

*For specifying the number of surfaces*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the same number of bands as the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateComponent.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateComponent.md). |
| `1 <= Value <= 1024` | Specifies the number of bands. There are generally either 1 or 3 bands.

When acquiring or processing monochrome images, the buffer requires only 1 band. For RGB color images, it requires 3 color bands.

This value can be any non-zero integer value up to 1024 for an image buffer, 1 or 3 for an array or LUT buffer, or 1 for a kernel or structuring element buffer. |

### `SizeX` *(in, AIL_INT)*

Specifies the buffer width.

*For the buffer width*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the buffer width is identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateComponent.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateComponent.md). |
| `Value` | Specifies the buffer width. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, width is specified in pixels. |

### `SizeY` *(in, AIL_INT)*

Specifies the buffer height.

*For the buffer height*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the buffer height is identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateComponent.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateComponent.md). |
| `Value` | Specifies the buffer height. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, height is specified in pixels. |

### `Type` *(in, AIL_INT)*

Specifies a combination of two values: data depth and data type, per band. Specify the depth of the buffer per band as one of the following:

*For the depth of the buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the data depth and type are identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateComponent.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateComponent.md). |
| `1` | Specifies a 1-bit (packed binary) buffer. Note that you cannot create a 1-bit LUT buffer. |
| `8` | Specifies an 8-bit buffer. |
| `16` | Specifies a 16-bit buffer. |
| `32` | Specifies a 32-bit buffer. |

*For specifying the data type*

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies that the type of data is floating-point. The valid data depth is 32 bits. |
| `M_SIGNED` | Specifies that the type of data is signed. The valid data depths are 8, 16, or 32 bits. |
| `M_UNSIGNED` *(default)* | Specifies that the type of data is unsigned. The valid data depths are 1, 8, 16, or 32 bits. |

### `Attribute` *(in, AIL_INT64)*

Specifies the buffer usage. This allows you to specify the type of buffer, compression type, storage format, data direction, location, internal storage format, memory type, and overscan region of the created buffer; all of which provides additional information for other Aurora Imaging Library functions.

*For specifying the buffer usage*

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies a buffer to store array type data. Note that some functions take an [`M_ARRAY`](../../Reference/buf/MbufCreateComponent.md) buffer rather than a user-defined array.

Note that this attribute is only supported for 1-band and 3-band buffers. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions.

The depth must be 8, 16, or 32-bit integer or floating-point.

Note that this attribute is only supported for 1-band buffers. |
| `M_LUT` | Specifies a buffer to store lookup table data.

The depth must be 8, 16, or 32-bit integer or floating-point and the internal storage format must be planar ([`M_PLANAR`](../../Reference/buf/MbufCreateComponent.md)).

Note that this attribute is only supported for 1-band and 3-band buffers. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions.

The data depth must be 32-bit. If signed, the range is -32768 to +32767. If unsigned, the range is 0 to +32767. Note that the data can be set to **M_DONT_CARE** to ignore specific structuring elements.

Note that this attribute is only supported for 1-band buffers. |

*For specifying the intended purpose of the image buffer*

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. Note that a buffer with this attribute cannot have the [`M_SIGNED`](../../Reference/buf/MbufCreateComponent.md) data type.

> **Note:** If [`M_COMPRESS`](../../Reference/buf/MbufCreateComponent.md) is specified, the underlying data must already be compressed with the specified compression type and color format. The [`SizeBand`](../../Reference/buf/MbufCreateComponent.md), [`Type`](../../Reference/buf/MbufCreateComponent.md) [`SizeX`](../../Reference/buf/MbufCreateComponent.md)and[`SizeY`](../../Reference/buf/MbufCreateComponent.md)parameters must also be set to the exact size of the compressed image stored in the underlying data. It is not possible to create a compressed buffer mapped on to only part of a compressed image.

When mapping buffers for operations that require a source and destination buffer, and one of the buffers has an [`M_COMPRESS`](../../Reference/buf/MbufCreateComponent.md) attribute, the data will be compressed or decompressed depending on the attributes of the destination buffer. If both the source and destination buffers have [`M_COMPRESS`](../../Reference/buf/MbufCreateComponent.md) specifiers but different compression types, the data will be re-compressed according to the settings of the destination buffer.

> **Note:** Note that buffers created from preallocated memory with the[`M_COMPRESS`](../../Reference/buf/MbufCreateComponent.md) attribute should typically only be passed to Aurora Imaging Library functions as a source. If you pass a compressed buffer created from preallocated memory as a destination for an Aurora Imaging Library function, an error will be generated if the compressed size in memory of the function output is not the same as the size of the allocated memory. This is true even if the [`ControlFlag`](../../Reference/buf/MbufCreateComponent.md)parameter is set to[`M_AIL_ID`](../../Reference/buf/MbufCreateComponent.md).

Only 1-band and 3-band buffers can have an [`M_COMPRESS`](../../Reference/buf/MbufCreateComponent.md)attribute. |

*For specifying the compression type*

| Value | Description |
| --- | --- |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufCreateComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateComponent.md), [`M_RGB48`](../../Reference/buf/MbufCreateComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateComponent.md), or[`M_YUV24`](../../Reference/buf/MbufCreateComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateComponent.md) format. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufCreateComponent.md), [`M_RGB48`](../../Reference/buf/MbufCreateComponent.md), [`M_YUV9`](../../Reference/buf/MbufCreateComponent.md), [`M_YUV12`](../../Reference/buf/MbufCreateComponent.md), [`M_YUV16`](../../Reference/buf/MbufCreateComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateComponent.md), or [`M_YUV24`](../../Reference/buf/MbufCreateComponent.md) format. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band data in [`M_RGB24`](../../Reference/buf/MbufCreateComponent.md) or [`M_RGB48`](../../Reference/buf/MbufCreateComponent.md) format. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_RGB24`](../../Reference/buf/MbufCreateComponent.md), [`M_YUV24`](../../Reference/buf/MbufCreateComponent.md), [`M_YUV12`](../../Reference/buf/MbufCreateComponent.md), [`M_YUV9`](../../Reference/buf/MbufCreateComponent.md), [`M_YUV16`](../../Reference/buf/MbufCreateComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateComponent.md), or [`M_YUV16`](../../Reference/buf/MbufCreateComponent.md) + [`M_PACKED`](../../Reference/buf/MbufCreateComponent.md) format. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_YUV16`](../../Reference/buf/MbufCreateComponent.md) + [`M_PACKED`](../../Reference/buf/MbufCreateComponent.md) format. |

*For specifying a storage format for image buffers*

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). Typically, packed buffers are used for display. Note that it might be slower to process buffers with the [`M_PACKED`](../../Reference/buf/MbufCreateComponent.md) attribute. Also, note that you cannot allocate a packed LUT buffer. |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. Typically, planar buffers are used for processing. |

*For specifying a planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar standard. |
| `M_YUV12` | Specifies YUV12 planar standard. |
| `M_YUV24` | Specifies YUV24 planar standard. |

*For specifying a packed color buffer format*

| Value | Description |
| --- | --- |
| `M_BGR24` | Specifies 24-bit color depth (BGR) packed pixels (BGRBGR). |
| `M_BGR32` | Specifies 32-bit color depth (BGR) packed pixels (BGRXBGRX). The most-significant byte is a "don't care" byte. |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5).

Note that when accessing an [`M_RGB15`](../../Reference/buf/MbufCreateComponent.md) + [`M_PACKED`](../../Reference/buf/MbufCreateComponent.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5).

Note that when accessing an [`M_RGB16`](../../Reference/buf/MbufCreateComponent.md) + [`M_PACKED`](../../Reference/buf/MbufCreateComponent.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |

*For specifying a packed or planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16) planar pixels. |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 packed or planar (4:2:2) standard. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the physical nature of the buffer. This parameter can be set to one of the values below.

*For specifying the physical nature of the buffer*

| Value | Description |
| --- | --- |
| `M_AIL_ID` | Specifies that the new buffer maps to an existing source buffer. [`ArrayOfDataPtr`](../../Reference/buf/MbufCreateComponent.md) is a pointer to an array that stores a pointer to the Aurora Imaging Library identifier of the source buffer. |
| `M_HOST_ADDRESS` | Specifies that [`ArrayOfDataPtr`](../../Reference/buf/MbufCreateComponent.md)is a pointer to an array of host addresses that point to the data. The number of pointers in the array depends on the number of bands and whether the data is compressed or packed.

Note that when using logical addresses, memory protection errors could result.

Note that you can use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md) to get the logical address of an Aurora Imaging Library allocated buffer. |
| `M_PHYSICAL_ADDRESS` | Specifies that [`ArrayOfDataPtr`](../../Reference/buf/MbufCreateComponent.md)is a pointer to an array of physical addresses that point to the data. The number of pointers in the array depends on the number of bands and whether the data is compressed or packed.

Note that using physical addresses provides direct access to any of your computer's memory mapped devices.

Note that you can use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md) to get the physical address of an Aurora Imaging Library allocated buffer. |

*For specifying how the pitch is measured*

| Value | Description |
| --- | --- |
| `M_PITCH` | Specifies that the pitch is in pixels. |
| `M_PITCH_BYTE` | Specifies that the pitch is in bytes. |

### `Pitch` *(in, AIL_INT)*

Specifies the size of the pitch. The pitch is the number of pixels or bytes (as specified by the [`ControlFlag`](../../Reference/buf/MbufCreateComponent.md) parameter) between the start of any two adjacent rows of the buffer. The actual length of a row in the buffer, in physical memory, might be different from the value of the [`SizeX`](../../Reference/buf/MbufCreateComponent.md) parameter (for example, due to use of buffer overscan).

*For specifying the size of the pitch*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that when dealing with a 1-bit buffer, the pitch is a multiple of 4 bytes; otherwise the pitch equals the [`SizeX`](../../Reference/buf/MbufCreateComponent.md) parameter. |
| `Value` | Specifies the pitch in pixels or bytes (as determined by the [`ControlFlag`](../../Reference/buf/MbufCreateComponent.md) parameter). |

### `ArrayOfDataPtr` *(in, **void)*

Specifies one or more data pointers that address the memory to which to map the created Aurora Imaging Library buffer.

### `ComponentType` *(in, AIL_INT64)*

Specifies the component type, which identifies what kind of information the buffer component stores.

*For specifying the type of component*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default component type. The default component type for image buffers is [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufCreateComponent.md). The default component type for all other buffers is[`M_COMPONENT_UNDEFINED`](../../Reference/buf/MbufCreateComponent.md). |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufCreateComponent.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufCreateComponent.md)component of the container. Coordinates associated with the confidence value 0 are considered invalid data and will not be used by 3D image processing functions.A confidence component is associated with a range or disparity component in the same container when there are no other range, disparity, or confidence components in the container. If there is more than one range, disparity, and/or confidence component in a container (for example, because a single grab into the container transmitted multiple range and confidence components), you will need to either create a child container which contains only the required components using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md), or free the extra components using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md). |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the column index of each corresponding pixel and will result in a better representation of the camera's lens model when generating X-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the row index of each corresponding pixel and will result in a better representation of the camera's lens model when generating Y-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 254.You can use custom component types to identify components which store information that does not fit one of the standard component types. In addition, some cameras transmit components with custom component types. Refer to your camera manual to determine what type of information is stored in transmitted components. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. Each pixel of a disparity map indicates the apparent distance (typically measured in pixel units) between where an object appears in the left and right images captured by a stereoscopic camera. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies that the component stores a 4x4 matrix that transforms depth map coordinates. Only depth map containers can have a matrix component.A matrix component is associated with a range component in the same container when there is only one range component and no other matrix components in that container. |
| `M_COMPONENT_MESH_AIL` | Specifies that the component stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufCreateComponent.md)component of the container.A mesh component is associated with a range component in the same container when there are no other range or mesh components in that container. A point cloud container which has a mesh component is referred to as a meshed point cloud container. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. Metadata components are used by Aurora Imaging Library internally. Typically, you should ignore metadata components in your application. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. Unlike an intensity component, a multispectral component might include information about non-visible light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the component stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufCreateComponent.md)component of the container.A normals component is associated with a range component in the same container when there are no other range or normals components in that container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. This can be either a 1-band buffer that stores a depth map, or a 3-band buffer that stores coordinates of 3D points. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. Each pixel of a reflectance map indicates how much of the light hitting an object at that location is reflected back. Typically, this is an intensity image of the light spectrum used by a 3D sensor to detect 3D distance/position information. Typically, if the map was generated by a laser profiler, each row indicates the detected intensity of the laser for a single scan. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufCreateComponent.md) component of the container. 3D image processing functions will only operate on points inside the ROI. Only depth map containers can have a region component.A region component is associated with a range component in the same container when there is only one range component and no other region components in that container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. Each pixel of a scatter map indicates how much of the light hitting an object at that location is detected scattering beneath the object's surface. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component stores information of an unknown type. |

### `ComponentIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the buffer identifier. Since the [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md) function also returns the buffer identifier, you can set this parameter to `M_NULL`. If mapping fails, [`M_NULL`](../../Reference/buf/MbufCreateComponent.md) is written as the identifier.

## Return Value

**Type:** `AIL_ID`

The returned value is the buffer identifier. If allocation fails, `M_NULL` is returned.

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

> You can allocate an image buffer with an [`M_KERNEL`](../../Reference/buf/MbufCreateComponent.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufCreateComponent.md) attribute with Aurora Imaging Library Lite. However, these attributes are not required to work under Aurora Imaging Library Lite.
