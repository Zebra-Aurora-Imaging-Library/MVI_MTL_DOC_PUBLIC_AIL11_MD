---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Object_detection
section: Classifier_and_training_settings_for_object_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Object_detection / Classifier and training settings for object detection"
---

# Classifier and training settings for object detection

Before you can start training, you need to allocate a classifier context and training objects, and set training related settings.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information on training settings, see [Fundamental decisions and settings](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md). |

## Training folders

Set the destination folder that will store prepared images using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassControl.md).

## Training related settings

A training context holds the settings with which to train a classifier context, such as training modes. Set your training related settings as required. Once you have established your training settings for your training context, you must preprocess the context by calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md) with the identifier of the training context.

### Training modes

For object detection, the only training mode available is [`M_COMPLETE`](../../Reference/class/MclassControl.md). This is for completely restarting the training of an object detection classifier context, or for training an object detection classifier context that is not trained. You do not have to manually set the training mode for object detection since,[`M_DEFAULT`](../../Reference/class/MclassControl.md) is the same as [`M_COMPLETE`](../../Reference/class/MclassControl.md).

Aurora Imaging Library automatically sets the related training mode controls to the required settings for object detection. You can also modify these controls yourself to adjust the training process by calling [`MclassControl`](../../Reference/class/MclassControl.md). The training mode controls let you adjust the:

- Learning rate ([`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md) and [`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md)). For object detection, the default learning rate is 0.001 and the default learning rate decay is 0.05.
- Maximum number of epochs ([`M_MAX_EPOCH`](../../Reference/class/MclassControl.md)). For object detection, the default maximum number of epochs is 60.
- Mini-batch size ([`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md)). For object detection, the default mini-batch size is 4.
- Schedule type ([`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md)). Note, for object detection, only [`M_FACTOR_DECAY`](../../Reference/class/MclassControl.md) is available.

Such training mode controls are also known as hyperparameters. If your classifier is not performing as expected, you can adjust the hyperparameters and retrain.
