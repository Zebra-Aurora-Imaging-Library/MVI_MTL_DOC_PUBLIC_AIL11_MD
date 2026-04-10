---
doctype: Reference
module: class
function: MclassDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassDraw"
---

# MclassDraw

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

> Draw specific features of a classifier context, a dataset context, or a prediction result buffer.

## Syntax

```c
void MclassDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextOrResultClassId,  //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT64 Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of a classifier context, a dataset context, or a prediction result buffer. Drawing operations are typically performed in a destination image buffer. You cannot draw from a tree ensemble result buffer, and you cannot perform any drawing operations with ONNX.

By default, this function performs the drawing operation for every class in the specified context or result. To draw for a specific class or specific instance of a class, specify its index ([`Index`](../../Reference/class/MclassDraw.md)).

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultClassId` *(in, AIL_ID)*

Specifies the identifier of the classifier or dataset context, or prediction result buffer from which to draw.

*For specifying the classifier or dataset context, or prediction result buffer*

| Value | Description |
| --- | --- |
| `ContextClassifierPredefinedId` | Specifies the identifier of the predefined CNN, object detection, segmentation, or anomaly detection classifier context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md),[`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md). |
| `ContextClassifierTreeEnsembleId` | Specifies the identifier of a tree ensemble classifier context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `ContextDatasetId` | Specifies the identifier of an images or features dataset context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md). |
| `ResultClassPredictId` | Specifies the identifier of the CNN, object detection, segmentation, or anomaly detections, prediction result buffer. This buffer is allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md) [`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md). |

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or LUT buffer in which to draw. You should typically specify an image, which can be any valid Aurora Imaging Library image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). By drawing into a display's overlay buffer, you can also annotate an image non-destructively. To specify a LUT, you must use the [`M_DRAW_CLASS_COLOR_LUT`](../../Reference/class/MclassDraw.md) drawing operation.

### `Operation` *(in, AIL_INT64)*

Specifies the drawing operation to perform.

*For a predefined (CNN, segmentation, or object detection) or tree ensemble classifier context, or a dataset context (images or features)*

| Value | Description |
| --- | --- |
| `M_DRAW_CLASS_COLOR_LUT` | Draws the color (LUT) associated to the class. This color is defined using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md).

When performing this operation, you must set the destination buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md)) to the identifier of an Aurora Imaging Library LUT buffer. To inquire the width of this LUT, use [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassInquire.md). |
| `M_DRAW_CLASS_ICON` | Draws the icon image associated to the class. This image is defined using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md).

