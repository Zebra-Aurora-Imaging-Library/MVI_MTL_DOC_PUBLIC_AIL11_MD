---
doctype: Reference
module: class
function: MclassDrawEntry
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassDrawEntry"
---

# MclassDrawEntry

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

> Draw region descriptors, classification results, segmentation results, object detection results, or anomaly detection results from an entry in an images dataset context.

## Syntax

```c
void MclassDrawEntry(
    AIL_ID    ContextGraId,            //in
    AIL_ID    DatasetContextClassId,   //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT64 EntryIndex,              //in
    AIL_UUID  EntryKey,                //in
    AIL_INT64 TaskType,                //in
    AIL_INT64 Index,                   //in
    AIL_INT * DstLabelArrayPtr,        //out
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific region descriptors, classification results, segmentation results, object detection results, or anomaly detection results from an entry in an images dataset context. Drawing operations are typically performed in a destination image buffer.

For datasets with more than 256 class definitions, using default colors will result in repeated colors.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the graphics context to use for the draw operation.

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `DatasetContextClassId` *(in, AIL_ID)*

Specifies the images dataset context from which to draw the entry.

*For specifying the image dataset context*

| Value | Description |
| --- | --- |
| `DatasetContextClassId` | Specifies the identifier of an images dataset context. This context must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md). |

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. This can be either an image buffer or a graphics list identifier depending on the draw operation.

### `Operation` *(in, AIL_INT64)*

Specifies the drawing operation. Set this parameter to one of the following values:

*For specifying the drawing operation*

| Value | Description |
| --- | --- |
| `M_DESCRIPTOR_TYPE_BOX` | Draws the bounding box descriptor from the specified region using the color specified in the dataset ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)). To change the color, call [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) on the specified graphics context ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)). To check for the presence of a bounding box descriptor, call [`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md) with [`M_NUMBER_OF_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassInquireEntry.md). Note that when no descriptor is present in the region, nothing is drawn.

[`M_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassDrawEntry.md) is only available for object detection. |
| `M_DESCRIPTOR_TYPE_MASK` | Draws the mask descriptor from the specified region using the color specified in the dataset ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)). To change the color, call [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) on the specified graphics context ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)). To check for the presence of a mask descriptor, call [`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md) with [`M_NUMBER_OF_DESCRIPTOR_TYPE_MASK`](../../Reference/class/MclassInquireEntry.md). Note that the destination must be an image buffer and not a graphics list. When no descriptor is present in the region, nothing is drawn.

[`M_DESCRIPTOR_TYPE_MASK`](../../Reference/class/MclassDrawEntry.md) is only available for segmentation and anomaly detection. |
| `M_DESCRIPTOR_TYPE_POLYGON` | Draws the polygon descriptor from the specified region using the color specified in the dataset ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)). To change the color, call [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) on the specifies graphics context ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)). To check for the presence of a polygon descriptor, call [`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md) with [`M_NUMBER_OF_DESCRIPTOR_TYPE_POLYGON`](../../Reference/class/MclassInquireEntry.md). Note that the required size of the [`DstLabelArrayPtr`](../../Reference/class/MclassDrawEntry.md) is [`M_NUMBER_OF_DESCRIPTOR_TYPE_POLYGON`](../../Reference/class/MclassInquireEntry.md) for a single region. When no descriptor is present in the region, nothing is drawn.

