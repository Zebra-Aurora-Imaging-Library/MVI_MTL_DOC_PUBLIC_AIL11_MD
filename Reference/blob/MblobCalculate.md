---
doctype: Reference
module: blob
function: MblobCalculate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / blob / MblobCalculate"
---

# MblobCalculate

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

> Perform blob analysis calculations.

## Syntax

```c
void MblobCalculate(
    AIL_ID ContextBlobId,        //in
    AIL_ID BlobIdentImageBufId,  //in
    AIL_ID GrayImageBufId,       //in
    AIL_ID ResultBlobId          //out
)
```

## Description

This function calculates the specified features for all currently included blobs in the blob identifier image, and stores results in the specified result buffer. To specify the features to calculate, pass a blob analysis context that has the required features enabled for calculation. Use [`MblobControl`](../../Reference/blob/MblobControl.md) to enable the features. You can select the blobs on which to perform the calculations using [`MblobSelect`](../../Reference/blob/MblobSelect.md).

Calculations of features with a binary definition (such as [`M_BREADTH`](../../Reference/blob/MblobControl.md)) are performed using only the blob identifier image ([`BlobIdentImageBufId`](../../Reference/blob/MblobCalculate.md)). You can limit blob analysis calculations to a region of the blob identifier image using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). In this case, the specified features are only calculated for included blobs (or parts of blobs) lying within the ROI. For more information, see [Regions of interest](../../UserGuide/C02_Building_an_application/S10_Child_buffers_regions_of_interest_and_fixturing.md).

If a feature with a grayscale definition (such as [`M_MAX_PIXEL`](../../Reference/blob/MblobControl.md)) is enabled for calculation, you must pass a grayscale image buffer to the [`GrayImageBufId`](../../Reference/blob/MblobCalculate.md) parameter. In this case, the blob identifier image is used to identify the blobs and the grayscale image is used to provide the blob pixel values. Note that the blob identifier image and the grayscale image must be the same size. Instead of passing the blob identifier image separately, you can pass an image buffer (or container) with an ROI that identifies the blobs to the [`GrayImageBufId`](../../Reference/blob/MblobCalculate.md) parameter. In this case, the [`BlobIdentImageBufId`](../../Reference/blob/MblobCalculate.md) parameter must be set to [`M_NULL`](../../Reference/blob/MblobCalculate.md).

When calculating depth map related features (using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_DEPTH_MAP_...`](../../Reference/blob/MblobControl.md)), you must set [`BlobIdentImageBufId`](../../Reference/blob/MblobCalculate.md) to [`M_NULL`](../../Reference/blob/MblobCalculate.md), and [`GrayImageBufId`](../../Reference/blob/MblobCalculate.md) to a fully corrected depth map image buffer or 3D-processable depth map container that is associated with an ROI that acts as the blob identifier image as well as identifies invalid pixels. Note that if you don't pass a fully corrected depth map image buffer or 3D-processable depth map container, you will obtain an error if you try to retrieve a depth map feature result using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md); [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) will not generate an error. Use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`](../../Reference/buf/MbufSetRegion.md) to associate an ROI with the specified depth map image. Note that for a depth map container, the ROI is stored in the [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component of the container.

The blob identifier image identifies each blob as a group of touching pixels in the current foreground state (zero or non-zero, based on [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/blob/MblobControl.md)). The current [`M_CONNECTIVITY`](../../Reference/blob/MblobControl.md) processing mode (also set with [`MblobControl`](../../Reference/blob/MblobControl.md)), determines when to consider pixels as touching.

If the blob identifier image has been previously binarized so that it contains only two extreme values (0 and 1 for 1-bit images, 0 and 0xff for 8-bit images, and 0 and 0xffff for 16-bit images), blob analysis can be performed a little faster. However, you must first set [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_IDENTIFIER_TYPE`](../../Reference/blob/MblobControl.md) to [`M_BINARY`](../../Reference/blob/MblobControl.md), to let Aurora Imaging Library know that the blob identifier image has only two states.

Depending on the blob identification mode ([`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md)), [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) either treats each blob individually ([`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md)), groups all blobs together ([`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md)), groups blobs according to their actual pixel value in the blob identifier image ([`M_LABELED`](../../Reference/blob/MblobControl.md) or [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md)). Both [`M_LABELED`](../../Reference/blob/MblobControl.md) and [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md) treat blobs with the same pixel value as being grouped together. However, [`M_LABELED`](../../Reference/blob/MblobControl.md) treats touching blobs as the same blob (regardless of their label); while [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md) treats them as separate.

