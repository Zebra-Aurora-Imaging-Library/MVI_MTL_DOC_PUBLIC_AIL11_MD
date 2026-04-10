---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Training
section: Fundamental_decisions_and_settings
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Training / Fundamental decisions and settings"
---

# Fundamental decisions and settings

Before training a classifier with [`MclassTrain`](../../Reference/class/MclassTrain.md), there are some fundamental training decisions and settings you will typically have to make. These are usually made by calling [`MclassControl`](../../Reference/class/MclassControl.md). The content on this page primarily applies to epoch based trainings (that is, CNN, segmentation, and object detection), though some information also applies to anomaly detection training, which does not use epochs. For training settings related to tree ensemble classifiers, see [Classifier and training settings for feature classification](../C55_ML_Feature_classification/S04_Classifier_and_training_settings_for_feature_classification.md).

During the training process, you can observe the evolution of the classifier to see if it is progressing as expected. Usually, after training, you would analyze your results, and train again to make improvements, until you consider your classifier properly trained. For more information, see [Analysis, adjustment, and additional settings](S04_Analysis_adjustment_and_additional_settings.md), and also the training section of your specific machine learning chapter in the [Machine learning tasks](toc.md#Machine_learning_tasks) part.

## Training modes

Depending on the problem definition, and the classifier (machine learning task) you choose, different training mode controls and values can be available (other than what is set by default).

Training mode controls differ in their input and output properties as well as in their initialization. To set these training modes, if they are available, call [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md). In effect, when you set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to a specific training mode setting ([`M_COMPLETE`](../../Reference/class/MclassControl.md), [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md), or [`M_FINE_TUNING`](../../Reference/class/MclassControl.md)), all related training mode values are set accordingly, though you can change them if needed. For more information, see [Training mode controls](S03_Fundamental_decisions_and_settings.md).

By default, an [`M_COMPLETE`](../../Reference/class/MclassControl.md) training is performed, and the related training values, given the classifier you are using, are set to their corresponding defaults. These defaults should be sufficient for typical cases. You can call [`MclassControl`](../../Reference/class/MclassControl.md) with the training context to modify the individual settings that apply to your classifier (for example, [`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md)), or if available, you can reset them all based on the type of training you want to do ([`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md)).

> **Note:** For anomaly detection, a complete training is always performed (though [`M_COMPLETE`](../../Reference/class/MclassControl.md) cannot be explicitly set). Unlike epoch based trainings, the related training values that an anomaly detection training affects are [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md), [`M_MINI_BATCHES_PER_SAMPLING`](../../Reference/class/MclassControl.md), and [`M_SAMPLES_FIXED_SIZE`](../../Reference/class/MclassControl.md). Refer to these values for details.

ICNet, CSNet, and ODNet predefined classifiers usually support a complete training mode, though not necessarily other modes (transfer learning and fine tuning), when you first use them. The mode generally refers to the process by which a predefined classifier is trained, and the extent to which it should use previously learned information. As previously mentioned, you are always performing a complete training with an ADNet classifier and [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) does not apply.

### Complete

In this mode ([`M_COMPLETE`](../../Reference/class/MclassControl.md)), a complete model training is performed (this is the default training mode). The classifier's source layer adapts to the size (and number of channels) of the input training images; the output layer adopts the number of target classes, and all the network weights are randomly re-initialized. This is the preferred mode when having access to a significant amount of training data. Typically, this is for completely restarting the training of the classifier context, or for training a classifier context that is not trained.

Note, once a classifier that requires a complete training (for example, [`M_ICNET_M`](../../Reference/class/MclassAlloc.md)) was trained once, it can be possible to then continue the training process as transfer learning or fine tuning. This requires copying the trained classifier from the train result into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md) with [`M_TRAINED_CLASSIFIER`](../../Reference/class/MclassCopyResult.md), and retraining it with [`MclassTrain`](../../Reference/class/MclassTrain.md).

### Transfer learning