[`M_DESCRIPTOR_TYPE_POLYGON`](../../Reference/class/MclassDrawEntry.md) is only available for segmentation and anomaly detection. |
| `M_DRAW_ANOMALY_CONTOUR_MASK` | Draws the contour of the anomalies using the color of the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter. To change the color, you can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) on the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md) must be an image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md). Additionally, if the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter is not grayscale, [`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md) must be 3 bands and 8-bit. |
| `M_DRAW_ANOMALY_HEATMAP` | Draws the anomaly scores for each pixel location in the image.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md) must be an 8-bit and 3-band image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md). |
| `M_DRAW_ANOMALY_MASK` | Draws the anomalous pixels using the color of the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter. To change the color, you can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) on the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md) must be an image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md). Additionally, if the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter is not grayscale, [`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md) must be 3 bands and 8-bit. |
| `M_DRAW_ANOMALY_SCORES` | Draws the anomaly scores for each pixel location in the image.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md) must be a 1-band image buffer of type [`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |
| `M_DRAW_BEST_INDEX_CONTOUR_IMAGE` | Draws the contours of the best classes. If you do not specify the [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) combination value, Aurora Imaging Library uses the result type [`M_BEST_INDEX_IMAGE_TYPE`](../../Reference/class/MclassGetResult.md) to determine the type of the image to use. If you specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md), the image type must be 3 bands, 8-bit unsigned.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of predefined classifier that you are using ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassGetResultEntry.md)):

- For [`M_ICNET_...`](../../Reference/class/MclassGetResultEntry.md) or legacy[`M_FCNET_...`](../../Reference/class/MclassGetResultEntry.md), you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md), [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md), and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDrawEntry.md).
- For [`M_CSNET_...`](../../Reference/class/MclassGetResultEntry.md), you can specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md). |
| `M_DRAW_BEST_INDEX_IMAGE` | Draws an image using the index value of the best class at each point. If you do not specify the [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) combination value, Aurora Imaging Library uses the result type [`M_BEST_INDEX_IMAGE_TYPE`](../../Reference/class/MclassGetResult.md) to determine the type of the image to use. If you specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md), the image type must be 3 bands, 8-bit unsigned.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of predefined classifier that you are using ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassGetResultEntry.md)):

- For [`M_ICNET_...`](../../Reference/class/MclassGetResultEntry.md) or legacy [`M_FCNET_...`](../../Reference/class/MclassGetResultEntry.md), you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md), [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md), and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDrawEntry.md).
- For [`M_CSNET_...`](../../Reference/class/MclassGetResultEntry.md), you can specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md). |
| `M_DRAW_BEST_SCORE_IMAGE` | Draws an image using the best score at each point. If you do not specify the [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) combination value, the image type must be float or unsigned. If you specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md), the image type must be 3 bands, 8-bit unsigned.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of predefined classifier that you are using ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassGetResultEntry.md)):

- For [`M_ICNET_...`](../../Reference/class/MclassGetResultEntry.md) or legacy[`M_FCNET_...`](../../Reference/class/MclassGetResultEntry.md), you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md), [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md), and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDrawEntry.md).
- For [`M_CSNET_...`](../../Reference/class/MclassGetResultEntry.md), you can specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md). |
| `M_DRAW_BOX` | Draws the bounding boxes for all instances, a specific class, or a specific instance (only one box will be drawn in this case). The drawn color is specified in the class definitions. To change the line thickness of the box, call [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) on the specified graphics context ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)).

Multiple drawing operations for object detection can be used together by adding them. For example, [`M_DRAW_BOX`](../../Reference/class/MclassDrawEntry.md) + [`M_DRAW_BOX_NAME`](../../Reference/class/MclassDrawEntry.md). |
| `M_DRAW_BOX_CENTER` | Draws the center positions of the bounding boxes for all instances, a specific class, or a specific instance (only one center position will be drawn in this case). The drawn color is specified in the class definitions. To change the line thickness of center position, call [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) on the specified graphics context ([`ContextGraId`](../../Reference/class/MclassDrawEntry.md)).

Multiple drawing operations for object detection can be used together by adding them. For example, [`M_DRAW_BOX`](../../Reference/class/MclassDrawEntry.md) + [`M_DRAW_BOX_NAME`](../../Reference/class/MclassDrawEntry.md). |
| `M_DRAW_BOX_NAME` | Draws the class names for bounding boxes for all instances, a specific class, or a specific instance (only one name will be drawn in this case).

The class names are drawn using the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter. You can modify the text settings of the drawn names using [`MgraControl`](../../Reference/gra/MgraControl.md).

Multiple drawing operations for object detection can be used together by adding them. For example, [`M_DRAW_BOX`](../../Reference/class/MclassDrawEntry.md) + [`M_DRAW_BOX_NAME`](../../Reference/class/MclassDrawEntry.md).

If used with [`M_DRAW_BOX`](../../Reference/class/MclassDrawEntry.md), the class name is drawn on top of the outline of the bounding box. If used with [`M_DRAW_BOX_CENTER`](../../Reference/class/MclassDrawEntry.md), the class name is drawn near the center of the bounding box. Text alignment controls set with [`MgraControl`](../../Reference/gra/MgraControl.md) are relative to these locations. |
| `M_DRAW_BOX_SCORE` | Draws the class scores for bounding boxes for all instances, a specific class, or a specific instance (only one score will be drawn in this case).

The class scores are drawn using the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDrawEntry.md) parameter. You can modify the text settings of the drawn scores using [`MgraControl`](../../Reference/gra/MgraControl.md).

Multiple drawing operations for object detection can be used together by adding them. For example, [`M_DRAW_BOX`](../../Reference/class/MclassDrawEntry.md) + [`M_DRAW_BOX_NAME`](../../Reference/class/MclassDrawEntry.md).

If used with [`M_DRAW_BOX`](../../Reference/class/MclassDrawEntry.md), the class score is drawn on top of the outline of the bounding box. If used with [`M_DRAW_BOX_CENTER`](../../Reference/class/MclassDrawEntry.md), the class score is drawn near the center of the bounding box. Text alignment controls set with [`MgraControl`](../../Reference/gra/MgraControl.md) are relative to these locations. |
| `M_DRAW_CLASS_SCORES` | Draws (fills) an image using the scores of a specific class. This drawing operation is not possible when drawing general results; you must set the [`Index`](../../Reference/class/MclassDrawEntry.md) parameter to a specific class.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of predefined classifier that you are using ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassGetResultEntry.md)):

- For [`M_ICNET_...`](../../Reference/class/MclassGetResultEntry.md) or legacy[`M_FCNET_...`](../../Reference/class/MclassGetResultEntry.md), you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md) and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDrawEntry.md).
  If you specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md), the drawing is done with the classification map size (the [`ControlFlag`](../../Reference/class/MclassDrawEntry.md) parameter must be set to its default). If you do not specify the [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md) combination value, you can set the [`ControlFlag`](../../Reference/class/MclassDrawEntry.md) parameter to [`M_BILINEAR_INTERPOLATION`](../../Reference/class/MclassDrawEntry.md) or [`M_RECEPTIVE_FIELD_PATCH`](../../Reference/class/MclassDrawEntry.md).
  In all cases, when using [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md), the image type must be float or 8-bit unsigned.
- For [`M_CSNET_...`](../../Reference/class/MclassGetResultEntry.md), you cannot specify any combination value.
- You cannot specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) for any predefined classifier. |
| `M_GROUND_TRUTH_IMAGE` | Draws all the mask regions using their class index for pixel values. Unlabeled pixels are not drawn. Pixels must not belong to multiple regions that have different values for [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md). Note that the destination must be an image buffer and not a graphics list.

For anomaly detection, non-anomalous values are drawn with index 0, anomalous pixels are drawn with index 1 and all don't care pixels are drawn with index -1. |

*For modifying the draw entry operation*

| Value | Description |
| --- | --- |
| `M_GROUND_TRUTH_INDEX` | Draws the descriptor from the specified region using [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md) for pixel values. This combination value cannot be applied to [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassDrawEntry.md). |
| `M_NETWORK_OUTPUT_SIZE` | Draws according to the exact classification map size (output of the network) without scaling or expanding. In this case, the expected size of the destination buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md)) is the same as the X- and Y-size of [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md).

You cannot use [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md) with:

- [`M_DESCRIPTOR_TYPE_...`](../../Reference/class/MclassDrawEntry.md) drawing operations.
- [`M_CSNET_...`](../../Reference/class/MclassGetResultEntry.md) predefined classifiers.
- [`M_REPLICATE_BORDER`](../../Reference/class/MclassDrawEntry.md). |
| `M_PSEUDO_COLOR` | Draws the descriptor from the specified region using color specified by [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) for the class index of the region.

If the operation is set to [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassDrawEntry.md), [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) draws all the mask regions using colors specified in class definitions. This draws an image analogous to the input image of [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md) with [`M_GROUND_TRUTH_IMAGE_COLOR`](../../Reference/class/MclassEntryAddRegion.md). In this case, you can use [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) with other combination values. |
| `M_REPLICATE_BORDER` | Draws with borders expanded to the size of the destination buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md)).

You cannot use [`M_REPLICATE_BORDER`](../../Reference/class/MclassDrawEntry.md) with:

- [`M_DESCRIPTOR_TYPE_...`](../../Reference/class/MclassDrawEntry.md) drawing operations.
- [`M_CSNET_...`](../../Reference/class/MclassGetResultEntry.md) predefined classifiers.
- [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDrawEntry.md). |

*For modifying the ground truth*

| Value | Description |
| --- | --- |
| `M_NO_REGION_PIXELS` | Specifies that unlabeled pixels are assigned values according to [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md).

If[`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) is set to [`M_NO_CLASS`](../../Reference/class/MclassControl.md), pixels are set to -2. For an 8 + [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md) buffer, that resolves to 0xFE (that is, 254). Otherwise if [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) is set to an index, the pixel will be given the same value of this index.

