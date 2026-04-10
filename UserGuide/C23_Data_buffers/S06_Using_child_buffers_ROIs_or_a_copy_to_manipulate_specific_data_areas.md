---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Using child buffers ROIs or a copy to manipulate specific data areas"
---

# Using child buffers, ROIs, or a copy to manipulate specific data areas

You can manipulate or control specific parts of a data buffer by creating a child buffer within it, by specifying a region of interest, or by copying specific parts of it to another buffer.

## Child buffers

A child buffer is a specified subset of a given data buffer (known as the parent buffer), associated with an Aurora Imaging Library identifier. Child buffers occupy a specific rectangular area of the parent buffer. Since this area is part of the same physical memory space as the parent buffer, changes made to the data of the child buffer affect the data of the parent buffer and vice versa. Note that a buffer which has no parent is referred to as the ancestor buffer of itself, its child buffers, and all other descendants.

Since a child buffer has an Aurora Imaging Library identifier, it is considered a data buffer, and it can be used in the same context as its parent buffer; for example, it can be selected to an Aurora Imaging Library 2D display and be the only data that is displayed. A child buffer takes on the same attributes and type as the parent buffer. However, any pixel coordinates specified or returned when using a child buffer are relative to the child buffer's top-left corner.

One major benefit of the child buffer is being able to handle several buffers simultaneously, in contexts where normally only one buffer can be handled. For example, an Aurora Imaging Library 2D display can only display one image buffer at a time. However, you might want to display the source and destination buffer of an operation in the same window. You can get around this situation by allocating a displayable image buffer as large as the 2D display and then allocating two child buffers from this buffer. You can then use one as the source data buffer and one as the destination. When the parent buffer is selected on the 2D display ([`MdispSelect`](../../Reference/disp/MdispSelect.md)), both the source and the destination child buffers can be seen.

When the parent buffer is multi-band, the child buffer can occupy one or all bands. The following example shows some child buffers that can be created from a multi-band image parent buffer.

*[Image: child_images.png]*

Just as its parent buffer, a child buffer must be allocated so that it can be associated with an identifier and recognized as an entity by Aurora Imaging Library. You can use one of the Aurora Imaging Library functions from the table below to allocate the child buffer.

| Aurora Imaging Library function | Monochrome parent buffer | Multi-band parent buffer |
| --- | --- | --- |
| [`MbufChild1d`](../../Reference/buf/MbufChild1d.md) | 1D region of first row of band. | 1D region of first row of all bands. |
| [`MbufChild2d`](../../Reference/buf/MbufChild2d.md) | 2D region of band. | 2D region of all bands. |
| [`MbufChildColor`](../../Reference/buf/MbufChildColor.md) | - | Entire area of band chosen. |
| [`MbufChildColor2d`](../../Reference/buf/MbufChildColor2d.md) | - | 2D region of band chosen or 2D region of all bands. |

Allocate a child buffer by specifying its size and offset with respect to each of the parent buffer dimensions. Note that, as a subset of the parent buffer, a child buffer cannot exceed the bounds of its parent in any dimension. For example, a color child buffer cannot be created from a monochrome parent buffer.

To inquire how many child buffers are associated with a given parent buffer, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_NB_CHILD`](../../Reference/buf/MbufInquire.md).

Once you have finished using a child buffer, you must free it using [`MbufFree`](../../Reference/buf/MbufFree.md), before freeing the parent buffer.

## Specifying a region of interest

A region of interest (ROI) is similar to a child buffer in that it identifies a subset of data in an image buffer. The ROI of an image buffer can be of any shape and can be composed of several non-contiguous areas. When dealing with processing operations that respect the ROI, all pixels outside the ROI are ignored. You can have the ROI move within the image buffer according to the relative coordinate system if the image is calibrated. Note that, unlike a child buffer, an ROI does not have its own Aurora Imaging Library identifier and cannot have results returned with respect to it.

Use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)to create an ROI from an image mask, the graphics in a 2D graphics list, or the valid pixels in a depth map.

*[Image: buf_roi_example.png]*

When defined from an image mask, the region of interest consists of the pixels corresponding to the non-zero pixels in the image mask. When defined from the graphics contained in a 2D graphics list, only areas corresponding to the graphics are processed. If the graphics defining the ROI are not filled, the ROI will consist of only the graphics' outline ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControlList.md)). However, if the combination constant [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)is specified when calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), the ROI is created such that non-filled graphics are considered filled. This is useful if you want to display the 2D graphics list defining the region of interest, or modify it interactively, without obstructing the target image. You can also specify the combination constant [`M_USE_LINE_THICKNESS_1`](../../Reference/buf/MbufSetRegion.md) when calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) to ignore the [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) setting of the graphics in the 2D graphics list and use the default value of one pixel. This is useful when interactively creating a region of interest from graphics in a 2D graphics list that have line thickness greater than one pixel for visual clarity.

*[Image: buf_roi_example2.png]*

To ignore invalid pixels in a depth map image buffer, associate the image with an ROI that identifies the valid pixels. To do so, use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`](../../Reference/buf/MbufSetRegion.md). You can also specify an image mask to further limit the ROI to pixels that are non-zero in the image mask buffer. The depth map must be fully corrected. For more information, see [Generating fully corrected depth and intensity maps](../C35_3D_Image_processing/S15_Generating_a_fully_corrected_depth_map.md).

