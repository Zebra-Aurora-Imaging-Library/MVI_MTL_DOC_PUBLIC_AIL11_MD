---
doctype: Reference
module: buf
function: MbufAllocComponent
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufAllocComponent"
---

# MbufAllocComponent

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

> Allocate a buffer and add it as a component of a container.

## Syntax

```c
AIL_ID MbufAllocComponent(
    AIL_ID    ContainerBufId,  //in
    AIL_INT   SizeBand,        //in
    AIL_INT   SizeX,           //in
    AIL_INT   SizeY,           //in
    AIL_INT   Type,            //in
    AIL_INT64 Attribute,       //in
    AIL_INT64 ComponentType,   //in
    AIL_ID *  ComponentIdPtr   //out
)
```

## Description

This function allocates a buffer and adds it as a component of a container. The buffer is allocated on the same system as the container.

> **Note:** Note that typically you do not need to manually allocate components. Aurora Imaging Library typically allocates/reallocates components as required when the container is used as a destination for a grabbing or processing function.

After allocating the component, you should check that the operation was successful (either using [`MappGetError`](../../Reference/app/MappGetError.md), or by verifying that the buffer identifier returned is not Null).

When the component is no longer required, free it using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md) or free its container using [`MbufFree`](../../Reference/buf/MbufFree.md).

> **Note:** Note that in some cases, a component allocated using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md)might be freed and reallocated automatically if its container is the destination for an Aurora Imaging Library function (such as a grabbing or 3D processing function). If the buffer is reallocated, its Aurora Imaging Library identifier will change. If you need to access the component directly using its Aurora Imaging Library buffer identifier, you should inquire its Aurora Imaging Library identifier (using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with [`M_COMPONENT_ID`](../../Reference/buf/MbufInquireContainer.md)) any time its container is passed as the destination of an Aurora Imaging Library function.

## Parameters

### `ContainerBufId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library identifier of the container to which to add the component.

### `SizeBand` *(in, AIL_INT)*

Specifies the number of (x,y) surfaces (also called bands) to allocate to the buffer.

*For specifying the number of surfaces to allocate*

| Value | Description |
| --- | --- |
| `1 <= Value <= 1024` | Specifies the number of bands.

When allocating a buffer to store color, specify 1 band for each color band, or other type of data, that the buffer needs to store (for example, monochrome images require 1 color band; RGB color images require 3 color bands).

This value can be any non-zero integer value up to 1024 for an image buffer, 1 or 3 for an array or LUT buffer, or 1 for a kernel or structuring element buffer. |

### `SizeX` *(in, AIL_INT)*

Specifies the buffer width. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, width is specified in pixels.

### `SizeY` *(in, AIL_INT)*

Specifies the buffer height. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, height is specified in pixels.

### `Type` *(in, AIL_INT)*

Specifies a combination of two values: data type and data depth per band. Specify the depth of the buffer per band as one of the following:

*For the data depth of the component*

| Value | Description |
| --- | --- |
| `1` | Specifies a 1-bit (packed binary) buffer. |
| `8` | Specifies an 8-bit buffer. |
| `16` | Specifies a 16-bit buffer. |
| `32` | Specifies a 32-bit buffer. |
| `M_FLOAT` | Specifies that the type of data is floating-point. |
| `M_SIGNED` | Specifies that the type of data is signed. |
| `M_UNSIGNED` *(default)* | Specifies that the type of data is unsigned. |

*For specifying the data type of the buffer*

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies that the type of data is floating-point. |
| `M_SIGNED` | Specifies that the type of data is signed. |
| `M_UNSIGNED` *(default)* | Specifies that the type of data is unsigned. |

### `Attribute` *(in, AIL_INT64)*

Specifies the buffer usage. Aurora Imaging Library uses this information to determine where to allocate the buffer in physical memory.

*For specifying the buffer usage*

