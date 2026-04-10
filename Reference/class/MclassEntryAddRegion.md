---
doctype: Reference
module: class
function: MclassEntryAddRegion
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassEntryAddRegion"
---

# MclassEntryAddRegion

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

> Add regions to an entry in an images dataset context used for object detection or segmentation.

## Syntax

```c
void MclassEntryAddRegion(
    AIL_ID             DatasetContextClassId,  //in
    AIL_INT64          EntryIndex,             //in
    AIL_UUID           EntryKey,               //in
    AIL_INT64          DescriptorType,         //in
    AIL_ID             DescriptorId,           //in
    AIL_CONST_TEXT_PTR DescriptorFileName,     //in
    AIL_INT64          ClassIndexGroundTruth,  //in
    AIL_INT64          ControlFlag             //in
)
```

## Description

This function adds regions to an entry in an images dataset context used for object detection or segmentation. This function should not be used for image or feature classification.

Each time you call [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md), one or more regions are added to the specified entry. The number and type of regions that are added is determined by the region's descriptor ([`DescriptorType`](../../Reference/class/MclassEntryAddRegion.md)). For example, you can add one or more regions from a mask (image) or a graphics list.

Each added region has a ground truth assigned to it. Depending on the descriptor, the added region's ground truth is established with the [`ClassIndexGroundTruth`](../../Reference/class/MclassEntryAddRegion.md) or [`DescriptorId`](../../Reference/class/MclassEntryAddRegion.md) parameter. The pixels in the entry image that correspond to the added region are identified with this ground truth.

The first region added has an index value of 1. Region 0 is created by default for all entries and is reserved for image classification. The ground truth of the entry itself applies to region 0, and has no effect on the added regions.

