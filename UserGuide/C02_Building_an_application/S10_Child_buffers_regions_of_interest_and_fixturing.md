---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Child_buffers_regions_of_interest_and_fixturing
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Child buffers regions of interest and fixturing"
---

# Child buffers, regions of interest, and fixturing

When working in Aurora Imaging Library, you can operate on a specified subset of data of an image buffer. This can be done with child buffers or with regions of interest (ROI). You can also fixture the relative coordinate system of an image to the same point on an object, regardless of the location of the object in the image, so that you can position a region of interest or a module's search region according to an object's found location. This can be used, for example, when automating a series of measurement operations.

> **Note:** Aurora Imaging Library Lite does not support fixturing.

## Child buffers

A child buffer is a specified subset of a data buffer (known as the parent buffer), associated with an Aurora Imaging Library identifier. Child buffers occupy a specific rectangular area of the parent buffer. Since this area is part of the same physical memory space as the parent buffer, changes made to the data of the child buffer affect the data of the parent buffer and vice versa. Note that a buffer which has no parent is referred to as the ancestor buffer of itself, its child buffers, and all other descendants.

Since a child buffer has an Aurora Imaging Library identifier, it is considered a data buffer, and it can be used in the same context as its parent buffer; for example, it can be selected to an Aurora Imaging Library display and be the only data that is displayed. A child buffer takes on the same attributes and type as its parent buffer. However, any pixel coordinates specified or returned when using a child buffer are relative to the child buffer's top-left corner.

*[Image: buf_parent_and_child_buffers.png]*

Just as its parent buffer, a child buffer must be allocated so that it can be associated with an identifier and recognized as an entity by Aurora Imaging Library. You can use one of the functions from the table below to allocate the child buffer.

| Aurora Imaging Library function | Monochrome parent buffer. | Multi-band parent buffer. |
| --- | --- | --- |
| [`MbufChild1d`](../../Reference/buf/MbufChild1d.md) | 1D region of first row of band. | 1D region of first row of all bands. |
| [`MbufChild2d`](../../Reference/buf/MbufChild2d.md) | 2D region of band. | 2D region of all bands. |
| [`MbufChildColor`](../../Reference/buf/MbufChildColor.md) | - | Entire area of band chosen. |
| [`MbufChildColor2d`](../../Reference/buf/MbufChildColor2d.md) | - | 2D region of band chosen or 2D region of all bands. |

Specify a child buffer's size and offset keeping in mind the parent buffer's dimensions. Note that, as a subset of the parent buffer, a child buffer cannot exceed the bounds of its parent in any dimension. For example, a color child buffer cannot be created from a monochrome parent buffer.

Once you have finished using a child buffer, you must free it using [`MbufFree`](../../Reference/buf/MbufFree.md), before freeing its parent buffer.

For more information, see [Child buffers](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md).

## Regions of interest

A region of interest (ROI) is similar to a child buffer in that it identifies a subset of data in an image buffer. The ROI of an image buffer can be of any shape and can be composed of several non-contiguous areas. When dealing with processing operations that respect the ROI, all pixels outside the ROI are ignored. You can have the ROI move within the image buffer according to the relative coordinate system, if the image is calibrated. Note that, unlike a child buffer, an ROI does not have its own Aurora Imaging Library identifier and cannot have results returned with respect to it.

Use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)to create an ROI from an image mask, the graphics in a 2D graphics list, or the valid pixels in a depth map.

*[Image: buf_roi_example.png]*

When defined from an image mask, the region of interest consists of the pixels corresponding to the non-zero pixels in the image mask. When defined from the graphics contained in a 2D graphics list, only areas corresponding to the graphics are processed. If the graphics defining the ROI are not filled, the ROI will consist of only the graphics' outline ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControlList.md)). However, if the combination constant [`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md)is specified when calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), the ROI is created such that non-filled graphics are considered filled. This is useful if you want to display the 2D graphics list defining the region of interest, or modify it interactively, without obstructing the target image. You can also specify the combination constant [`M_USE_LINE_THICKNESS_1`](../../Reference/buf/MbufSetRegion.md) when calling [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) to ignore the [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) setting of the graphics in the 2D graphics list and use the default value of one pixel. This is useful when interactively creating a region of interest from graphics in a 2D graphics list that have line thickness greater than one pixel for visual clarity.

*[Image: buf_roi_example2.png]*

You can use [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_REGION_USE`](../../Reference/buf/MbufControl.md) to set whether the ROI associated with the image buffer is used with supported image processing operations.

> **Note:** It is not possible to set an ROI in an image buffer that has one or more child buffers. However, it is possible to define an ROI directly in a child buffer. The child buffer's ROI is not accessible from the parent buffer.

For more information, see [Specifying a region of interest](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md).

## Fixturing an object with the relative coordinate system

A technique known as fixturing is available to place the relative coordinate system at a fixed position from an object. This is useful if the object could appear any number of times at any location or orientation in your images, and you want to apply the same measurement or inspection process to every instance of the object. Since fixturing is based on the relative coordinate system, your images must be calibrated even if they only use a one-to-one uniform camera calibration.

To fixture an object, you must first set up an analysis operation that will return the location(s) of the object in your image (for example, using Model Finder). Once you have established the location (reference location) for each instance of the object, create a loop that calls [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md) and the reference location of one of the instances. For each instance of your object, the relative coordinate system is placed at the reference location. In the loop, after displacing the relative coordinate system, set up the rest of the inspection process with respect to the relative coordinate system. You can now process each instance of your object, and have results returned with respect to the displaced relative coordinate system.

The following animation depicts a set of nails where the relative coordinate system is fixtured to each instance of the nail, before placing the Aurora Imaging Library Measurement module's search region with respect to the relative coordinate system and then performing the analysis.

*[Image: fixture_intro]*

The following code snippet illustrates how to setup fixturing. An image calibrated with a uniform one-to-one camera calibration is used.

> **Code example:** [userguide.building_an_application.child_buffers_regions_of_interest_and_fixturing01](userguide.building_an_application.child_buffers_regions_of_interest_and_fixturing01)

For more information, see [Fixturing overview](../C29_Fixturing/S01_Fixturing_in_AIL.md).
