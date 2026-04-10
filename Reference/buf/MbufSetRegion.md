---
doctype: Reference
module: buf
function: MbufSetRegion
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufSetRegion"
---

# MbufSetRegion

> Set the region of interest (ROI) of an image buffer.

## Syntax

```c
void MbufSetRegion(
    AIL_ID     ImageBufId,            //out
    AIL_ID     ImageOrGraphicListId,  //in
    AIL_INT64  Label,                 //in
    AIL_INT64  Operation,             //in
    AIL_DOUBLE Param                  //in
)
```

## Description

This function sets the region of interest (ROI) of an image buffer; if supported, Aurora Imaging Library functions will only operate on this region of the buffer. [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) can also be used to modify, delete, copy, extract, or link the ROI.

The ROI of an image buffer can be of any shape and can be composed of several non-contiguous areas. When dealing with processing operations that respect the ROI, all pixels outside the ROI are ignored; to disable their use of the ROI, use [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_REGION_USE`](../../Reference/buf/MbufControl.md).

You can define an ROI from an image mask, a 2D graphics list, or the valid pixels in a depth map. When defined from an image mask, the region of interest consists of the pixels corresponding to the non-zero pixels in the image mask. When defined from the graphics contained in a 2D graphics list, only areas corresponding to the graphics are processed. If the graphics defining the ROI are not filled, the ROI will consist of only the graphics' outline ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControlList.md)). However, if the combination constant [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)is specified when calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), the ROI is created such that non-filled graphics are considered filled. This is useful if you want to display the 2D graphics list defining the region of interest, or modify it interactively, without obstructing the target image. You can also specify the combination constant [`M_USE_LINE_THICKNESS_1`](../../Reference/buf/MbufSetRegion.md) when calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) to ignore the [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) setting of the graphics in the 2D graphics list and use the default value of one pixel. This is useful when interactively creating a region of interest from graphics in a 2D graphics list that have line thickness greater than one pixel for visual clarity.

> **Note:** The image mask must be the same size as the image buffer.

For a fully-corrected depth map image buffer, you can also define an ROI from the valid pixels of the image (using [`M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`](../../Reference/buf/MbufSetRegion.md)). In this case, any pixel in the image that is set to the maximum possible value (for example, 255 for an 8-bit buffer) is considered invalid and is excluded from the ROI.

Note that, unlike a child buffer, an ROI does not have its own Aurora Imaging Library identifier and cannot have results returned with respect to it.

When defining the ROI with an image mask, this raster (bitmap) information is copied and associated with the image buffer, creating an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI. When defining the ROI with a 2D graphics list, this vectorial information and/or its corresponding raster (bitmap) version is copied and associated with the image buffer, creating an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI, an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI, or an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, depending on how you set the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter. Some functions (for example, [`MblobCalculate`](../../Reference/blob/MblobCalculate.md)) expect an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI, while other functions (for example, [`McodeRead`](../../Reference/code/McodeRead.md)) expect an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI; creating an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI allows more Aurora Imaging Library modules to use the ROI in the source image.

When an ROI is defined, the information from the specified 2D graphics list or image mask is copied and the copy is associated with the source image buffer. Any subsequent changes to the original 2D graphics list or image mask will not affect the ROI. To update the ROI using a new or modified 2D graphics list or image mask, call [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) again with the new information. The old ROI information is replaced with the new ROI information.

An [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI or an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI can be associated with the same camera calibration information as the image. This ensures that whenever the relative coordinate system moves (using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md), [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md), or [`McalFixture`](../../Reference/cal/McalFixture.md)), the ROI moves accordingly. To associate an ROI with camera calibration information, you must define the ROI with a 2D graphics list. All graphics in the 2D graphics list that were created in world units (using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) set to [`M_WORLD`](../../Reference/gra/MgraControl.md)) will be associated with the same camera calibration information as the image; all other graphics in the 2D graphics list, as well as any generated raster information, will not be calibrated. Note that defining an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI with a 2D graphics list containing graphics created in world units will generate an error if the specified image buffer is not already calibrated.

If the camera calibration information associated with the image buffer changes, the raster information of an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI will be discarded, causing the ROI to become an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI. Calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter set to [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) and the [`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md) parameter set to [`M_NULL`](../../Reference/buf/MbufSetRegion.md) transforms the ROI to an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, such that the raster data is synchronized with the vector information.