To control and inquire about regions, call [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) and [`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md). To draw region information, call [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md). Some internal descriptor information about regions, such as their polygon, can be drawn but not inquired.

## Parameters

### `DatasetContextClassId` *(in, AIL_ID)*

Specifies the identifier of the images dataset context containing the entries for which to add the regions. This dataset context should be used for object detection or segmentation and must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md).

### `EntryIndex` *(in, AIL_INT64)*

Specifies the index of the entry to which regions are added.

*For specifying the entry's index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the [`EntryIndex`](../../Reference/class/MclassEntryAddRegion.md) parameter is not required. |
| `Value >= 0` | Specifies the index of an entry. |

### `EntryKey` *(in, AIL_UUID)*

Specifies the key (_AIL_UUID_) of the entry to which regions are added. The key is defined as an Aurora Imaging Library universal unique identifier (UUID).

*For specifying the entry's key*

| Value | Description |
| --- | --- |
| `M_DEFAULT_KEY` | Specifies that the [`EntryKey`](../../Reference/class/MclassEntryAddRegion.md) parameter is not required. |
| `AIL_UUID Value` | Specifies the key (_AIL_UUID_) of an entry. |

### `DescriptorType` *(in, AIL_INT64)*

Specifies the descriptor type of the region to add to the entry.

### `DescriptorId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library object that describes the region to add. If this parameter is not needed, set it to `M_NULL`.

### `DescriptorFileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the file that describes the region to add. If this parameter is not needed, set it to `M_NULL`.

### `ClassIndexGroundTruth` *(in, AIL_INT64)*

Specifies the index of the class definition that represents the added regions' ground truth. If this parameter is not needed, set it to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

## Parameter Associations

### For establishing the region to add to the entry

---

### `M_DESCRIPTOR_TYPE_BOX`

Adds a new bounding box region to the specified entry. The pixels in the entry image that correspond to the areas within the bounding box are considered part of the ground truth established for this region ([`ClassIndexGroundTruth`](../../Reference/class/MclassEntryAddRegion.md)). The bounding box must not be rotated by any angle.  [`M_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassEntryAddRegion.md) is only available for object detection.

#### `2D graphics list identifier`

Sets the identifier of the 2D graphics list from which to establish the added region. The 2D graphics list must contain a single rectangle that is not defined in world units.

---

### `M_DESCRIPTOR_TYPE_MASK`

Adds an image (mask) region to the specified entry. The pixels in the entry image that correspond to the non-zero pixels (masked) in the specified image are considered part of the ground truth specified for this region ([`ClassIndexGroundTruth`](../../Reference/class/MclassEntryAddRegion.md)).  You must use a single band image, and its dimensions must be the same as the entry image, as specified by [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)). This image must not have an ROI associated with it ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)).  [`M_DESCRIPTOR_TYPE_MASK`](../../Reference/class/MclassEntryAddRegion.md) is only available for segmentation.

#### `M_NULL`

Specifies that this parameter is not needed and the added region will be established from an image file ([`DescriptorFileName`](../../Reference/class/MclassEntryAddRegion.md)).

#### `Image buffer identifier`

Sets the identifier of the image buffer from which to establish the added region. This image will be saved on disk at the location specified by [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)).

---

### `M_DESCRIPTOR_TYPE_POLYGON`

Adds a region with one or more polygons to the specified entry. The pixels in the entry image that correspond to the areas within the polygons are considered part of the ground truth established for this region ([`ClassIndexGroundTruth`](../../Reference/class/MclassEntryAddRegion.md)).  [`M_DESCRIPTOR_TYPE_POLYGON`](../../Reference/class/MclassEntryAddRegion.md) is only available for segmentation.

#### `2D graphics list identifier`

Sets the identifier of the 2D graphics list from which to establish the added region. The 2D graphics list must contain polygons that are not defined in world units.

---

### `M_GROUND_TRUTH_IMAGE`

Adds regions to the entry according to the ground truth indices in the specified image. The pixel values in this image must represent class definition index values. The corresponding pixels in the entry image have that class definition as their ground truth. The number of regions added corresponds to the number of class definitions in the ground truth image. [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassEntryAddRegion.md) is similar to [`M_GROUND_TRUTH_IMAGE_COLOR`](../../Reference/class/MclassEntryAddRegion.md), except you are providing the index of the class definitions, instead of their color.  If you are adding many regions, using [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassEntryAddRegion.md) can be more efficient than adding the regions one by one. For each class definition (region) detected, an image (mask) is saved to disk at the location specified by [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)).  If the specified ground truth image has an index value that does not correspond to an existing class definition index, Aurora Imaging Library creates a class definition with that index, and all the missing class definitions (indices) up to it. This is the same behavior as [`MclassImport`](../../Reference/class/MclassImport.md).  An index (pixel value) of -1 in the specified ground truth image indicates a don't care value. Pixels in the entry image that correspond to the -1 pixels in the ground truth image do not contribute to the training loss, but are available to be assigned to a region. This has the same effect as setting [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md) to [`M_DONT_CARE_CLASS`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)).  An index (pixel value) of -2 in the specified ground truth image indicates an unlabeled value. Pixels in the entry image that correspond to the -2 pixels in the ground truth image are not part of any region.  Be cautious of how don't care pixels (-1) and unlabeled pixels (-2) get resolved, given the data type and depth of your images. For example, when using 8-bit unsigned images, unlabeled pixels (-2) resolve to 0xFE (that is, 254); if your dataset already holds 254 or more class definitions, an Aurora Imaging Library error is generated, requesting an image buffer with a greater depth (for example, 16- bit unsigned).

#### `Image buffer identifier`

Sets the identifier of the image buffer from which to establish the added regions. This image buffer must not have an ROI associated with it ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)).

---

### `M_GROUND_TRUTH_IMAGE_COLOR`

Adds regions to the entry according to the ground truth colors in the specified image. The pixel values in this image must represent class definition colors. The corresponding pixels in the entry image have that class definition as their ground truth. The number of regions added corresponds to the number of class definition colors in the ground truth image. [`M_GROUND_TRUTH_IMAGE_COLOR`](../../Reference/class/MclassEntryAddRegion.md) is similar to [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassEntryAddRegion.md), except you are providing the color of the class definitions, instead of their index.  For each class definition (region) detected, an image (mask) is saved to disk at the location specified by [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)).  If the specified ground truth image has a color that does not correspond to an existing class definition color, Aurora Imaging Library creates a class definition with that color. Class definition colors are set with [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)).  A color (pixel value) of [`M_DONT_CARE_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) in the specified ground truth image indicates a don't care value. Pixels in the entry image that correspond to [`M_DONT_CARE_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) in the ground truth image do not contribute to the training loss, but are available to be assigned to a region.  A color (pixel value) of [`M_NO_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) in the specified ground truth image indicates an unlabeled value. Pixels in the entry image that correspond to [`M_NO_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) in the ground truth image are not part of any region.  When using [`M_GROUND_TRUTH_IMAGE_COLOR`](../../Reference/class/MclassEntryAddRegion.md), the [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md), [`M_DONT_CARE_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md), and [`M_NO_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) controls must have unique colors.

#### `Image buffer identifier`

Sets the identifier of the image buffer from which to establish the added regions. This image buffer must not have an ROI associated with it ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)).