| Value | Description |
| --- | --- |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions. |
| `M_ARRAY` | Specifies a buffer to store array type data. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_LUT` | Specifies a buffer to store lookup table data. |

*For specifying the intended purpose of the image buffer*

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |

*For allocating an overscan region*

| Value | Description |
| --- | --- |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. |

*For specifying the compression type*

| Value | Description |
| --- | --- |
| `M_H264` | Specifies that the buffer will be used to hold H.264 data. This compression type is used when specifying a source of an input or a destination of an output for a sequence operation as an array of buffers ([`MseqDefine`](../../Reference/seq/MseqDefine.md) with [`M_BUFFER_LIST`](../../Reference/seq/MseqDefine.md)). |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufAllocComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocComponent.md), [`M_RGB48`](../../Reference/buf/MbufAllocComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocComponent.md), or [`M_YUV24`](../../Reference/buf/MbufAllocComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocComponent.md) format. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufAllocComponent.md), [`M_RGB48`](../../Reference/buf/MbufAllocComponent.md), [`M_YUV9`](../../Reference/buf/MbufAllocComponent.md), [`M_YUV12`](../../Reference/buf/MbufAllocComponent.md), [`M_YUV16`](../../Reference/buf/MbufAllocComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocComponent.md), or [`M_YUV24`](../../Reference/buf/MbufAllocComponent.md) format. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band data in [`M_RGB24`](../../Reference/buf/MbufAllocComponent.md) or [`M_RGB48`](../../Reference/buf/MbufAllocComponent.md) format. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. The supported data formats are 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_RGB24`](../../Reference/buf/MbufAllocComponent.md), [`M_YUV24`](../../Reference/buf/MbufAllocComponent.md), [`M_YUV12`](../../Reference/buf/MbufAllocComponent.md), [`M_YUV9`](../../Reference/buf/MbufAllocComponent.md), [`M_YUV16`](../../Reference/buf/MbufAllocComponent.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocComponent.md), or [`M_YUV16`](../../Reference/buf/MbufAllocComponent.md) + [`M_PACKED`](../../Reference/buf/MbufAllocComponent.md) format. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_YUV16`](../../Reference/buf/MbufAllocComponent.md) + [`M_PACKED`](../../Reference/buf/MbufAllocComponent.md) format. |

*For specifying a storage format and a location specifier*

| Value | Description |
| --- | --- |
| `M_DIB` | Forces the buffer to be a DIB buffer. |
| `M_GDI` | Forces the buffer to be compatible with GDI. |
| `M_LINUX_MXIMAGE` | Forces the buffer to be an X11 Ximage. |

*For specifying that the buffer is FPGA accessible*

| Value | Description |
| --- | --- |
| `M_FPGA_ACCESSIBLE` | Forces the buffer to be allocated in a bank of memory that is accessible from the Processing FPGA. |

*For specifying a storage format for image buffers*

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |

*For specifying a planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar pixels. |
| `M_YUV12` | Specifies YUV12 planar pixels. |
| `M_YUV24` | Specifies YUV24 planar pixels. |

*For specifying a packed color buffer format*

| Value | Description |
| --- | --- |
| `M_BGR24` | Specifies 24-bit color depth packed pixels (BGRBGR).   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers. |
| `M_BGR32` | Specifies 32-bit color depth packed pixels (BGRXBGRX). The most-significant byte is a "don't care" byte.   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers. |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5). Note that when accessing an [`M_RGB15`](../../Reference/buf/MbufAllocComponent.md) + [`M_PACKED`](../../Reference/buf/MbufAllocComponent.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5). Note that when accessing an [`M_RGB16`](../../Reference/buf/MbufAllocComponent.md) + [`M_PACKED`](../../Reference/buf/MbufAllocComponent.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers. |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md).   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers.

   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers if those buffers are also packed ([`M_PACKED`](../../Reference/buf/MbufAllocComponent.md)). |

*For specifying a packed or planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels.   This value can be used with planar ([`M_PLANAR`](../../Reference/buf/MbufAllocComponent.md)) grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers.

   This value can be used with packed ([`M_PACKED`](../../Reference/buf/MbufAllocComponent.md)) grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). packed or planar pixels.   This value can be used with packed ([`M_PACKED`](../../Reference/buf/MbufAllocComponent.md)) grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers.

   This value can be used with planar ([`M_PLANAR`](../../Reference/buf/MbufAllocComponent.md)) grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers if the buffer is allocated on the Host ([`M_HOST_MEMORY`](../../Reference/buf/MbufAllocComponent.md)) and not on-board. |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 (4:2:2) pixels.   This value can be used with packed ([`M_PACKED`](../../Reference/buf/MbufAllocComponent.md)) grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers. |

*For specifying a location for a buffer*

| Value | Description |
| --- | --- |
| `M_HOST_MEMORY` *(default)* | Forces the buffer in Host memory. |
| `M_OFF_BOARD` | Ensures that the buffer is not in on-board memory.   To ensure that an [`M_OFF_BOARD`](../../Reference/buf/MbufAllocComponent.md) buffer can be used by the DMA transfer section of the board, the buffer must be allocated in non-paged memory ([`M_OFF_BOARD`](../../Reference/buf/MbufAllocComponent.md)+[`M_NON_PAGED`](../../Reference/buf/MbufAllocComponent.md) or [`M_HOST_MEMORY`](../../Reference/buf/MbufAllocComponent.md)+[`M_NON_PAGED`](../../Reference/buf/MbufAllocComponent.md)). |
| `M_ON_BOARD` | Forces the buffer in on-board memory.   By default, all on-board buffers are allocated in memory that is not mapped on the PCI bus. This unshared memory cannot be accessed by the Host or any system other than the one on which it was allocated. See combination values below ([`M_SHARED`](../../Reference/buf/MbufAllocComponent.md)) to change this default behavior.

   This is only available for grab buffers ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)).

   This is only available for image buffers ([`M_IMAGE`](../../Reference/buf/MbufAllocComponent.md)).

   This is only available for image ([`M_IMAGE`](../../Reference/buf/MbufAllocComponent.md)) and LUT buffers ([`M_LUT`](../../Reference/buf/MbufAllocComponent.md)). |

*For specifying a memory bank to be used*

| Value | Description |
| --- | --- |
| `M_MEMORY_BANK_n` | Forces the buffer to be allocated in the specified memory bank.   Valid values for _n_ are between 0 and 6, inclusive. However, not all memory banks are necessarily present on a given system. Typically, if there is more than 1 memory bank, it is NUMA-enabled. Note that, to ensure that you are accessing a NUMA-enabled memory bank, you must specify the[`M_HOST_MEMORY`](../../Reference/buf/MbufAllocComponent.md) combination value.

 To determine the actual number of memory banks available on the Aurora Imaging Library system, use [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MEMORY_BANK_NUM`](../../Reference/app/MappInquireMp.md) and [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MEMORY_BANK_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md). For more information, refer to [NUMA Support](../../UserGuide/C66_Using_AIL_with_multiprocessing_and_under_multithread_systems/S02_Transparent_MultiCore_Use.md).

 Note that this value cannot be used with [`M_COMPRESS`](../../Reference/buf/MbufAllocComponent.md) or [`M_NON_PAGED`](../../Reference/buf/MbufAllocComponent.md).

 Also note that the combination of [`M_HOST_MEMORY`](../../Reference/buf/MbufAllocComponent.md) with this value, in paged memory, is only useful when doing multi-core processing on a Host system.

   Note that this value is not available with the[`M_HOST_MEMORY`](../../Reference/buf/MbufAllocComponent.md) combination value.

   Valid values for _n_ are between 0 and 1, inclusive. Refer to your Hardware-specific Notes for more information about on-board memory. Note that, to assure you are accessing an on-board memory bank, you must specify the[`M_ON_BOARD`](../../Reference/buf/MbufAllocComponent.md) combination value.

 |   |   |   |