Not all functions support an image buffer with an ROI. If you pass an image buffer with an ROI to a processing or analysis function that does not use the ROI of an image buffer, an error will be generated. The functions' descriptions state whether they support the use of an ROI.

> **Note:** If a child buffer is allocated using a parent buffer with an ROI, the child buffer's ROI is automatically linked with that of the parent buffer (the same as using this function with [`M_LINK_TO_PARENT`](../../Reference/buf/MbufSetRegion.md) and the Aurora Imaging Library identifier of the child buffer). You can also set an ROI in the child buffer directly. Note that the ROI of the child buffer is only accessible using the child buffer's Aurora Imaging Library identifier.

You cannot set an ROI in a YUV image buffer unless its format is YUV24 Planar, and it is not a child buffer.

## Parameters

### `ImageBufId` *(out, AIL_ID)*

Specifies the identifier of the image buffer for which to set the ROI, or from which to copy or extract the ROI.

### `ImageOrGraphicListId` *(in, AIL_ID)*

Specifies the identifier of the image buffer or 2D graphics list that contains the data to use to set the ROI, or to which to copy or extract the ROI information. If not used, set this parameter to `M_NULL`.

### `Label` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

### `Param` *(in, AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the operation to perform for the ROI(s)

---

### `M_DEFAULT`

Same as [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md).

---

### `M_COPY`

Specifies to copy the ROI of the source image buffer and set the copy as the ROI of the destination image buffer.  If the source buffer has an ROI only in an [`M_RASTER`](../../Reference/buf/MbufInquire.md)format, and the destination buffer is smaller than the source buffer, the copied ROI is cropped to the size of the destination buffer. If the destination buffer is larger than the source buffer, the copied ROI retains the size of the source buffer's ROI. This is true even if the source buffer is a child buffer that has an ROI that is linked to the ROI of a larger parent buffer (by default, or using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_LINK_TO_PARENT`](../../Reference/buf/MbufSetRegion.md)); only the part of the linked ROI that is within the area of the source buffer is copied.  If the source buffer has an ROI in an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) format, the entire ROI is copied, including parts of the ROI that are outside the source image.  If the ROI is in an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)format, the entire vector portion of the ROI is copied, including parts of the ROI that are outside the source image. The raster portion of the ROI is recalculated based on the region that is visible in the destination image (if the source image and destination image are not the same size).

| Value | Description |
| --- | --- |
| `Image buffer identifier from which to copy the ROI` | Specifies the identifier of the source image buffer from which to copy the ROI. |
| `Image buffer identifier for which to define the ROI` | Specifies the identifier of the destination image buffer to which to copy the ROI.  If the image buffer is already associated with an ROI, this association is removed and the image buffer is associated with the new ROI. If the image buffer is a child buffer with an ROI linked to that of its parent, the ROIs are unlinked. |

---

### `M_DELETE`

Specifies to remove ROI information from the image buffer.

| Value | Description |
| --- | --- |
| `Image buffer identifier for which to remove the ROI` | Specifies the Aurora Imaging Library identifier of the image buffer from which to remove ROI information. |

---

### `M_EXTRACT`

