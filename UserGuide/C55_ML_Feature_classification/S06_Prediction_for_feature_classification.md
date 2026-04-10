---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Feature_classification
section: Prediction_for_feature_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Feature_classification / Prediction for feature classification"
---

# Prediction for feature classification

[`MclassPredict`](../../Reference/class/MclassPredict.md) uses a trained classifier context to make class predictions on a target. For a trained tree ensemble classifier, your target is either a set of features, or a features dataset. For more information about prediction, see [Prediction settings, results, and drawings](../C50_ML_Prediction/S03_Prediction_settings_results_and_drawings.md).

## Prepare for prediction

Allocate a classification result buffer to hold the prediction results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).

Before predicting, preprocess your trained classifier context using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).

## Predict

Perform the prediction operation with the trained classifier context and the target data that you want to classify using [`MclassPredict`](../../Reference/class/MclassPredict.md).

Note, if you have a testing dataset, you should predict with it, using [`MclassPredict`](../../Reference/class/MclassPredict.md). As previously discussed, predicting with a testing dataset serves as a quarantined final check for your trained classifier. If the results are what you expect (they should be approximately the same as your training results), you can continue with prediction using your trained classifier. If the results are not what you expect, it is a sign that you should continue training.

When performing the prediction operation with a dataset as your target, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

To prevent the classification process from taking too long, you can set a maximum calculation time for [`MclassPredict`](../../Reference/class/MclassPredict.md), by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TIMEOUT`](../../Reference/class/MclassControl.md). You can also stop the current execution of [`MclassPredict`](../../Reference/class/MclassPredict.md) (from another thread of higher priority), by specifying[`M_STOP_PREDICT`](../../Reference/class/MclassControl.md).

## Results

Retrieve the required results from the classification result buffer, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md). You can also get results from a specific entry in a dataset using [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md).

Typically, the most important prediction results to retrieve are the best predicted class ([`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResult.md)) and its score ([`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResult.md)).

### Drawing

For feature classification, you can only specify drawing operations for a dataset context (for example, [`M_DRAW_CLASS_ICON`](../../Reference/class/MclassDraw.md)). You cannot draw prediction results.

## Assisted labeling

You can perform assisted labeling for feature classification by adding prediction results to your dataset. For more information about assisted labeling, see [Assisted labeling](../C50_ML_Prediction/S04_Advanced_techniques.md).
