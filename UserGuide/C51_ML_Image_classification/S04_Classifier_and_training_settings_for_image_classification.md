---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Image_classification
section: Classifier_and_training_settings_for_image_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Image_classification / Classifier and training settings for image classification"
---

# Classifier and training settings for image classification

Before you can start training, you need to allocate a classifier context and training objects, and set training related settings.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information on training settings, see [Fundamental decisions and settings](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md). |

## Training objects and folders

Allocate a training context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md). A training context holds the settings with which to train a classifier context, such as training modes. You will also need to allocate a training result buffer to hold training results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md).

Set the destination folder that will store prepared images using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassControl.md).

## Training modes

Depending on the problem definition, different training modes exist for your classifier. To set these training modes for your training context, call [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md).

- Complete ([`M_COMPLETE`](../../Reference/class/MclassControl.md)).
  - Typically, this is for completely restarting the training of a CNN, segmentation, or object detection classifier context, or for training a CNN, segmentation, or object detection classifier context that is not trained.
- Transfer learning ([`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md)).
  - Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must train on a similar (but new) problem.
- Fine tuning ([`M_FINE_TUNING`](../../Reference/class/MclassControl.md)).
  - Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must train with additional data.

When you specify a training mode, Aurora Imaging Library automatically sets the related training mode controls to the required settings. You can also modify these controls yourself to adjust the training process by calling [`MclassControl`](../../Reference/class/MclassControl.md). The training mode controls let you adjust the:

- Learning rate ([`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md) and [`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md)). For image classification, the default learning rate is 0.005 and the default learning rate decay is 0.1.
- Maximum number of epochs ([`M_MAX_EPOCH`](../../Reference/class/MclassControl.md)). For image classification, the default maximum number of epochs is 60.
- Mini-batch size ([`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md)). For image classification, the default mini-batch size is 32.
- Schedule type ([`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md)). For image classification, the default schedule type is [`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md).

Such training mode controls are also known as hyperparameters. If your classifier is not performing as expected, you can adjust the hyperparameters and retrain.

Once you have established your training mode controls (and all other settings for your training context), you must preprocess the context by calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md) with the identifier of the training context before training.
