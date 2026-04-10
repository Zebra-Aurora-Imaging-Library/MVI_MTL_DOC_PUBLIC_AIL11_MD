---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Prediction
section: Prediction_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Predicting_and_advanced_techniques / Prediction overview"
---

# Prediction overview

[`MclassPredict`](../../Reference/class/MclassPredict.md) uses a trained classifier context to make class predictions (interferences) on a target. For a trained CNN, segmentation, object detection, or anomaly detection classifier context, your target is either an image or a dataset of images. For a trained tree ensemble classifier context, your target is either a feature (list of values) or a dataset of features.

Specifying a dataset as the target can help training and also can help label your data. For more information, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md) and [Assisted labeling](S04_Advanced_techniques.md).

The Classification module also lets you import a trained ONNX machine learning model into an ONNX classifier context and use it for prediction. For more information, see [ONNX](S05_ONNX.md).

## Predict engine

The predict engine refers to the hardware (CPU/GPU) with which the prediction is performed. [`MclassPredict`](../../Reference/class/MclassPredict.md) uses the default predict engine established by the **Aurora Imaging Configurator** utility. You can modify this by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_PREDICT_ENGINE`](../../Reference/class/MclassControl.md) or in **Aurora Imaging Configurator**. Aurora Imaging Library provides an example, _PredictEngineSelection.cpp_, to help you pick the fastest prediction engine available to you.

*[Image: MclassPredictEngineSelectionExample.png]*

To run/view this and other examples, use Aurora Imaging Example Launcher.

> **Note:** To take advantage of all available resources, such as GPU prediction using OpenVINO and CUDA, you should install the required Aurora Imaging Library add-on. For more information, see [Aurora Imaging Library add-ons and updates](../C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md).
