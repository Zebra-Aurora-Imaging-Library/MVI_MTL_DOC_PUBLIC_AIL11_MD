---
doctype: Reference
module: class
function: MclassPrepareData
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassPrepareData"
---

# MclassPrepareData

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

> Prepare images in a dataset, or individual images, for training or prediction.

## Syntax

```c
void MclassPrepareData(
    AIL_ID    PrepareDataContextClassId,      //in
    AIL_ID    SrcDatasetContextOrImageBufId,  //in
    AIL_ID    DstDatasetContextOrImageBufId,  //out
    AIL_ID    ClassifierContextClassId,       //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function creates prepared (modified) versions of images in a dataset for training ([`MclassTrain`](../../Reference/class/MclassTrain.md)), or an individual image for prediction ([`MclassPredict`](../../Reference/class/MclassPredict.md)). You can, for example, use this function to resize or crop images, or add augmented images to a dataset. Some preparations are only available for training.

[`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) requires a data preparation context ([`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) or [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md)) that is preprocessed ([`MclassPreprocess`](../../Reference/class/MclassPreprocess.md)). To inquire the context's preprocessing state, call [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_PREPROCESSED`](../../Reference/class/MclassInquire.md). Note that [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) does not support preparing signed images. Image classification and segmentation support buffers with up to 1024 bands.

The data preparation context ([`PrepareDataContextClassId`](../../Reference/class/MclassPrepareData.md)) holds the settings with which to modify the specified source ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)). To change the data preparation context settings, call [`MclassControl`](../../Reference/class/MclassControl.md). For example, you can use the [`M_SIZE_...`](../../Reference/class/MclassControl.md) controls to set the size of the images that [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) produces, or you can use the [`M_AUGMENT_NUMBER_ABSOLUTE`](../../Reference/class/MclassControl.md) control to set the number of augmented entries to add to the destination dataset.

> **Note:** You can consider data preparation as taking the specified source data ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)), modifying it (according to your data preparation settings), and copying the modified version into the specified destination ([`DstDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)). One image in your source can result in multiple modified images in your destination, depending on your data preparation settings. Source data is not modified. Class definitions and authors are copied from the source dataset to the destination, if they do not exist in the destination. If using a newly allocated destination dataset for segmentation, control values for [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md), [`M_NO_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md), and [`M_DONT_CARE_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) are copied to the destination; otherwise, these values must be the same for the source and destination datasets.

To prepare your data with specific, preset augmentations, you can call [`MclassControl`](../../Reference/class/MclassControl.md) and enable the [`M_PRESET...`](../../Reference/class/MclassControl.md) augmentation operation setting. Some preset augmentation settings are not available for binary (1-bit) and grayscale (1-band) images. Alternatively, you can access additional types of augmentation operations using the data preparation context's internal augmentation context. This context is automatically managed by Aurora Imaging Library (you need not allocate it or free it) and is an internal version of the image processing context for augmentation (that is, [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md)).

To specify the additional augmentations, you must first inquire the identifier of this internal augmentation context, by calling [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_AUGMENT_CONTEXT_ID`](../../Reference/class/MclassInquire.md). You can then use that augmentation context identifier with [`MimControl`](../../Reference/im/MimControl.md) and enable the augmentation operation ([`M_AUG_..._OP`](../../Reference/im/MimControl.md)).

Aurora Imaging Library only performs augmentations on a source dataset ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)); if you specify a source image buffer, augmentation settings are ignored (cropping/resizing settings are applied). [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) only adds augmented images to the destination dataset if at least one augmentation operation is enabled; by default; no augmentations are enabled.

The modified (cropped/resized or augmented) images that this function produces are placed in the specified dataset context or image buffer ([`DstDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)), depending on the source you want to prepare ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)). The images are also placed in the destination folder, specified by the [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassControl.md) control. By default, each time you call this function, a new _PrepareX_ folder is created, where `_X_` is the next available value to create a unique folder name. You can change this behavior with the [`M_DESTINATION_FOLDER_MODE`](../../Reference/class/MclassControl.md) control.

All the images that this function produces (and copies into the prepared data folder) will have a MIM file extension and be named as follows: `_OriginalName___Prp___AugmentationNumber_.mim`. For example, if [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) produces three augmentations of _Abyssinian.mim_, they would be named _Abyssinian_Prp_1.mim_, _Abyssinian_Prp_2.mim_, and _Abyssinian_Prp_3.mim_. The suffix '0' is reserved for images that are prepared but not augmented (for example, _Abyssinian_Prp_0.mim_ would be a cropped/resized copy of the original image).

To hook functions to data preparation events, call [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

## Parameters

### `PrepareDataContextClassId` *(in, AIL_ID)*

Specifies the identifier of the data preparation context that this function uses to modify the source image data. This context must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md), or [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md) and it must be in a preprocessed state before calling [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).

### `SrcDatasetContextOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source images dataset context or image buffer. You must have previously allocated the specified source on the required system using either [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md) with [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md).

### `DstDatasetContextOrImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination images dataset context or image buffer. The type of destination must be same as the source ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)); that is, the destination must be an image dataset context if the source is an image dataset context, and an image buffer if the source is an image buffer. You cannot specify the same dataset or image for both the [`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md) and [`DstDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md) parameters.

### `ClassifierContextClassId` *(in, AIL_ID)*

Specifies the identifier of the predefined CNN, predefined object detection, or predefined segmentation classifier context that this function uses to prepare your image data. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md). To not use a classifier context, or if the size mode specified by [`MclassControl`](../../Reference/class/MclassControl.md) is [`M_USER_DEFINED`](../../Reference/class/MclassControl.md), you must set this parameter to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