| --- | --- | --- |
| Memory bank number | Type of memory | Description |
| 0 | Acquisition (SDRAM) | This is the default memory bank for an on-board buffer created with the [`M_GRAB`](../../Reference/buf/MbufAllocComponent.md) attribute. |

   In this case,_n_ must be set to 0. This is the default memory bank for an on-board buffer created with the [`M_GRAB`](../../Reference/buf/MbufAllocComponent.md) attribute. Refer to your Hardware-specific Notes for more information about on-board memory. Note that, to assure you are accessing an on-board memory bank, you must specify the[`M_ON_BOARD`](../../Reference/buf/MbufAllocComponent.md) combination value. |

*For specifying a location in a specific type of memory*

| Value | Description |
| --- | --- |
| `M_SHARED` | Specifies that the buffer is in shared processing memory. This memory is mapped on the PCI bus. Note that this combination value can only be added to [`M_ON_BOARD`](../../Reference/buf/MbufAllocComponent.md). |

*For specifying the attribute of the specified physical memory type*

| Value | Description |
| --- | --- |
| `M_NON_PAGED` | Forces the buffer in Aurora Imaging Library reserved, non-pageable memory, if possible. The maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufAllocComponent.md)) buffers that can be allocated might be restricted by the total amount of Aurora Imaging Library-reserved, non-paged memory (specified at installation time or using the Aurora Imaging Configurator utility). |
| `M_PAGED` | Forces the buffer in pageable memory. |