Combined with [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md), this draws all the mask regions using the color specified in class definitions. If[`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) is set to [`M_NO_CLASS`](../../Reference/class/MclassControl.md), pixels are set to the color specified by [`M_NO_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md). This draws an image analogous to [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md) with [`M_GROUND_TRUTH_IMAGE_COLOR`](../../Reference/class/MclassEntryAddRegion.md), but draws unlabeled pixels. |

### `EntryIndex` *(in, AIL_INT64)*

Specifies the index of the entry to draw.

*For specifying the index of an entry*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the index of an entry is not required. |
| `Value >= 0` | Specifies the index of an entry. |

### `EntryKey` *(in, AIL_UUID)*

Specifies the key (_AIL_UUID_) of the entry to which regions are added. The key is defined as an Aurora Imaging Library universal unique identifier (UUID).

*For specifying the unique key of an entry*

| Value | Description |
| --- | --- |
| `M_DEFAULT_KEY` | Specifies that the unique key of an entry is not required. |
| `Value` | Specifies the unique key of an entry. |

### `TaskType` *(in, AIL_INT64)*

Specifies the task (image classification, segmentation, object detection, or anomaly detection) for which the drawing operation is performed. A dataset entry might contain results from more than one task.

*For specifying the type of task for which to draw results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_AUTO`](../../Reference/class/MclassDrawEntry.md). |
| `M_ANOMALY_DETECTION` | Specifies that you are drawing for anomaly detection. |
| `M_AUTO` | Specifies the task for which to get results automatically, given the content in the dataset used ([`DatasetContextClassId`](../../Reference/class/MclassDrawEntry.md)).

If the dataset selected is an images dataset, and it contains only image classification results,[`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_CLASSIFICATION`](../../Reference/class/MclassDrawEntry.md). If the dataset contains only segmentation results,[`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_SEGMENTATION`](../../Reference/class/MclassDrawEntry.md). If the dataset only contains object detection results, [`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_OBJECT_DETECTION`](../../Reference/class/MclassDrawEntry.md). If the dataset only contains anomaly detection results, [`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_ANOMALY_DETECTION`](../../Reference/class/MclassDrawEntry.md).

If the dataset contains two types of results, then [`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_SEGMENTATION`](../../Reference/class/MclassDrawEntry.md) (if segmentation results are present), or [`M_CLASSIFICATION`](../../Reference/class/MclassDrawEntry.md) (if no segmentation results are present).

If the dataset contains image classification, segmentation, and object detection results, [`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_SEGMENTATION`](../../Reference/class/MclassDrawEntry.md).

If the dataset contains segmentation and anomaly detection results, [`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_SEGMENTATION`](../../Reference/class/MclassDrawEntry.md).

If the dataset selected is a features dataset, [`M_AUTO`](../../Reference/class/MclassDrawEntry.md) is the same as [`M_CLASSIFICATION`](../../Reference/class/MclassDrawEntry.md), since segmentation and object detection are not supported for a features dataset.

To determine whether [`M_CLASSIFICATION`](../../Reference/class/MclassDrawEntry.md), [`M_SEGMENTATION`](../../Reference/class/MclassDrawEntry.md), [`M_OBJECT_DETECTION`](../../Reference/class/MclassDrawEntry.md), or [`M_ANOMALY_DETECTION`](../../Reference/class/MclassDrawEntry.md) results are available in the dataset, use the [`M_PREDICT_INFO`](../../Reference/class/MclassGetResultEntry.md) result. Typically, a dataset should be set up for a specific task (either image classification, segmentation, object detection, or anomaly detection). |
| `M_CLASSIFICATION` | Specifies that you are drawing for image classification. |
| `M_OBJECT_DETECTION` | Specifies that you are drawing for object detection. |
| `M_SEGMENTATION` | Specifies that you are drawing for segmentation. |

### `Index` *(in, AIL_INT64)*

Specifies what to use to perform the drawing operation. Set this parameter to one of the following values.

*For specifying what to use to perform the drawing operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the function call refers to the entry and not to a specific region or instance. The draw operation will be applied in a sequential manner to all regions or instances associated with the image.

> **Note:** Note, this is not available when [`Operation`](../../Reference/class/MclassDrawEntry.md) is set to [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDrawEntry.md). |
| `M_CLASS_INDEX` | Specifies that the drawing operation is for a specific class in the specified dataset context ([`DatasetContextClassId`](../../Reference/class/MclassDrawEntry.md)).

> **Note:** Note, this is not available when [`Operation`](../../Reference/class/MclassDrawEntry.md) is set to [`M_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassDrawEntry.md), [`M_DESCRIPTOR_TYPE_MASK`](../../Reference/class/MclassDrawEntry.md), [`M_DESCRIPTOR_TYPE_POLYGON`](../../Reference/class/MclassDrawEntry.md), or [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassDrawEntry.md). |
| `M_INSTANCE_INDEX` | Specifies that the drawing operation is for a specific instance in the specified dataset context ([`DatasetContextClassId`](../../Reference/class/MclassDrawEntry.md)).

