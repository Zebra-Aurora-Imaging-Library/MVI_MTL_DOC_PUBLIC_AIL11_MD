---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Segmentation
section: Prediction_for_segmentation
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Coarse_segmentation / Prediction for segmentation"
---

# Prediction for segmentation

[`MclassPredict`](../../Reference/class/MclassPredict.md) uses a trained classifier context to make class predictions on a target. For a trained segmentation classifier, your target is either an image or a dataset of images.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information about prediction, see [Prediction settings, results, and drawings](../C50_ML_Prediction/S03_Prediction_settings_results_and_drawings.md). |

## Prepare for prediction

Allocate a classification result buffer to hold the prediction results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md). When predicting with a segmentation classifier, the target image size can be smaller or bigger than the images used to train the classifier. Some CSNet classifiers perform better with smaller or bigger images, so you should set the target image size accordingly by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md).

When predicting with a dataset as your target, you must set the control [`M_SEGMENTATION_FOLDER`](../../Reference/class/MclassControl.md) for the target dataset. Segmentation prediction scores are saved at this location.

Before predicting, preprocess your trained classifier context using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).

## Predict

Perform the prediction operation with the trained classifier context and the target data that you want to classify using [`MclassPredict`](../../Reference/class/MclassPredict.md). If your training images were prepared by cropping and resizing, then the target image must also be prepared in the same way. This maintains consistency between the training data and the target data. By default, Aurora Imaging Library assumes that the target images are the same size as the training images, which you can retrieve using [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_SIZE_X`](../../Reference/class/MclassInquire.md) and [`M_SIZE_Y`](../../Reference/class/MclassInquire.md). The target images can also be smaller or bigger than the images used to train your classifier. This requires calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md). Typically, cropping is the only valid method for predicting on different sized images. Resized images should not be used for prediction unless your classifier was trained using similarly resized images.

When performing the prediction operation with a dataset as your target, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

## Results

Retrieve the required results from the classification result buffer, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md). You can also get results from a specific entry in a dataset using [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md).

Typically, the most important prediction results to retrieve are the best predicted class ([`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResult.md)) and its score ([`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResult.md)).

### Drawing results

To draw prediction results, call [`MclassDraw`](../../Reference/class/MclassDraw.md). You can also draw prediction results from a specific entry in a dataset using [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md). You can perform drawing operations to, for example, illustrate the best class result ([`M_DRAW_BEST_INDEX_IMAGE`](../../Reference/class/MclassDraw.md)or [`M_DRAW_BEST_INDEX_CONTOUR_IMAGE`](../../Reference/class/MclassDraw.md)) or the resulting class scores ([`M_DRAW_BEST_SCORE_IMAGE`](../../Reference/class/MclassDraw.md) and [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDraw.md)).

To draw the icon image related to the class, use [`M_DRAW_CLASS_ICON`](../../Reference/class/MclassDraw.md). Note, this image is not a result; you specify it using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md), and you draw it from a classifier context or a dataset context (not a result buffer). By drawing the class' icon image, you are able to visually identify the class for which you are getting results. Similarly, you can also specify and draw a color related to a class, to help visually identify it ([`M_DRAW_CLASS_COLOR_LUT`](../../Reference/class/MclassDraw.md)).

Prediction results for segmentation can prove complex to decipher, given that you can have multiple results for each training image. For example, a segmentation prediction can identify two classes in the following target image.

*[Image: MclassSegDrawingClassNone.png]*

By calling [`MclassDraw`](../../Reference/class/MclassDraw.md), you can draw a contour around the classes found, as well as the corresponding color of every class, including the background. Such drawings help you better understand the results of the prediction.

*[Image: MclassSegDrawingClassContourAndColor.png]*

To perform these drawing operations, specify [`M_DRAW_BEST_INDEX_CONTOUR_IMAGE`](../../Reference/class/MclassDraw.md) + [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md) (for the image on the left) and [`M_DRAW_BEST_INDEX_IMAGE`](../../Reference/class/MclassDraw.md) + [`M_PSEUDO_COLOR`](../../Reference/class/MclassDraw.md) (for the image on the right).

## Assisted labeling

You can perform assisted labeling for segmentation by adding prediction results to your dataset. For more information about assisted labeling, see [Assisted labeling](../C50_ML_Prediction/S04_Advanced_techniques.md).