When defining the ROI with an image mask, this raster (bitmap) information is stored in the image buffer, creating an[`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI. When defining the ROI with a 2D graphics list, this vectorial information and/or its corresponding raster (bitmap) version is stored in the image buffer, creating an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI, an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI, or an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, depending on how you set the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter of [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). Some functions (for example, [`MblobCalculate`](../../Reference/blob/MblobCalculate.md)) expect an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI, while other functions (for example, [`McodeRead`](../../Reference/code/McodeRead.md)) expect an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI; creating an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI allows more Aurora Imaging Library modules to use the ROI in the source image. To inquire an ROI's type, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_REGION_TYPE`](../../Reference/buf/MbufInquire.md).

When an ROI is defined, the information from the specified 2D graphics list or image mask is copied to the source image buffer. Any subsequent changes to the original 2D graphics list or image mask will not affect the ROI. To update the ROI using a new or modified 2D graphics list or image mask, call [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) again with the new information. The old ROI information is replaced with the new ROI information.

An [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI or an[`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI can be associated with the same camera calibration information as the image. This ensures that whenever the relative coordinate system moves (using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md), [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md), or [`McalFixture`](../../Reference/cal/McalFixture.md)), the ROI moves accordingly. To associate an ROI with camera calibration information, you must define the ROI with a 2D graphics list. All graphics in the 2D graphics list that were created in world units (using [`MgraControl`](../../Reference/gra/MgraControl.md) with[`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) set to[`M_WORLD`](../../Reference/gra/MgraControl.md)), will be associated with the same camera calibration information as the image; all other graphics in the 2D graphics list, as well as any generated raster information, will not be calibrated.

If the camera calibration information associated with the image buffer changes, the raster information of an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI will be discarded, causing the ROI to become an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI. Calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter set to [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) and the [`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md) parameter set to [`M_NULL`](../../Reference/buf/MbufSetRegion.md) transforms the ROI back to an[`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, such that the raster data is synchronized with the vector information.

By default, the ROI of a child buffer is dynamically linked with that of its parent. You can define different ROIs for a parent buffer and its child buffer, or relink the ROIs using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)with the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter set to [`M_LINK_TO_PARENT`](../../Reference/buf/MbufSetRegion.md). Note that a child buffer's ROI is not accessible from its parent buffer.

To remove the ROI from a source image buffer, call [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter set to[`M_DELETE`](../../Reference/buf/MbufSetRegion.md). Alternatively, to ignore the ROI associated with a source image buffer without removing it, call [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_REGION_USE`](../../Reference/buf/MbufControl.md).

The following table gives a broad overview of the modules that support the raster and vector-based ROIs. This table is meant as a general guideline and every module will have exceptions. For example, in most modules which do support ROIs, the [`M...Draw`](../../Reference/blob/MblobDraw.md) function will not support any type of ROI. For functions that take an image buffer, the Aurora Imaging Library Reference indicates if the function does not support an ROI or if there are ROI limitations. If the function takes an image buffer but does not specify whether it does or does not support an ROI, it is safe to assume it does support an ROI without any restrictions.

| Module | ROI | ROI in world units |
| --- | --- | --- |
| M3dimConvertDepthMap | Y | N | N |
| M3dimGet | Y | N | N |
| M3dimGetList | Y | N | N |
| M3dimRemapDepthMap | Y | N | N |
| M3dimStat | Y | N | N |
| M3dmap | N | N | N |
| M3dmetStat | Y | N | N |
| M3dmetVolume | Y | N | N |
| Magm | N | N | N |
| Mbead | N | N | N |
| Mblob | Y | N | N |
| MbufConvert3d | Y | N | N |
| Mclass | N | Y | Y |
| Mcode | N | Y | Y |
| Mcol (in general) | N | N | N |
| [`McolDefine`](../../Reference/col/McolDefine.md) | Y | N | N |
| Mdlocr | N | Y | Y |
| Mdmr | N | Y | Y |
| Medge | N | N | N |
| Mgra | N | N | N |
| Mim (in general) | N | N | N |
| [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md)(for stats) | Y [^Some] | N [^Some] | N |
| [`MimArith...`](../../Reference/im/MimArith.md) | Y | N | N |
| [`MimLutMap`](../../Reference/im/MimLutMap.md) | Y | N | N |
| Mmeas | N | Y | Y |
| Mmet | N | N | N |
| Mmod | N | N | N |
| Mocr | N | Y | Y |
| Mpat | N | Y | Y |
| Mreg | N | N | N |
| Mstr | N | Y | Y |

[^Some]: Note that not all statistics operations support ROIs; for a list of those that do, see [`MimControl`](../../Reference/im/MimControl.md).

[^VnR]: These types of ROI will also support [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROIs.

### Processing a non-rectangular area when an ROI is not supported

To process a non-rectangular area when an ROI is not supported, you can apply a mask to the source image. For example:

1. Allocate an image buffer, using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The buffer should be at least the same size as the area in the source image to process. This buffer will be used as a mask buffer.
2. Draw a filled shape in the mask buffer corresponding to the pixels to be modified, using the graphics functions ([`Mgra...`](../../Reference/gra/MgraAlloc.md)) or the drawing functions ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)). You can also annotate the image with Windows GDI annotations. For more information, see [Using GDI annotations](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md).
3. Allocate a temporary destination buffer, using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The temporary destination buffer should be at least the same size as the area to process.
4. Perform the required operation from the source buffer to the temporary destination buffer.
5. Copy the temporary destination buffer to the required destination buffer, using [`MbufCopyCond`](../../Reference/buf/MbufCopyCond.md), with the mask buffer as the condition buffer.

## Copying specific buffer areas

As an alternative to using a child buffer or an ROI, you can restrict operations to specific areas or bits of a buffer (child or parent) by copying the required portions to another buffer. For information about the functions allowing you to copy specific buffer areas, see[Managing data buffers](S07_Managing_data_buffers.md).