Specifies to extract the 2D graphics list or image mask used to define the ROI of the source image buffer, and store it in the destination image buffer or 2D graphics list.  > **Note:** When you pass a 2D graphics list as a destination, the graphics used to define the source ROI are added to the destination 2D graphics list. Existing graphics in the destination 2D graphics list are not removed.  If the source buffer has an ROI in an [`M_RASTER`](../../Reference/buf/MbufInquire.md) format, [`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md) must be set to the Aurora Imaging Library identifier of an image buffer. If the source buffer has an ROI in an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) format,[`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md) must be set to the Aurora Imaging Library identifier of a 2D graphics list.  If the source buffer has an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, [`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md)can be set to the Aurora Imaging Library identifier of either a 2D graphics list or an image buffer. If the destination image buffer is larger than the source image, the existing image mask is copied; the image mask is not recalculated based on the size of the destination image.

| Value | Description |
| --- | --- |
| `Image buffer identifier from which to extract the ROI` | Specifies the Aurora Imaging Library identifier of the source image buffer from which to extract the 2D graphics list or image mask used to define the ROI. |
| `2D graphics list identifier in which to store the extracted 2D graphics` | Specifies the identifier of the destination 2D graphics list in which to store the 2D graphics of the extracted 2D graphics list. The 2D graphics list must have been previously allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |
| `Image buffer identifier in which to store the extracted image mask` | Specifies the identifier of the destination image buffer in which to store the extracted image mask. |

---

### `M_LINK_TO_PARENT`

Specifies to link the ROI of the image buffer to the ROI of its parent buffer. The child buffer's offset and size are used to determine what portion of the parent's ROI to use for the child. The ROI information for the parent and child buffer occupy the same memory space. Therefore, modifying the ROI of the parent buffer affects the ROI of the child buffer. Modifying the ROI of the child buffer does not affect the ROI of the parent buffer; instead it unlinks the ROIs.  > **Note:** The ROI of a child buffer is linked to the ROI of its parent buffer by default.  This image shows the relationship between the linked ROIs of a parent and child buffer.  *[Image: buf_linked_ROIs.png]*  You can unlink the ROI by setting an ROI for the child buffer using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with any other type of operation. To retain the child buffer's current ROI when unlinking, set the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter to [`M_COPY`](../../Reference/buf/MbufSetRegion.md), and set both the [`ImageBufId`](../../Reference/buf/MbufSetRegion.md) and [`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md)parameters to the Aurora Imaging Library identifier of the child buffer.  If the specified buffer is not a child buffer, an error is generated.

| Value | Description |
| --- | --- |
| `Child image buffer identifier of which to link the ROI` | Specifies the Aurora Imaging Library identifier of the child image buffer, of which to link the ROI with that of its parent buffer. |

---

### `M_NO_RASTERIZE`

Specifies to create an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI, defined by the graphics of a 2D graphics list. The content of the 2D graphics list is copied, and this vectorial information is associated with the specified image buffer.  Only the areas defined by the graphics of the 2D graphics list are processed by Aurora Imaging Library functions. If a shape is not filled, only the pixels corresponding to its contour will be processed by Aurora Imaging Library functions unless the combination constant [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)is specified.

| Value | Description |
| --- | --- |
| `Image buffer identifier for which to set the ROI` | Specifies the Aurora Imaging Library identifier of the image buffer for which to set the ROI.  If the image buffer is already associated with an ROI, this association is removed and the image buffer is associated with the new ROI. If the image buffer is a child buffer with an ROI linked to that of its parent, the ROIs are unlinked. |
| `2D graphics list identifier to use to define the ROI` | Specifies the identifier of the 2D graphics list to use to define the ROI. The 2D graphics list must have been previously allocated and filled with 2D graphics using the graphics functions ([`Mgra...`](../../Reference/gra/MgraAlloc.md)). |

---

### `M_RASTERIZE`

Specifies to create an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI defined by an image mask, an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI defined by a 2D graphics list, or to change an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI to an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI. Aurora Imaging Library functions will only operate on source image pixels corresponding to either non-zero value pixels of the image mask or areas defined by the graphics of the 2D graphics list, depending on whether the Aurora Imaging Library function requires raster or vectorial information.  When creating an ROI defined by an image mask, the content of the image mask is copied and this raster (bitmap) information is stored as the raster portion of the ROI associated with the specified image buffer.  When creating an ROI defined by a 2D graphics list, or changing an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI to an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, an image mask is generated from the 2D graphics list or existing vectorial information. This raster (bitmap) information and the vectorial information (from the existing ROI or specified 2D graphics list) are stored as the raster portion and vector portion of the ROI associated with the image buffer. When generating the mask, pixels corresponding to areas defined by the vectorial information are set to a non-zero value, and all other image mask pixels are set to a zero value. If a shape is not filled, only the pixels corresponding to its contour will be set to a non-zero value, unless the combination constant [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)is specified.  > **Note:** [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)is not available when creating an ROI from an image mask.