When performing this operation, you must set the destination buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md)) to the identifier of an Aurora Imaging Library image buffer. To inquire the format of this buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md)), such as the X- and Y-size, use [`M_CLASS_ICON_ID`](../../Reference/class/MclassInquire.md) (the dimension of the destination image in which you draw should be the same as the class description's icon image). |

*For a CNN or segmentation prediction result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_BEST_INDEX_CONTOUR_IMAGE` | Draws the contours of the best classes. If you do not specify the [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md) combination value, Aurora Imaging Library uses the result type [`M_BEST_INDEX_IMAGE_TYPE`](../../Reference/class/MclassGetResult.md) to determine the type of the image to use. If you specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md), the image type must be 3 bands, 8-bit unsigned.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of prediction result buffer that you are using:

- For CNN prediction results, you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md), [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md), and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDraw.md).
- For segmentation prediction results, you can specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md). |
| `M_DRAW_BEST_INDEX_IMAGE` | Draws an image using the index value of the best class at each point. If you do not specify the [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md) combination value, Aurora Imaging Library uses the result type [`M_BEST_INDEX_IMAGE_TYPE`](../../Reference/class/MclassGetResult.md) to determine the type of the image to use. If you specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md), the image type must be 3 bands, 8-bit unsigned.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of prediction result buffer that you are using:

- For CNN prediction results, you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md), [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md), and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDraw.md).
- For segmentation prediction results, you can specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md). |
| `M_DRAW_BEST_SCORE_IMAGE` | Draws an image using the best score at each point. If you do not specify the [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md) combination value, the image type must be float or unsigned. If you specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md), the image type must be 3 bands, 8-bit unsigned.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of prediction result buffer that you are using:

- For CNN prediction results, you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md), [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md), and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDraw.md).
- For segmentation prediction results, you can specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md). |
| `M_DRAW_CLASS_SCORES` | Draws (fills) an image using the scores of a specific class. This drawing operation is not possible when drawing general results; you must set the [`Index`](../../Reference/class/MclassDraw.md) parameter to a specific class.

Note, this drawing operation supports combination values from the [`draw`](../../Reference/draw.md) table. The combination values that you can specify depend on the type of prediction result buffer that you are using:

- For CNN prediction results, you can specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md) and [`M_REPLICATE_BORDER`](../../Reference/class/MclassDraw.md).
  If you specify [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md), the drawing is done with the classification map size (the [`ControlFlag`](../../Reference/class/MclassDraw.md) parameter must be set to its default). If you do not specify the [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md) combination value, you can set the [`ControlFlag`](../../Reference/class/MclassDraw.md) parameter to [`M_BILINEAR_INTERPOLATION`](../../Reference/class/MclassDraw.md) or [`M_RECEPTIVE_FIELD_PATCH`](../../Reference/class/MclassDraw.md).
  In all cases, when using [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md), the image type must be float or 8-bit unsigned.
- For segmentation prediction results, you cannot specify any combination value.
- You cannot specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md) for any predefined classifier. |

*For an object detection prediction result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_BOX` | Draws the bounding boxes for all instances, a specific class, or a specific instance (only one box will be drawn in this case). The drawn color is specified in the class definitions. To change the line thickness of the box, you can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) on the [`ContextGraId`](../../Reference/class/MclassDraw.md). |
| `M_DRAW_BOX_CENTER` | Draws the center positions of the bounding boxes for all instances, a specific class, or a specific instance (only one center position will be drawn in this case). The drawn color is specified in the class definitions. To change the line thickness of the center position, you can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md) with the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter. |
| `M_DRAW_BOX_NAME` | Draws the class names for bounding boxes for all instances, a specific class, or a specific instance (only one name will be drawn in this case).

The class names are drawn using the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter. You can modify the text settings of the drawn names using [`MgraControl`](../../Reference/gra/MgraControl.md).

If used with [`M_DRAW_BOX`](../../Reference/class/MclassDraw.md), the class name is drawn on top of the outline of the bounding box. If used with [`M_DRAW_BOX_CENTER`](../../Reference/class/MclassDraw.md), the class name is drawn near the center of the bounding box. Text alignment controls set with [`MgraControl`](../../Reference/gra/MgraControl.md) are relative to these locations. |
| `M_DRAW_BOX_SCORE` | Draws the class scores for bounding boxes for all instances, a specific class, or a specific instance (only one score will be drawn in this case).

The class scores are drawn using the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter. You can modify the text settings of the drawn scores using [`MgraControl`](../../Reference/gra/MgraControl.md).

If used with [`M_DRAW_BOX`](../../Reference/class/MclassDraw.md), the class score is drawn on top of the outline of the bounding box. If used with [`M_DRAW_BOX_CENTER`](../../Reference/class/MclassDraw.md), the class score is drawn near the center of the bounding box. Text alignment controls set with [`MgraControl`](../../Reference/gra/MgraControl.md) are relative to these locations. |