> **Note:** Note, this is only available when [`Operation`](../../Reference/class/MclassDrawEntry.md)is set to [`M_DRAW_BOX`](../../Reference/class/MclassDrawEntry.md), [`M_DRAW_BOX_CENTER`](../../Reference/class/MclassDrawEntry.md), [`M_DRAW_BOX_NAME`](../../Reference/class/MclassDrawEntry.md), or [`M_DRAW_BOX_SCORE`](../../Reference/class/MclassDrawEntry.md). |
| `M_REGION_INDEX` | Specifies the function call refers to a specific region within the entry.

> **Note:** Note, this is only available when [`Operation`](../../Reference/class/MclassDrawEntry.md) is set to [`M_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassDrawEntry.md), [`M_DESCRIPTOR_TYPE_MASK`](../../Reference/class/MclassDrawEntry.md), or [`M_DESCRIPTOR_TYPE_POLYGON`](../../Reference/class/MclassDrawEntry.md). |

### `DstLabelArrayPtr` *(out, *AIL_INT)*

Specifies the address of the array in which to write the labels that have been automatically given to the copied or moved graphics in the destination graphics list. Set this parameter to `M_NULL` if the destination ([`DstImageBufOrListGraId`](../../Reference/class/MclassDrawEntry.md)) is an image buffer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to control the drawing operation.

*For specifying how to control the class score drawing*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. This is the only possible setting for all drawing operations except [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDrawEntry.md). |
| `M_BILINEAR_INTERPOLATION` | Scales the classification map with a bilinear calculation. The scale is a network property and established in receptive field calculations. The scaled image is placed at the offset of the receptive field. This value is only supported for the [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDrawEntry.md) drawing operation. |
| `M_RECEPTIVE_FIELD_PATCH` | Fills the corresponding receptive fields with calculated scores. Scores are drawn in order, from low to high. The best score is therefore drawn over lower scores. This value is only supported for the [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDrawEntry.md) drawing operation. |

This value only applies to an images dataset context.
