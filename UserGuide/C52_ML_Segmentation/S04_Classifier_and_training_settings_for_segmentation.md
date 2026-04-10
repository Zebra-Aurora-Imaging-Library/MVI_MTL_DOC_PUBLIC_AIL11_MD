---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Segmentation
section: Classifier_and_training_settings_for_segmentation
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Coarse_segmentation / Classifier and training settings for segmentation"
---

# Classifier and training settings for segmentation

Before you can start training, you need to allocate a classifier context and training objects, and set training related settings.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information on training settings, see [Fundamental decisions and settings](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md). |

## Training objects and folders

Allocate a training context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md). A training context holds the settings with which to train a classifier context, such as training modes. You will also need to allocate a training result buffer to hold training results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md).

Set the destination folder that will store prepared images and segmentation scores using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassControl.md).

## Training related settings

Set your training related settings as required. Once you have established your training settings for your training context, you must preprocess the context by calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md) with the identifier of the training context.

### Training modes

Depending on the problem definition, different training modes exist for your classifier. To set these training modes for your training context, call [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md).

- Complete ([`M_COMPLETE`](../../Reference/class/MclassControl.md)).
  - Typically, this is for completely restarting the training of a CNN or segmentation classifier context, or for training a CNN or segmentation classifier context that is not trained.
- Transfer learning ([`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md)).
  - Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must train on a similar (but new) problem.
- Fine tuning ([`M_FINE_TUNING`](../../Reference/class/MclassControl.md)).
  - Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must train with additional data.

When you specify a training mode, Aurora Imaging Library automatically sets the related training mode controls to the required settings. You can also modify these controls yourself to adjust the training process by calling [`MclassControl`](../../Reference/class/MclassControl.md). The training mode controls let you adjust the:

- Learning rate ([`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md) and [`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md)). For segmentation, the default learning rate is 0.001 and the default learning rate decay is 0.05.
- Maximum number of epochs ([`M_MAX_EPOCH`](../../Reference/class/MclassControl.md)). For segmentation, the default maximum number of epochs is 60.
- Mini-batch size ([`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md)). For segmentation, the default mini-batch size is 4.
- Schedule type ([`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md)). For segmentation, the default schedule type is, [`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md).

Such training mode controls are also known as hyperparameters. If your classifier is not performing as expected, you can adjust the hyperparameters and retrain.

### Class weights

Class weights allow you to influence your classifier's training by placing more or less importance (weight) on certain class definitions. Class weights are a particularly useful setting in segmentation, because there are multiple classes represented in each image. In an image, 90% of the pixels might represent class 0 (the background class), and only 10% represent class 1 (the object of interest). Without setting class weights, the classifier could learn to always predict class 0 and be correct 90% of the time, but this would be a useless classifier.

You can control how the weights are set in your training context by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_WEIGHT_MODE`](../../Reference/class/MclassControl.md). The default setting is [`M_INVERSE_CLASS_FREQUENCY`](../../Reference/class/MclassControl.md), which will set weights for each class such that the classifier will place more weight on the less frequent classes (class 1). The strength of this effect is controlled by [`M_CLASS_WEIGHT_STRENGTH`](../../Reference/class/MclassControl.md). This might make your classifier less accurate than the "always predict class 0" method, but it will be much more useful for predicting class 1 (your objects of interest). If you need more control over the class weights, you can manually set the weight factor for each class using [`M_USER_DEFINED`](../../Reference/class/MclassControl.md) and [`M_CLASS_WEIGHT`](../../Reference/class/MclassControl.md). For more information, see [Fundamental decisions and settings](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md).
