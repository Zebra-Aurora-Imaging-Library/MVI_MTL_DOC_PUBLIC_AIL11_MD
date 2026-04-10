---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Object_detection
section: Prediction_for_object_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Object_detection / Prediction for object detection"
---

# Prediction for object detection

[`MclassPredict`](../../Reference/class/MclassPredict.md) uses a trained classifier context to make class predictions on a target. For a trained object detection classifier, your target is either an image or a dataset of images.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information about prediction, see [Prediction settings, results, and drawings](../C50_ML_Prediction/S03_Prediction_settings_results_and_drawings.md). |

## Prepare for prediction

Allocate a classification result buffer to hold the prediction results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md). When predicting with an object detection classifier, the target image size must be the same size as the images used to train the classifier.

Before predicting, preprocess your trained classifier context using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).

## Predict

Perform the prediction operation with the trained classifier context and the target data that you want to classify using [`MclassPredict`](../../Reference/class/MclassPredict.md). If your training images were prepared by cropping and resizing, then the target image must also be prepared in the same way. This maintains consistency between the training data and the target data. By default, Aurora Imaging Library assumes that the target images are the same size as the training images used to train your ODNet classifier, which you can retrieve using [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_SIZE_X`](../../Reference/class/MclassInquire.md) and [`M_SIZE_Y`](../../Reference/class/MclassInquire.md).

When performing the prediction operation with a dataset as your target, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

To prevent the classification process from taking too long, you can set a maximum calculation time for [`MclassPredict`](../../Reference/class/MclassPredict.md), by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TIMEOUT`](../../Reference/class/MclassControl.md). You can also stop the current execution of [`MclassPredict`](../../Reference/class/MclassPredict.md) (from another thread of higher priority), by specifying[`M_STOP_PREDICT`](../../Reference/class/MclassControl.md).

Note, if you have a testing dataset, you should predict with it, using [`MclassPredict`](../../Reference/class/MclassPredict.md). As previously discussed, predicting with a testing dataset serves as a quarantined final check for your trained classifier. If the results are what you expect (they should be approximately the same as your training results), you can continue with prediction using your trained classifier. If the results are not what you expect, it is a sign that you should retrain the classifier.

## Results

Retrieve the required results from the classification result buffer, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md). You can also get results from a specific entry in a dataset using [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md).

Typically, the most important prediction results to retrieve are the best predicted class ([`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResult.md)), its score ([`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResult.md)), and the coordinates and shape of the bounding boxes ([`M_BOX_4_CORNERS`](../../Reference/class/MclassGetResult.md), [`M_CENTER_...`](../../Reference/class/MclassGetResult.md), [`M_HEIGHT`](../../Reference/class/MclassGetResult.md), or [`M_WIDTH`](../../Reference/class/MclassGetResult.md)).

### Drawing results

To draw prediction results, call [`MclassDraw`](../../Reference/class/MclassDraw.md). You can also draw prediction results from a specific entry in a dataset using [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md).

To draw the icon image related to the class, use [`M_DRAW_CLASS_ICON`](../../Reference/class/MclassDraw.md). Note, this image is not a result; you specify it using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md), and you draw it from a classifier context or a dataset context (not a result buffer). By drawing the class' icon image, you are able to visually identify the class for which you are getting results. Similarly, you can also specify and draw a color related to a class, to help visually identify it ([`M_DRAW_CLASS_COLOR_LUT`](../../Reference/class/MclassDraw.md)).

Prediction results for object detection can prove complex to decipher, given that you can have multiple results for each training image. For example, an object detection prediction can identify two classes and four instances in the following target image.

*[Image: MclassDetDrawingClassNone.png]*

By calling [`MclassDraw`](../../Reference/class/MclassDraw.md), you can draw a box around the classes found, the center of the box, the corresponding color of every class, the name of the class, as well as the prediction score. Such drawings help you better understand the results of the prediction.

*[Image: MclassObjectDetectionDraw.png]*

To perform these drawing operations, specify [`M_DRAW_BOX`](../../Reference/class/MclassDraw.md), [`M_DRAW_BOX_NAME`](../../Reference/class/MclassDraw.md), [`M_DRAW_BOX_SCORE`](../../Reference/class/MclassDraw.md) (for the image above). You can also specify [`M_DRAW_BOX_CENTER`](../../Reference/class/MclassDraw.md) to draw the center positions of the bounding boxes. You can also set the threshold score for drawing using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md).