*For an anomaly detection prediction result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_ANOMALY_CONTOUR_MASK` | Draws the contour of the anomalies using the color of the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter. To change the color, you can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) on the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md) must be an image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md). Additionally, if the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter is not grayscale, [`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md) must be 3 bands and 8-bit. |
| `M_DRAW_ANOMALY_HEATMAP` | Draws the anomaly scores projected as a heat map for each pixel location in the image.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md) must be an 8-bit and 3-band image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md). |
| `M_DRAW_ANOMALY_MASK` | Draws the anomalous pixels using the color of the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter. To change the color, you can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) on the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md) must be an image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md). Additionally, if the graphics context specified by the [`ContextGraId`](../../Reference/class/MclassDraw.md) parameter is not grayscale, [`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md) must be 3 bands and 8-bit. |
| `M_DRAW_ANOMALY_SCORES` | Draws the anomaly score for each pixel location in the image.

Note, [`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md) must be a 1-band image buffer of type [`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |

*For modifying the drawing operation*

| Value | Description |
| --- | --- |
| `M_NETWORK_OUTPUT_SIZE` | Draws according to the exact classification map size (output of the network) without scaling or expanding. In this case, the expected size of the destination buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md)) is the same as the X- and Y-size of [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) and [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md).

You cannot use [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md) with [`M_REPLICATE_BORDER`](../../Reference/class/MclassDraw.md). |
| `M_PSEUDO_COLOR` | Draws using the pseudo colors defined by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md). For all drawing operations, if you specify [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md), the destination buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md)) image type must be 3 bands, 8-bit unsigned.

Depending on the draw operation, you can use [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md) with other combination values. |
| `M_REPLICATE_BORDER` | Draws with borders expanded to the size of the destination buffer ([`DstImageBufOrListGraId`](../../Reference/class/MclassDraw.md)).

You cannot use [`M_REPLICATE_BORDER`](../../Reference/class/MclassDraw.md) with [`M_NETWORK_OUTPUT_SIZE`](../../Reference/class/MclassDraw.md). |

### `Index` *(in, AIL_INT64)*

Specifies what to use to perform the drawing operation.

*For specifying what to use to perform the drawing operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CONTEXT`](../../Reference/class/MclassDraw.md), if the [`ContextOrResultClassId`](../../Reference/class/MclassDraw.md) parameter is a predefined CNN, segmentation, object detection, or anomaly detection classifier context, a tree ensemble classifier context, an images dataset context, or a features dataset context; same as [`M_GENERAL`](../../Reference/class/MclassDraw.md), if the [`ContextOrResultClassId`](../../Reference/class/MclassDraw.md) is a CNN, segmentation, object detection, or anomaly detection prediction result buffer. |
| `M_CLASS_INDEX` | Specifies that the drawing operation is for a specific class in the specified context or result buffer ([`ContextOrResultClassId`](../../Reference/class/MclassDraw.md)).

You can draw for a specific class when specifying a CNN prediction result buffer ([`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), an object detection prediction result buffer ([`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)), a segmentation prediction result buffer ([`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)), a predefined CNN classifier context ([`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a predefined object detection classifier context ([`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a predefined segmentation classifier context ([`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a tree ensemble classifier context ([`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)), an images dataset context([`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md)), or a features dataset context ([`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md)). |
| `M_INSTANCE_INDEX` | Specifies that the drawing operation is for a specific instance in the specified object detection prediction result buffer ([`ContextOrResultClassId`](../../Reference/class/MclassDraw.md)). |
| `M_CONTEXT` | Specifies that the drawing operation is for every class in the classifier context or the dataset context ([`ContextOrResultClassId`](../../Reference/class/MclassDraw.md)). |
| `M_GENERAL` | Specifies that the drawing operation is for every class in the CNN, segmentation, object detection, or anomaly detection prediction result buffer ([`ContextOrResultClassId`](../../Reference/class/MclassDraw.md)). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to control the drawing operation.

*For specifying how to control the class score drawing*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. This is the only possible setting for all drawing operations except [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDraw.md). |
| `M_BILINEAR_INTERPOLATION` | Scales the classification map with a bilinear calculation, when using [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDraw.md). The scale is established in receptive field calculations. The scaled image is placed at the offset of the receptive field. |
| `M_RECEPTIVE_FIELD_PATCH` | Fills the corresponding receptive fields with calculated scores, when using [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDraw.md). Scores are drawn in order, from low to high. The best score is therefore drawn over lower scores. |