| Value | Description |
| --- | --- |
| `Image buffer identifier for which to set the ROI` | Specifies the Aurora Imaging Library identifier of the image buffer for which to set the ROI.  If the image buffer is already associated with an ROI, this association is removed and the image buffer is associated with the new ROI (except when [`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md) is set to [`M_NULL`](../../Reference/buf/MbufSetRegion.md). If the image buffer is a child buffer with an ROI linked to that of its parent, the ROIs are unlinked. |
| `M_NULL` | Specifies to rasterize the existing [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI of the image buffer. This changes the [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI to an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI.  Note that this operation has no effect on an [`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI. |
| `2D graphics list identifier to use to define the ROI` | Specifies the identifier of the 2D graphics list to use to define the ROI. The 2D graphics list must have been previously allocated and filled with 2D graphics using the graphics functions ([`Mgra...`](../../Reference/gra/MgraAlloc.md)). |
| `Image mask buffer identifier to use to define the ROI` | Specifies the identifier of the image buffer to use as an image mask to define the ROI. The image mask must be the same size as the image buffer, and must be a 1-band image buffer. |

---

### `M_RASTERIZE_AND_DISCARD_LIST`

Specifies to create an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI defined by a 2D graphics list. An image mask is generated from the specified 2D graphics list, and stored as the raster ROI associated with the image buffer. When generating the mask, pixels corresponding to areas defined by the 2D graphics list are set to a non-zero value, and all other mask pixels are set to a zero value. If a shape is not filled, only the pixels corresponding to its contour will be set to a non-zero value, unless the combination constant [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)is specified. This operation does not store the vectorial information from the 2D graphics list in the ROI.  Aurora Imaging Library functions will only operate on source image pixels corresponding to non-zero value pixels of the image mask.

| Value | Description |
| --- | --- |
| `Image buffer identifier for which to set the ROI` | Specifies the Aurora Imaging Library identifier of the image buffer for which to set the ROI.  If the image buffer is already associated with an ROI, this association is removed and the image buffer is associated with the new ROI. If the image buffer is a child buffer with an ROI linked to that of its parent, the ROIs are unlinked. |
| `2D graphics list identifier to use to define the ROI` | Specifies the identifier of the 2D graphics list to use to define the ROI. The 2D graphics list must have been previously allocated and filled with 2D graphics using the graphics functions ([`Mgra...`](../../Reference/gra/MgraAlloc.md)). |

---

### `M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`

Specifies to create an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI for a fully-corrected depth map image buffer; in this case, the ROI is defined by the valid pixels of the depth map. Any pixel in the image that is set to the maximum possible value (for example, 255 for an 8-bit buffer) is considered invalid and is excluded from the ROI.  You can either set and define the ROI using the depth map alone, or you could use the depth map and an image mask buffer. If you choose to use both the depth map and the mask buffer, the ROI will be defined by the pixels that are both valid in the depth map and non-zero in the image mask buffer. Note that if the mask buffer is a depth map, the ROI will be defined by the pixels that are valid in both depth maps (not equal to the invalid value).  Note that, if some pixels of the depth map are later changed to be valid or invalid (for example during a grab), the ROI is not dynamically updated; you must call this function again to update the ROI.  > **Note:** You can also create an[`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI defined by an image buffer or 2D graphics list for a fully-corrected depth map image buffer. To do so, use [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md).

| Value | Description |
| --- | --- |
| `Fully-corrected depth map image buffer identifier for which to set the ROI` | Specifies the Aurora Imaging Library identifier of the image buffer for which to set the ROI.  The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer and must store a fully-corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).  If the image buffer is already associated with an ROI, this association is removed and the image buffer is associated with the new ROI. If the image buffer is a child buffer with an ROI linked to that of its parent, the ROIs are unlinked. |
| `M_NULL` | Specifies that the depth map will be the only buffer used to define the ROI. |
| `Image mask buffer identifier to use to define the ROI` | Specifies the identifier of the image buffer to use as an image mask to define the ROI. The ROI will be defined by the pixels that are both valid in the depth map and non-zero in the image mask buffer (or not equal to the invalid value if this image buffer stores a fully-corrected depth map). The image mask must be the same size as the depth map image buffer, and must be a 1-band image buffer. |

### Combination Constants — For interpreting the graphics in the 2D graphics list

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to set whether to interpret the graphics in the 2D graphics list as being filled or as having a line thickness of 1 pixel.

| Value | Description |
| --- | --- |
| `M_FILL_REGION` | Specifies that the graphics in the 2D graphics list are interpreted as if they are filled when defining the ROI. If one or more graphics of the 2D graphics list are already filled (for example, a rectangle created with [`MgraRectFill`](../../Reference/gra/MgraRectFill.md)), [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)has no effect on the already filled graphics. |
| `M_USE_LINE_THICKNESS_1` | Specifies that the graphics in the 2D graphics list are interpreted as if their [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) setting is set to 1 pixel when defining the ROI. |
