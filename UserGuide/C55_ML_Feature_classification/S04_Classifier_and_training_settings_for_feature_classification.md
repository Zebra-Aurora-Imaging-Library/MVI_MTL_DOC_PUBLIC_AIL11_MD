---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Feature_classification
section: Classifier_and_training_settings_for_feature_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Feature_classification / Classifier and training settings for feature classification"
---

# Classifier and training settings for feature classification

Before you can start training, you need to allocate a classifier context and training objects, and set training-related settings.

Allocate a training context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). A training context holds the settings with which to train a classifier context, such as training modes. You will also need to allocate a classification result buffer to hold training results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).

## Setting up a classifier

Allocate a tree ensemble classifier context by calling [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). When you allocate a tree ensemble classifier context, it is essentially empty and ready for training.

By default, the tree ensemble uses 10 trees and has no maximum depth (there is no limit to the number of levels a tree can have). To modify these training settings, and others that affect the internal architecture of the tree ensemble, call [`MclassControl`](../../Reference/class/MclassControl.md) and specify the tree ensemble training context.

By default, Aurora Imaging Library uses a bootstrap aggregating (bagging) process to train the tree ensemble. You can call [`MclassControl`](../../Reference/class/MclassControl.md) and specify the tree ensemble training context to adjust that process. For example, you can decide whether randomly selected entries are available for reselection (whether to bootstrap with or without replacement) or whether to use out-of-bag dataset entries to estimate the generalization accuracy.

Note, bagging information is typically unreliable if your training dataset has augmented entries.

Also note that, you can either train a tree ensemble classifier from the ground up (using an empty tree ensemble classifier context), or you can continue training a previously trained tree ensemble classifier (this is referred to as a warm start). To continue training a previously trained tree ensemble classifier, you must copy the classification result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produced into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md), and then pass that trained tree ensemble context back to [`MclassTrain`](../../Reference/class/MclassTrain.md).

## Feature importance

For feature classification, every entry in a features dataset refers to a set of features. For example, every entry can refer to a set of blob features, such as area, perimeter, and Feret diameter. Specific values for these features are specified, for each entry, using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_RAW_DATA`](../../Reference/class/MclassControlEntry.md).

By specifying a feature importance mode, Aurora Imaging Library establishes, during training, whether certain features are more important than others in determining the class to which the input data belongs. For example, the perimeter feature can end up being very important to establishing a successful classification, while the area feature can end up being almost irrelevant.

To specify (or disable) the feature importance mode, call [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_FEATURE_IMPORTANCE_MODE`](../../Reference/class/MclassControl.md).

> **Note:** [`M_FEATURE_IMPORTANCE_MODE`](../../Reference/class/MclassControl.md) does not directly affect training. It allows you to retrieve which features are more important; you can then use this information to modify your dataset (for example, you can specify only the important features) and retrain.

### Modes

By default, Aurora Imaging Library uses a decreasing impurity process ([`M_MEAN_DECREASE_IMPURITY`](../../Reference/class/MclassControl.md)) to establish the feature importance. In this case, the more a feature affects a proper node splitting, the more important it is. Proper splitting means that the two output sets resulting from splitting the node are (on average) significantly purer (closer to agreeing on the final class) than the node's input set.

Alternatively you can use a drop column or permutation process to establish the feature importance. With a drop column process, the more the elimination of a feature affects accuracy, the more importance that feature is given. With a permutation process, the more the shuffling of a feature affects accuracy, the more importance that feature is given.

To specify a drop column or permutation importance, you must train with a development dataset or compute out-of-bag results (that is, enable [`M_COMPUTE_OUT_OF_BAG_RESULTS`](../../Reference/class/MclassControl.md)). To specify the set with which to calculate a drop column or permutation importance, use [`M_FEATURE_IMPORTANCE_SET`](../../Reference/class/MclassControl.md).

In general, the fastest modes with which to establish the feature importance are [`M_MEAN_DECREASE_IMPURITY`](../../Reference/class/MclassControl.md), [`M_PERMUTATION`](../../Reference/class/MclassControl.md), and [`M_DROP_COLUMN`](../../Reference/class/MclassControl.md), while the modes with which to most accurately establish the feature importance are [`M_DROP_COLUMN`](../../Reference/class/MclassControl.md), [`M_PERMUTATION`](../../Reference/class/MclassControl.md), and [`M_MEAN_DECREASE_IMPURITY`](../../Reference/class/MclassControl.md).

Note, if you disable the feature importance mode, you cannot retrieve any information about the feature importance.

## Proximity matrix

The proximity measure matrix tells you a measure of the similarity between pairs of entries in your training dataset, based on the terminal nodes of the decision trees. For example, if entries _i_ and _j_are in the same terminal node, their proximity increases. To calculate the proximity measure matrix when building trees, you must enable [`M_COMPUTE_PROXIMITY_MATRIX`](../../Reference/class/MclassControl.md) with [`MclassControl`](../../Reference/class/MclassControl.md).

## Class weights

You can control how the class weights are set in your training context by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_WEIGHT_MODE`](../../Reference/class/MclassControl.md). The default setting for feature classification is [`M_BALANCE`](../../Reference/class/MclassControl.md), which adjusts the weights for each class to be inversely proportional to the frequency of the class in the training dataset. If you need more control over the weights for each class, you can manually set the weight factor for each class using [`M_USER_DEFINED`](../../Reference/class/MclassControl.md) and [`M_CLASS_WEIGHT`](../../Reference/class/MclassControl.md). For more information about class weights, see [Class weights](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md).

Once you have established your training settings for your training context, you must preprocess the context by calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md) with the identifier of the training context.
