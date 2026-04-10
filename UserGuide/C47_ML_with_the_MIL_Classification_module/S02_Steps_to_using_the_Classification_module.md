---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_with_the_MIL_Classification_module
section: Steps_to_using_the_Classification_module
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Classification_and_classifier_overview / Steps to using the Classification module"
---

# Steps to perform machine learning

The following steps provide a basic methodology to perform machine learning with the Aurora Imaging Library Classification module:

1. [Choose a task and classifier](S02_Steps_to_using_the_Classification_module.md).
2. [Collect data and build a dataset](S02_Steps_to_using_the_Classification_module.md).
3. [Train](S02_Steps_to_using_the_Classification_module.md).
4. [Predict](S02_Steps_to_using_the_Classification_module.md).

> **Note:** To take advantage of all available resources, such as GPU training and prediction (using OpenVINO or CUDA), you should install the required Aurora Imaging Library add-on. For more information, see [Aurora Imaging Library add-ons and updates](S06_Requirements_recommendations_and_troubleshooting.md).

## Choose a task and classifier

The machine learning task you choose is a vital initial decision that guides your application's development. Once you know the task (image classification, segmentation, object detection, anomaly detection, feature classification), you can select the classifier and start building your data and training. In general:

- Image classification is for classifying entire images.
- Segmentation is for classifying pixels in an image (typically, in image regions).
- Object detection is for classifying instances of objects (regions) in images.
- Anomaly detection is for classifying images or pixels that deviate from the norm.
- Feature classification is for classifying numerical data (typically features extracted from image).

For more information about available tasks, and why to choose one over the other, see [Which task is right for you](S05_Which_task_is_right_for_you.md).

> **Note:** Additional alternatives to performing machine learning with the Classification module include importing a trained ONNX format classifier (a machine learning model) and using it for prediction ([`MclassPredict`](../../Reference/class/MclassPredict.md)). For more information, see [ONNX](../C50_ML_Prediction/S05_ONNX.md). You can also have Zebra construct and train a classifier for you, using the labeled data that you provide (such classifiers are considered pretrained). For more information, [contact customer support](../C00_AIL_Introduction/S06_Service_information.md).

## Collect data and build a dataset

The data that you collect and use to train your classifier must be correctly identified (for example, labeled) and representative of the task you want to perform. This is critical to building a good dataset, and developing a properly trained classifier. Although the kind of data you need is different from task to task, the requirement of having proper data is essential for all. To use your data with Aurora Imaging Library, you must add it to a dataset context.

> **Note:** You must specify only non-anomalous (good) training images for anomaly detection. Labeling is not needed; all training images are considered non-anomalous. By training exclusively on non-anomalous images, the trained classifier can then identify the anomalous ones during prediction.

Some important recommendations regarding image data (for image classification, segmentation, object detection, and anomaly detection):

- Use Aurora Imaging CoPilot to build and manage your images datasets. For example, Aurora Imaging CoPilot lets you interactively create, label, modify, import, and export datasets. When using Aurora Imaging CoPilot, ensure that you have installed all related Aurora Imaging Library updates. For more information, see [Requirements, recommendations, and troubleshooting](S06_Requirements_recommendations_and_troubleshooting.md).
- Decouple dataset creation from usage. Since constructing a dataset involves disk storage, it should be done first, properly, and on its own. Once the dataset is ready, you can use it (for example, restore it) for training or prediction. For more information, see [Guidelines for managing an images dataset](../C48_ML_Datasets/S07_Guidelines_for_managing_an_images_dataset.md).
- Increase and improve your images dataset, using[`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). For example, you can resize and crop your image data, and you can perform numerous augmentations on it. For more information, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

## Train

Once you have built your dataset, you can train your classifier with it, using [`MclassTrain`](../../Reference/class/MclassTrain.md). This requires allocating a training context and a training result buffer.

Training for image classification, segmentation, object detection, and anomaly detection requires two datasets: a training dataset and a development dataset. Although you can explicitly create these from your single dataset (for example, by using [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md)) and specify them at training time, the training itself can automatically split your single dataset into the ones required when you call [`MclassTrain`](../../Reference/class/MclassTrain.md). At this time, training can automatically augment your data with an internally defined augmentation context. [`MclassTrain`](../../Reference/class/MclassTrain.md) can also automatically allocate a classifier context based on the specified training context. These automatic mechanisms are not necessarily supported for all cases; see [`MclassTrain`](../../Reference/class/MclassTrain.md) for details.

Training is typically an involved process that includes analyzing training results, calculating statistics, modifying settings, modifying data, and re-training the trained classifier context until it is ready for prediction.

## Predict

Once your classifier is trained, you can predict with it, using [`MclassPredict`](../../Reference/class/MclassPredict.md). To do so, you must allocate the required result buffers.

You can also use prediction to try and label data (assisted labeling), or as part of the training phase; that is, prediction results can cause you to implement more advanced training settings, re-train, and then predict again.

## Task and object summary

The Aurora Imaging Library Classification module requires allocating several objects (for example, contexts and buffers) whose type relates to the task and classifier chosen. The details of what you must allocate are discussed in the [task specific chapters](toc.md#Machine_learning_tasks); the following table represents a summary.

| *[Image: MclassObject.png]* | Task |
| --- | --- |
| Classifier context | [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md) | [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md) | [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md) | [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md) | [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md) |
| Specific predefined classifier context | [`M_ICNET_...`](../../Reference/class/MclassAlloc.md) | [`M_CSNET_...`](../../Reference/class/MclassAlloc.md) | None. | [`M_ODNET`](../../Reference/class/MclassAlloc.md) | [`M_ADNET`](../../Reference/class/MclassAlloc.md) |
| Dataset context | [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) | [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) | [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md) | [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) | [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) |
| Data preparation context | [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md) | [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md) | None. | [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) | None¹ |
| Training context | [`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md) | [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md) | [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md) | [`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md) | [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md) |
| Training result buffer | [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md) |
| Prediction result buffer | [`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md) | [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md) |
| ¹Although there is no dedicated data preparation context for anomaly detection, it can be possible to use the data preparation context of another task. For more information, see [Steps to perform anomaly detection](../C54_ML_Anomaly_detection/S02_Steps_to_perform_anomaly_detection.md). |

## Work-flow summary

Regardless of the task and classifier you choose, the basic work-flow for using the Aurora Imaging Library Classification module does not generally change. This work-flow involves building your data, training the classifier with it, and then predicting with the trained classifier.

*[Image: MclassWorkflowOverview.png]*

The time, resources, and requirements needed for each phase, particularly building and training, can vary considerably and is highly contingent on the task you are performing and the specifics of your application.