### `ComponentType` *(in, AIL_INT64)*

Specifies the component type, which identifies what kind of information the component stores.

*For specifying the type of component*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default component type. The default component type for image buffers is [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md). The default component type for all other buffers is[`M_COMPONENT_UNDEFINED`](../../Reference/buf/MbufAllocComponent.md). |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufAllocComponent.md)component of the container. Coordinates associated with the confidence value 0 are considered invalid data and will not be used by 3D image processing functions.

A confidence component is associated with a range or disparity component in the same container when there are no other range, disparity, or confidence components in the container. If there is more than one range, disparity, and/or confidence component in a container (for example, because a single grab into the container transmitted multiple range and confidence components), you will need to either create a child container which contains only the required components using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md), or free the extra components using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md). |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the column index of each corresponding pixel and will result in a better representation of the camera's lens model when generating X-coordinates.

A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.

> **Note:** Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the row index of each corresponding pixel and will result in a better representation of the camera's lens model when generating Y-coordinates.

A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.

> **Note:** Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 254.

You can use custom component types to identify components which store information that does not fit one of the standard component types. In addition, some cameras transmit components with custom component types. Refer to your camera manual to determine what type of information is stored in transmitted components. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. Each pixel of a disparity map indicates the apparent distance (typically measured in pixel units) between where an object appears in the left and right images captured by a stereoscopic camera. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies that the component stores a 4x4 matrix that transforms depth map coordinates. Only depth map containers can have a matrix component.

A matrix component is associated with a range component in the same container when there is only one range component and no other matrix components in that container. |
| `M_COMPONENT_MESH_AIL` | Specifies that the component stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md)component of the container.

A mesh component is associated with a range component in the same container when there are no other range or mesh components in that container. A point cloud container which has a mesh component is referred to as a meshed point cloud container. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. Metadata components are used by Aurora Imaging Library internally. Typically, you should ignore metadata components in your application. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. Unlike an intensity component, a multispectral component might include information about non-visible light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the component stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md)component of the container.

A normals component is associated with a range component in the same container when there are no other range or normals components in that container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. This can be either a 1-band buffer that stores a depth map, or a 3-band buffer that stores coordinates of 3D points. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. Each pixel of a reflectance map indicates how much of the light hitting an object at that location is reflected back. Typically, this is an intensity image of the light spectrum used by a 3D sensor to detect 3D distance/position information. Typically, if the map was generated by a laser profiler, each row indicates the detected intensity of the laser for a single scan. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component of the container. 3D image processing functions will only operate on points inside the ROI. Only depth map containers can have a region component.

A region component is associated with a range component in the same container when there is only one range component and no other region components in that container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. Each pixel of a scatter map indicates how much of the light hitting an object at that location is detected scattering beneath the object's surface. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component stores information of an unknown type. |

### `ComponentIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the buffer identifier. Since the [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md) function also returns the buffer identifier, you can set this parameter to `M_NULL`. If allocation fails, [`M_NULL`](../../Reference/buf/MbufAllocComponent.md) is written as the identifier.

## Return Value

**Type:** `AIL_ID`

The returned value is the buffer identifier. If allocation fails, `M_NULL` is returned.

## Remarks

> If you are creating a DLL that includes a call to [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md), ensure that the call is not made from the `DllMain()` function, because [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md) might load a required DLL and you cannot load a DLL from `DllMain()`. If necessary, call [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md) from an initialization function in your DLL instead.

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAllocComponent.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

> You can allocate a buffer with an [`M_KERNEL`](../../Reference/buf/MbufAllocComponent.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAllocComponent.md) attribute with Aurora Imaging Library Lite. However, these attributes are not required to work under Aurora Imaging Library Lite.
