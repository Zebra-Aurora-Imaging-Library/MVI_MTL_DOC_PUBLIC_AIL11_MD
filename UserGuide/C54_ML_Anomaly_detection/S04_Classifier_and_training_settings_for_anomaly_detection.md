---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Anomaly_detection
section: Classifier_and_training_settings_for_anomaly_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Anomaly_detection / Classifier and training settings for anomaly detection"
---

# Classifier and training settings for anomaly detection

Before you can start training, you need to allocate a classifier context and training objects, and set training related settings.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information on training settings, see [Fundamental decisions and settings](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md). |

## Training objects and folders

Allocate a training context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md). A training context holds the settings with which to train a classifier context, such as the training mode. You will also need to allocate a training result buffer to hold training results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md).

## Training modes and related settings

For anomaly detection, you must perform a complete training; that is, each training session is a fresh and total training of the classifier.

Aurora Imaging Library automatically sets the settings related to the complete training. You can also modify these by calling [`MclassControl`](../../Reference/class/MclassControl.md). For example, you can adjust the:

- Mode in which the samples are selected ([`M_TRAIN_MODE`](../../Reference/class/MclassControl.md)). By default, this is automatically set to a global selection ([`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md)).
- Size of each mini-batch ([`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md)). The default size is 1. Aurora Imaging Library uses mini-batches to split the training dataset to help make the training process more efficient.
- Number of mini-batches loaded into memory before sampling ([`M_MINI_BATCHES_PER_SAMPLING`](../../Reference/class/MclassControl.md)) when [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md) is set to [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md). The default number is 1.
- Number of samples learned by the classifier ([`M_SAMPLES_FIXED_SIZE`](../../Reference/class/MclassControl.md)). The higher the number, the bigger the size of the network (bigger networks can have a better/higher capacity to handle more complex cases/images). The default is 4000.

By default, Aurora Imaging Library tries to perform a global type of training, with [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md) being set to [`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md), but this can lead to running out of GPU memory. In this case, it is recommended to set [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md) to [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md), and then increase [`M_MINI_BATCHES_PER_SAMPLING`](../../Reference/class/MclassControl.md) until you maximize GPU usage.

[`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md) loads the whole training set into memory and then samples it, which is memory intensive but faster than [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md) since the sampling occurs only once. [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md) partitions the training set and samples each partition iteratively. This takes longer than [`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md) since the sampling process is repeated at each iteration. Note that both modes result in similar accuracy.

Such training mode controls are also known as hyperparameters. If your classifier is not performing as expected, you can adjust them and retrain (specifically, [`M_SAMPLES_FIXED_SIZE`](../../Reference/class/MclassControl.md) can affect the classifier's performance and also its inference time).

Once you have established your training mode controls (and all other settings for your training context), you must preprocess the context by calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md) with the identifier of the training context before training.
