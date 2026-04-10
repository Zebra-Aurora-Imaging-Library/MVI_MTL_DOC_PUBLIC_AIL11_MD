---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Image_classification
section: Prediction_for_image_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Image_classification / Prediction for image classification"
---

# Prediction for image classification

[`MclassPredict`](../../Reference/class/MclassPredict.md) uses a trained classifier context to make class predictions on a target. For a trained CNN classifier, your target is either an image or a dataset of images.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information about prediction, see [Prediction settings, results, and drawings](../C50_ML_Prediction/S03_Prediction_settings_results_and_drawings.md). |

## Prepare for prediction

Allocate a classification result buffer to hold the prediction results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md). Image classification with ICNET classifiers requires target images that are the same size as the images used during training. The size of the training images can be inquired using [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_SIZE_X`](../../Reference/class/MclassInquire.md) and [`M_SIZE_Y`](../../Reference/class/MclassInquire.md).

Before predicting, preprocess your trained classifier context using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).

## Predict

Perform the prediction operation with the trained classifier context and the target data that you want to classify using [`MclassPredict`](../../Reference/class/MclassPredict.md). If your training images were prepared by cropping and resizing, then the target image must also be prepared in the same way. This maintains consistency between the training data and the target data. The size of the target images must be the same as the images used to train your ICNet classifier.

When performing the prediction operation with a dataset as your target, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

## Results

Retrieve the required results from the classification result buffer, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md). You can also get results from a specific entry in a dataset using [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md).

Typically, the most important prediction results to retrieve are the best predicted class ([`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResult.md)) and its score ([`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResult.md)).

### Drawing results

To draw prediction results, call [`MclassDraw`](../../Reference/class/MclassDraw.md). You can also draw prediction results from a specific entry in a dataset using [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md). You can perform drawing operations to, for example, illustrate the best class result ([`M_DRAW_BEST_INDEX_IMAGE`](../../Reference/class/MclassDraw.md)or [`M_DRAW_BEST_INDEX_CONTOUR_IMAGE`](../../Reference/class/MclassDraw.md)) or the resulting class scores ([`M_DRAW_BEST_SCORE_IMAGE`](../../Reference/class/MclassDraw.md) and [`M_DRAW_CLASS_SCORES`](../../Reference/class/MclassDraw.md)).

To draw the icon image related to the class, use [`M_DRAW_CLASS_ICON`](../../Reference/class/MclassDraw.md). Note, this image is not a result; you specify it using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md), and you draw it from a classifier context or a dataset context (not a result buffer). By drawing the class' icon image, you are able to visually identify the class for which you are getting results. Similarly, you can also specify and draw a color related to a class, to help visually identify it ([`M_DRAW_CLASS_COLOR_LUT`](../../Reference/class/MclassDraw.md)).

In general, drawing operations for prediction results can prove particularly useful when performing an image classification prediction with a child buffer or a buffer with a region.

## Assisted labeling

You can perform assisted labeling for image classification by adding prediction results to your dataset. For more information about assisted labeling, see [Assisted labeling](../C50_ML_Prediction/S04_Advanced_techniques.md).
