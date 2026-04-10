---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Anomaly_detection
section: Prediction_for_anomaly_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Anomaly_detection / Prediction for anomaly detection"
---

# Prediction for anomaly detection

[`MclassPredict`](../../Reference/class/MclassPredict.md) uses a trained classifier context to make class predictions on a target. For a trained anomaly detection classifier, your target can be either an image or a dataset of images.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information about prediction, see [Prediction settings, results, and drawings](../C50_ML_Prediction/S03_Prediction_settings_results_and_drawings.md). |

## Prepare for prediction

To allocate a classification result buffer to hold the prediction results, call [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md). Anomaly detection with ADNet classifiers require target images that are the same size as the images used during training. The size of the training images can be inquired using [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_SIZE_X`](../../Reference/class/MclassInquire.md) and [`M_SIZE_Y`](../../Reference/class/MclassInquire.md).

Before predicting, preprocess your trained classifier context using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).

## Predict

Perform the prediction operation with the trained classifier context and the target data that you want to classify using [`MclassPredict`](../../Reference/class/MclassPredict.md). If your training images were prepared by cropping and resizing, then the target image must also be prepared in the same way. This maintains consistency between the training data and the target data. As mentioned, the size of the target images at prediction must be the same as the images used to train your ADNet classifier.

When performing the prediction operation with a dataset as your target, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

## Results

With anomaly detection, you can retrieve image type results (similar to image classification) or pixel type results (similar to segmentation). For example, you can call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) to retrieve whether an image is anomalous ([`M_IMAGE_PREDICTED_ANOMALOUS`](../../Reference/class/MclassGetResult.md)) or whether each pixel in the image is anomalous ([`M_MASK_IMAGE`](../../Reference/class/MclassGetResult.md)).

Regardless of how many classes you have in your datasets, images or pixels are predicted as either anomalous or not, and the resulting anomalous scores are calculated. Getting the score of the anomalous results (that is, [`M_IMAGE_SCORE`](../../Reference/class/MclassGetResult.md) and [`M_PIXEL_SCORES`](../../Reference/class/MclassGetResult.md)) can help shed some light on the accuracy of those results. Note that if [`M_IMAGE_SCORE`](../../Reference/class/MclassGetResult.md) >= [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md), the image will be anomalous. Similarly, pixels will be anomalous if [`M_PIXEL_SCORES`](../../Reference/class/MclassGetResult.md) >= [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md).

You can also get results from a specific entry in a dataset using [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md).

## Drawing results

To draw results, you can call [`MclassDraw`](../../Reference/class/MclassDraw.md). For example, you can draw the anomalous pixels ([`M_DRAW_ANOMALY_MASK`](../../Reference/class/MclassDraw.md)), the anomaly scores ([`M_DRAW_ANOMALY_SCORES`](../../Reference/class/MclassDraw.md)), the anomaly scores projected as a heat map ([`M_DRAW_ANOMALY_HEATMAP`](../../Reference/class/MclassDraw.md)), and the contour of the anomalies ([`M_DRAW_ANOMALY_CONTOUR_MASK`](../../Reference/class/MclassDraw.md)).

You can also draw prediction results from a specific entry in a dataset using [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md).

In general, drawing operations for prediction results can prove particularly useful when performing an anomaly detection prediction with a child buffer or a buffer with a region. Drawing can also give you a real-life sense for the classifier's performance after analyzing different stats.

## Assisted labeling

You can perform assisted labeling for anomaly detection by adding prediction results to your dataset. For more information about assisted labeling, see [Assisted labeling](../C50_ML_Prediction/S04_Advanced_techniques.md).
