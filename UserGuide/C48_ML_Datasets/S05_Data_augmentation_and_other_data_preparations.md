---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Datasets
section: Data_augmentation_and_other_data_preparations
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Datasets / Data augmentation and other data preparations"
---

# Data augmentation and other data preparations

Ideally, your source dataset is initially filled with a sufficient quantity and quality of images for a successful training. In practice, successful training often requires that you further prepare your source data for training, for example, by augmenting your training images (such as adding translated, rotated, and blurred images), or cropping and resizing images (since training images should be the same size). Aurora Imaging Library facilitates these data modifications with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).

*[Image: MimAugment.png]*

You can call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with an images dataset or a specific image as your source (for image classification, segmentation, or object detection). You cannot call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with a features dataset (you cannot use it for feature classification). [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) does not support preparing signed images.

> **Note:** There is no dedicated data preparation for anomaly detection. However it can be possible to use [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md), or [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) to prepare your data. In this case, it is important to select the data preparation context that is consistent with your labeling. You can also can add augmented images to the training dataset using [`MimAugment`](../../Reference/im/MimAugment.md). For more information, see [Building a dataset for anomaly detection](../C54_ML_Anomaly_detection/S03_Building_a_dataset_for_anomaly_detection.md).

[`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) only performs augmentations (such as translations, rotations, and blur) if you specify a dataset as your source; if you specify an image as your source, augmentation settings are ignored, although cropping and resizing settings are applied. [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) only adds augmented images to the destination dataset if at least one augmentation operation is enabled; by default; no augmentations are enabled.

Augmented images should only be in your training dataset; do not allow them in your development or testing dataset, since errors can occur. Other preparations such as cropping and resizing can be made on images in any dataset, and are often required to meet image size requirements.

To hook functions to data preparation events, call [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

## Preparing for preparation

[`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) requires a data preparation context, which you must allocate depending on your machine learning task (that is, you must call [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) or [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md)). The data preparation context holds the settings with which to modify the source data ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)). To change the data preparation context settings, call [`MclassControl`](../../Reference/class/MclassControl.md). For example, you can use the [`M_SIZE_...`](../../Reference/class/MclassControl.md) controls to set the size of the images that [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) produces, or you can use the [`M_AUGMENT_NUMBER_ABSOLUTE`](../../Reference/class/MclassControl.md) control to set the number of augmented entries to add to the destination dataset.

> **Note:** You can consider data preparation ([`MclassPrepareData`](../../Reference/class/MclassPrepareData.md)) as taking the specified source data ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)), modifying it (according to your data preparation settings), and copying the modified version into the destination ([`DstDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)). One image in your source can result in multiple modified images in your destination, depending on your data preparation settings. Source data is not modified. Class definitions and authors are copied from the source dataset to the destination, if they do not exist in the destination.

To prepare your data with specific, preset augmentations, you can call [`MclassControl`](../../Reference/class/MclassControl.md) and enable the [`M_PRESET...`](../../Reference/class/MclassControl.md) augmentation operation settings. Alternatively, you can access additional types of augmentation operations using the data preparation context's internal augmentation context. This context is automatically managed by Aurora Imaging Library (you need not allocate it or free it) and is an internal version of the image processing context for augmentation (that is, [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md)). Note, some preset augmentations are not available for binary and grayscale images.

To specify the additional augmentations, you must first inquire the identifier of this internal augmentation context, by calling [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_AUGMENT_CONTEXT_ID`](../../Reference/class/MclassInquire.md). You can then use that augmentation context identifier with [`MimControl`](../../Reference/im/MimControl.md), and perform any of the augmentation operations within [`MimControl`](../../Reference/im/MimControl.md). The augmentations are applied when you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).

### Images produced