This mode ([`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md)) performs the transfer learning technique. Similar to complete, the source layer adapts to the size of the input training images and the output layer adapts to the number of target classes. Note, the classifier's source layer only adapts to the size of input training images (since, the number of bands will not change, the input training images should have the same bands as the CNN or segmentation classifier). Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must train on a similar (but new) problem.

The classifier's weights for the feature extraction layers however are those of the pretrained classifier. During a transfer learning type of training, only the classifier layers are trained to classify the images. You should use this mode when the quantity of training data is limited. For more information about the classifier's internal layers, see [Classifiers, what they are and what they do](../C47_ML_with_the_MIL_Classification_module/S04_Classifiers_in_general.md).

Note, you can use the partly pretrained classifiers [`M_ICNET_MONO_XL`](../../Reference/class/MclassAlloc.md) and [`M_ICNET_COLOR_XL`](../../Reference/class/MclassAlloc.md) to properly extract the generally important features suitable for most applications. You must then typically continue the training process, with a transfer learning mode and your own labeled images, to complete the training for your specific needs.

### Fine tuning

This mode ([`M_FINE_TUNING`](../../Reference/class/MclassControl.md)) is used to fine-tune a classifier (model). Fine tuning is used to improve a previously-trained model with the addition of new training data. Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must train with additional data.

In this mode, the input and output layers as well as all the network weights are those from the previously-trained model. As a consequence, the number of target classes, as well as the size and bands of the input images, cannot be changed in this mode.

Typical reasons to fine-tune are:

- You captured more data and updated the training dataset for all or some specific classes.
- You must now account for new image conditions like illumination or camera perspective.

### Summary and comparison

The following table summarizes and compares the training modes.

| [`M_COMPLETE`](../../Reference/class/MclassControl.md) | [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md) | [`M_FINE_TUNING`](../../Reference/class/MclassControl.md) |
| --- | --- | --- |
| Input images | Adapts size and bands of input images | Adapts size of input images | Uses existing sizes |
| Feature extraction layers | Resets internal parameters (weights) | Starts from existing internal parameters (weights) | Uses existing internal parameters (weights) |
| Classification layers | Resets internal parameters (weights) | Resets internal parameters (weights) | Uses existing internal parameters (weights) |
| Output layers | Adapts number of target classes | Adapts number of target classes | Uses existing classes |
| Usage | New application with lots of training data | New application with limited training data | Improve existing application |

## Training mode controls

When you specify a specific training mode, the related training mode controls are automatically set to the required values. You can also modify these controls yourself, to adjust the training process. The following are some examples of what the training mode controls let you adjust:

- Learning rate.
- Maximum number of epochs.
- Mini-batch size.
- Schedule type.

Such training mode controls are also known as hyperparameters. Details about all the training mode controls and related values that you can use can be found in the task specific chapters (in the [Machine learning tasks](toc.md#Machine_learning_tasks) part) and in the reference ([`MclassControl`](../../Reference/class/MclassControl.md)).

### Learning rate

The learning rate controls the factor by which the optimizer updates the network weights after each iteration. The higher the learning rate, the bigger the steps the optimizer takes to adjust the network parameters, and vice versa.

Training begins with an initial learning rate ([`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md)) which then decreases after each epoch according to the decay value ([`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md)). As an example, if the decay is set to 0.2, the learning rate loses 20% of its amplitude value after each epoch.

At the beginning, higher learning rate helps the network to converge faster to a desirable state. After several iterations, the network's weights are updated more and more carefully with lower learning rate.

### Maximum number of epochs

The maximum number of epochs ([`M_MAX_EPOCH`](../../Reference/class/MclassControl.md)) sets the maximum number of epochs to complete the training process. An epoch refers to one complete cycle through the dataset that the classifier must learn during training.

The more complex the application, the more epochs might be needed.

### Mini-batch size

Datasets cannot usually fully reside in memory during training. Mini-batches are used to break down the dataset that each epoch cycles through. Aurora Imaging Library loads the data and processes it one mini-batch after another for each epoch.

The mini-batch size ([`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md)) determines the number of images that are part of a mini-batch. The larger the mini-batch size the faster the training process and potentially the more accurate the resulting network but the more memory is required.

A larger mini-batch size tends to make the learning smoother and faster. The maximum batch size is limited by the available memory for calculations. In general, a bigger batch size improves the network accuracy, however, depending on data, it is not systematically the case. For image classification, batch sizes typically range from 32 to 512. Batch sizes are generally much smaller for segmentation and object detection because they use more memory.