If you are calculating chain features (enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md) or [`M_ALL_FEATURES`](../../Reference/blob/MblobControl.md)), you cannot use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) set to [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md).

If several calls are made to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) with the same image or container, context, and result buffer, features calculated in one call remain in the result buffer and are not recalculated in subsequent calls. However, if you then use the result buffer with different images or a different container, or if you change the blob analysis context's processing settings using [`MblobControl`](../../Reference/blob/MblobControl.md) (constants from the [`ProcSettings`](../../Reference/blob/MblobControl.md) table), any results already in the buffer become invalid and are discarded. Therefore, it is more efficient to use a result buffer exclusively in one processing mode with the same images/container.

If you have associated the blob identifier image with a camera calibration context, and [`GrayImageBufId`](../../Reference/blob/MblobCalculate.md) is set to [`M_NULL`](../../Reference/blob/MblobCalculate.md), the result buffer will be associated with the same camera calibration context as the blob identifier image. However, if you provide a grayscale image that is not calibrated or does not have the same camera calibration context as the blob identifier image, the result buffer will not be calibrated (both the identifier and grayscale images must have the same camera calibration context for the result buffer to be calibrated).

Most feature calculations are initially performed in the pixel coordinate system. It is typically possible to transform the result of these calculations to the relative coordinate system using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md). Those feature types whose calculation are not performed in the pixel coordinate system, or that cannot be transformed into the relative coordinate system, will have that fact stated in the result type's description in [`MblobGetResult`](../../Reference/blob/MblobGetResult.md). Refer to the description of a particular feature's result, in [`MblobGetResult`](../../Reference/blob/MblobGetResult.md), to determine if it can be transformed and returned in the relative coordinate system. Note that some feature results such as [`M_NUMBER_OF_HOLES`](../../Reference/blob/MblobGetResult.md) do not have a different meaning in either the pixel coordinate system or the relative coordinate system.

## Parameters

### `ContextBlobId` *(in, AIL_ID)*

Specifies the identifier of the blob analysis context, that specifies the feature(s) to calculate, and other settings related to the blob analysis operation. The blob analysis context must have been previously allocated on the required system with [`MblobAlloc`](../../Reference/blob/MblobAlloc.md).

### `BlobIdentImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to use as the blob identifier image. This image is used to distinguish blob pixels from background pixels and which pixels belong to which blob. The blob identifier image buffer must be a packed binary, 8-, 16-, or 32-bit unsigned, single-band image buffer. 32-bit image buffers are only supported when the blob identification mode is set to [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md).

### `GrayImageBufId` *(in, AIL_ID)*

Specifies the identifier of the depth map container or the image buffer to use as the grayscale image. This buffer is used to supply the intensity of blob pixels when calculating features with a grayscale definition. If this parameter is set to `M_NULL`, features with a grayscale definition, such as [`M_SUM_PIXEL`](../../Reference/blob/MblobControl.md), cannot be calculated. This parameter is ignored when calculating features with only a binary definition.

### `ResultBlobId` *(out, AIL_ID)*

Specifies the identifier of a blob analysis result buffer, in which to store calculated results. The blob analysis result buffer must have been previously allocated on the required system using [`MblobAllocResult`](../../Reference/blob/MblobAllocResult.md).