The modified (cropped/resized or augmented) images that [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) produces are placed in the specified dataset context or image buffer ([`DstDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)), depending on the source you want to prepare ([`SrcDatasetContextOrImageBufId`](../../Reference/class/MclassPrepareData.md)). The images are also placed in the destination folder, specified by the [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassControl.md) control. By default, each time you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), a new _PrepareX_ folder is created, where `_X_` is the next available value to create a unique folder name. You can change this behavior with the [`M_DESTINATION_FOLDER_MODE`](../../Reference/class/MclassControl.md) control.

All the images that [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) produces (and copies into the prepared data folder) are MIMs (Aurora Imaging Library images) and named as follows: `_OriginalName___Prp___AugmentationNumber_.mim`. For example, if [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) produces three augmentations of _Abyssinian.mim_, they would be named _Abyssinian_Prp_1.mim_, _Abyssinian_Prp_2.mim_, and _Abyssinian_Prp_3.mim_. The suffix '0' is reserved for images that are prepared but not augmented (for example, _Abyssinian_Prp_0.mim_ would be a cropped/resized copy of the original image).

## Augmentation

Data augmentation is a technique to synthetically generate more samples (entries) from a limited dataset to be used during training. Therefore, if you do not have enough training data, you can use augmentation to synthesize more. The ultimate goal is to have training data that is a full representation of all expected variations and conditions. If you cannot get the actual data, augmentation can be a solution.

Data augmentation can prevent overfitting and can improve accuracy as well as robustness. Various types of data augmentation are available, such as:

- Transformation-related.
  Examples include translation, rotation, flip, shearing, aspect ratio, and scale.
- Intensity-related.
  Examples include smoothing, adding noise, brightness variations.

The application of a specific augmentation type depends on the problem definition. The augmentation should make sense in the context of the application. For instance, if the rotation of an object does not happen in the real application or it changes the label of the image, then rotation should not be applied as an augmentation (or, only a few degrees of rotation could be used, if necessary).

The following bottle cap inspection example illustrates the point. In this case, the original image is shown first, then, a rotation of 180 degrees is shown, but this actually can never happen in the application, so it must not be included in the set of augmented images that you use. Finally, the third image shows an 10 degree rotation, which can happen (the bottle might shake and tilt on the conveyor), so this augmentation can be used.

*[Image: MclassTheoreticalAugmentation.png]*

An example of an augmentation changing the label of an image is a 180 degree rotation performed on an image of the number 6. The image would then represent the number 9, which has a different label. This type of augmentation should be avoided.

Other types of data augmentations might be needed in addition to just supplementing original variations. For example, over time, images might be subject to change of focus and noise. These changes could have a noticeable impact on the network's accuracy. Hence, to overcome this problem, these artifacts should also be simulated through data augmentation.

In some cases, the original dataset is not balanced. This means that there is a significant difference in quantity of labeled data between the different classes. As a result, the network might give too much importance to the class with more data. One solution to an imbalanced dataset is data augmentation.

For example, 1000 images are available for class A (non-defective parts) and only 50 images are available for class B (defective parts). In this situation, more data augmentation could be applied for class B to narrow the gap between the two classes by setting [`M_AUGMENT_BALANCING`](../../Reference/class/MclassControl.md) with [`MclassControl`](../../Reference/class/MclassControl.md). This balancing is then applied when you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).

Such augmentations, to a single class, require especially prudent considerations, since you do not want the classifier to unrealistically learn how to identify that class (the augmentations must always reflect the overall problem). This in part explains why, as previously mentioned, you must not add augmented data to the development dataset or the testing dataset, since they are used to regulate and help you identify misguided classifications.

For more information about setting up, performing, and analyzing an augmentation for images, see [Augmentation](../C05_Specialized_image_processing/S11_Augmentation.md).

Note the following recommendations:

- Only allow augmented entries in the training dataset. In this case, augmented entries also refers to the entries used to augment them. All of these entries must only be in the training dataset.
- Augment your data after splitting it into different datasets. It is not recommended to call [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) to create the development dataset or testing dataset if your source dataset contains augmented entries.
  Identify augmented entries by calling [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_AUGMENTATION_SOURCE`](../../Reference/class/MclassControlEntry.md).
- Disregard bagging information if your training dataset has augmented entries.
- Keep the source entry and its augmented entries in one dataset. The source and its variations are considered part of the augmentation.

Note, original images should be a little larger than those in the final application, to ensure that augmented images do not contain overscan pixels. In the final training dataset, you can crop the images to meet the application's size requirements.