Batch size plays a key role in the consumption of resources. It is recommended to observe memory (GPU or CPU) during training, with the help of the operating system's performance monitor.

### Schedule type

The schedule type ([`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md)) sets the schedule with which to adjust the learning rate. You can either specify to decay the learning rate at a cyclical schedule or to decay the learning rate as the internal parameters (weights) are updated.

When specifying a cyclical schedule ([`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md)), it is expected that you have a minimum number of mini-batches (for example, a dozen) per epoch.

### Comparing default training mode controls

The table below shows the default settings for the training mode controls and how the defaults differ depending on whether the base training mode ([`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md)) is complete, transfer learning, or fine tuning.

| [`M_COMPLETE`](../../Reference/class/MclassControl.md)¹ | [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md) | [`M_FINE_TUNING`](../../Reference/class/MclassControl.md) |
| --- | --- | --- |
| [`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md) | 0.005 (image classification) or 0.001 (segmentation or object detection) | 0.005 (image classification) or 0.001 (segmentation) | 0.001 (image classification or segmentation) |
| [`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md) | 0.1 (image classification) or 0.05 (segmentation or object detection) | 0.1 (image classification) or 0.05 (segmentation) | 0.1 (image classification) or 0.05 (segmentation) |
| [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) | 60 (image classification, segmentation, and object detection) | 60 (image classification or segmentation) | 60 (image classification or segmentation) |
| [`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md) | 32 (image classification), 4 (object detection and segmentation), or 1 (anomaly detection) | 32 (image classification) or 4 (segmentation) | 32 (image classification) or 4 (segmentation) |
| [`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md) | [`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md) (image classification or segmentation) or [`M_FACTOR_DECAY`](../../Reference/class/MclassControl.md) (object detection). | [`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md) (image classification or segmentation) | [`M_DECAY`](../../Reference/class/MclassControl.md) (image classification or segmentation) |
| ¹For object detection, you can only set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_COMPLETE`](../../Reference/class/MclassControl.md). For anomaly detection, a complete training is always performed, though [`M_COMPLETE`](../../Reference/class/MclassControl.md) cannot be explicitly set. Unlike epoch based trainings, anomaly detection training also affects [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md), [`M_MINI_BATCHES_PER_SAMPLING`](../../Reference/class/MclassControl.md), and [`M_SAMPLES_FIXED_SIZE`](../../Reference/class/MclassControl.md). Refer to these values for details. |

Be careful when modifying either the base training mode ([`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md)) or any of the related training mode settings, as Aurora Imaging Library uses your last specified value. For example, if you want a specific learning rate and you set that value, but then set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to a specific training mode, the learning rate, along with all other related training mode controls, will be reset to the default values listed for that training mode.

## Class weights

Class weights allow you to influence your classifier's training by placing more or less importance (weight) on certain class definitions. Class weights are able to be adjusted for segmentation and feature classification. While class weights are not supported for image classification or object detection, image classification and object detection datasets are easily balanced with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) and [`M_AUGMENT_BALANCING`](../../Reference/class/MclassControl.md). By properly balancing a dataset to meet your classifier's needs, you can achieve a similar effect to setting class weights. Note, class weights and data preparation are not supported for anomaly detection.

In the real world, the result of some class predictions are more important than others, particularly when it relates to costs in a business application. For example, it is very important to identify a faulty part before it gets installed in a cost-sensitive application. In this case, it would be preferable to incorrectly identify a good part as being bad, as opposed to incorrectly identifying a bad part as being good. In the first scenario, you either discard the part or set it aside for further inspection. In the second scenario, a bad part will be installed, causing a failure. The costs of these two scenarios might differ drastically, and you can use class weights to train a classifier with this in mind.

If your dataset only has a few entries of an important class definition (the bad part in the above example), your classifier might not be good at predicting that a target belonging to that class because there are few examples of that class during training. By setting a higher class weight to the class represented by those few entries, you can train your classifier to be more likely to predict that a target belongs to that class. In this case, you would set a higher class weight to the dataset entries representing a bad part, particularly if they are underrepresented in the dataset. Even though this might makes the classifier less accurate (by predicting that a part is bad more often than it really is), it will be more useful to this application.
