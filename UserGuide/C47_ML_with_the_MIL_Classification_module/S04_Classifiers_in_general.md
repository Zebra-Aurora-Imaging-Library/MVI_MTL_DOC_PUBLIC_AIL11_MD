---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_with_the_MIL_Classification_module
section: Classifiers_in_general
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Classification_and_classifier_overview / Classifiers in general"
---

# Classifiers, what they are and what they do

Classifiers are the underlying mathematical architectures of machine learning. To work, they require training, typically with labeled data. Once trained, you can use the classifier to predict the class to which new data belongs. The classifier you choose depends on the task you want to perform.

> **Note:** Anomaly detection does not need labeled data. However, you must only provide non-anomalous (good) training images. This type of vetted data (only good images) lets the training process infer that every training image is _Good_, which can be considered a type of implied label. Even if your images are labeled (for example, if they were used for another machine learning task), the anomaly detection training process considers them non-anomalous images.

There are several predefined classifiers you can choose from, for image classification ([`M_ICNET_...`](../../Reference/class/MclassAlloc.md)), segmentation ([`M_CSNET_...`](../../Reference/class/MclassAlloc.md)), object detection ([`M_ODNET`](../../Reference/class/MclassAlloc.md)), and anomaly detection ([`M_ADNET`](../../Reference/class/MclassAlloc.md)) tasks. Although the underlying structure of these ICNets, CSNets, ODNets, and ADNets is similar, the specifics of the problems they are designed to handle, and types of the images recommended for them, can differ.

Note, there are no predefined tree ensemble classifiers for feature classification.

> **Note:** This section discusses classifiers in general. Training a classifier with a dataset and predicting with it is discussed in the later chapters ([Datasets](../C48_ML_Datasets/ChapterInformation.md),[Training](../C49_ML_Training/ChapterInformation.md), and [Prediction](../C50_ML_Prediction/ChapterInformation.md)). For information about importing a trained ONNX format classifier (a machine learning model) and using it for prediction with Aurora Imaging Library, see [ONNX](../C50_ML_Prediction/S05_ONNX.md).

## CNN, segmentation, object detection, and anomaly detection classifiers

A CNN is a complex function with millions of weights that, when properly set (successfully trained), can classify an image. Structurally, a CNN is basically implemented as a cascade of layers, including a source layer, one or more hidden layers, and an output layer.

*[Image: MclassTheoreticalCNNLayers.png]*

The source layer defines the input size of the network, while the output layer establishes the classes. The hidden layers learn how to process the image for the defined classes. Whether you are performing image classification, segmentation, object detection, or anomaly detection, the underlying technology is a CNN that is specifically designed for the required machine learning task.

Since Aurora Imaging Library predefines this classifier architecture, you cannot explicitly control it. Your control lies in training the classifier. During training, Aurora Imaging Library establishes the values of the classifier's weights based on your labeled images (training dataset context) and training settings (training context). In this way, the classifier determines the boundaries between classes within a complex internal feature space.

The label of the image identifies the class to which it belongs; this class label is known as the ground truth and is usually established by a human. For example, a human will label numerous images as either _GoodApple_ or _BadApple_. The training analyzes these labeled images and iteratively adjusts the classifier's weights so it can identify and differentiate the different images with, ideally, a very low error rate (high accuracy). The goal is to establish a stable set of weights such that the trained classifier can classify similar images as well as a human.

Several settings, which you can specify for the training context, can affect how Aurora Imaging Library establishes the classifier's weights. These settings are sometimes referred to as training mode settings. Adjusting these settings (such as the learning rate and the number of training cycles or epochs) can help improve how the classifier gets trained on the labeled data.

## ICNet, CSNet, ODNet, and ADNet

The predefined image classification (ICNet) and segmentation (CSNet) classifiers that you can train can be small, medium, or extra large, and you can choose a specific one. More complicated problems and larger images can call for larger classifiers, which can result in longer training and prediction times. The specific classifier you choose can affect training settings, in particular, the [training mode](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md) (complete, transfer learning, or fine tuning).

> **Note:** The predefined object detection (ODNet) and anomaly detection (ADNet) classifiers do not have different sizes; that is, you must use [`M_ODNET`](../../Reference/class/MclassAlloc.md) and [`M_ADNET`](../../Reference/class/MclassAlloc.md).

Predefined ICNet, CSNet, ODNet, and ADNet classifiers do not impose any restriction on the minimum size of training images; however, all training images must be the same size. When predicting with ICNets (image classification), ODNet (object detection), and ADNet (anomaly detection), the size of the target images must be the same as the images used for training. When predicting with CSNets (segmentation), the target image size can be smaller or bigger than the images used to train these classifiers (this requires calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md)).

### Small ICNet and CSNet

The small ICNet ([`M_ICNET_S`](../../Reference/class/MclassAlloc.md)) and CSNet ([`M_CSNET_S`](../../Reference/class/MclassAlloc.md)) classifiers are compact and typically used for smaller images. The capacity of the network is low, so it might not be suitable for highly complex problems. You can use images with up to 1024 bands with these classifiers. They are for a complete training.

### Medium ICNet and CSNet

The medium ICNet ([`M_ICNET_M`](../../Reference/class/MclassAlloc.md)) and CSNet ([`M_CSNET_M`](../../Reference/class/MclassAlloc.md)) classifiers are designed to solve the majority of problems. The capacity of these networks is high enough to cover many problems with a fairly fast prediction speed. You can use images with up to 1024 bands with these classifiers. They are for a complete training.

### Extra large ICNet and CSNet

The extra large ICNets ([`M_ICNET_XL`](../../Reference/class/MclassAlloc.md),[`M_ICNET_MONO_XL`](../../Reference/class/MclassAlloc.md), and [`M_ICNET_COLOR_XL`](../../Reference/class/MclassAlloc.md)) and CSNets ([`M_CSNET_XL`](../../Reference/class/MclassAlloc.md),[`M_CSNET_MONO_XL`](../../Reference/class/MclassAlloc.md), and [`M_CSNET_COLOR_XL`](../../Reference/class/MclassAlloc.md)) classifiers have a high capacity, but prediction is slower than the other classifiers.

With the general version of these classifiers ([`M_ICNET_XL`](../../Reference/class/MclassAlloc.md) and [`M_CSNET_XL`](../../Reference/class/MclassAlloc.md)), you can use images with up to 1024 bands. These classifiers are for a complete training.

With a specific version of these classifiers ([`M_ICNET_MONO_XL`](../../Reference/class/MclassAlloc.md), [`M_ICNET_COLOR_XL`](../../Reference/class/MclassAlloc.md), [`M_CSNET_MONO_XL`](../../Reference/class/MclassAlloc.md), and [`M_CSNET_COLOR_XL`](../../Reference/class/MclassAlloc.md)), you can use either grayscale (1-band) or color (3-band) images, depending on the one you choose. These classifiers are intended for transfer learning (to build on training done by Zebra).

### Sizing up your scenario, for ICNet and CSNet

Many factors can theoretically affect which ICNet or CSNet works best; for example, the complexity of the problem (that is, how challenging is it to perform the image classification or segmentation task), the amount of labeled data that you have (training images), the accuracy that you need, and the required speed of your training and prediction.

> **Note:** As a rule of thumb, the harder it is for a human to perform the task (such as identifying an object in an image), the more complex it is for a classifier. Note, some image characteristics that seem innocuous to humans, such as minor variances in rotation and lighting, can prove complex for a classifier.

In practice, medium predefined classifiers work best, regardless of your scenario, or the problem's complexity. You should use a medium classifier first, and only try another if results are not what you expect.

The following summarizes the typical ICNet or CSNet classifier to choose, based on the given scenario.

|  |
||
| I want to explore and experiment. | - | ✔ | - | - | - |
| I want to use what is recommended to work best in the vast majority of cases. | - | ✔ | - | - | - |
| I have a problem that does not seem very complex, a lot of good - although small - training images, and the medium (recommended) classifier does not work as well as expected. | ✔ | - | - | - | - |
| I have a complex problem, a lot of good training images, and the medium (recommended) classifier does not work as well as expected. | - | - | ✔ | - | - |
| I do not have enough training images (my problem is either typical or complex), and the medium (recommended) classifier does not work as well as expected. | - | - | - | ✔ | ✔ |
| I cannot pin point the reason, but I have tried the other classifiers and they do not work as expected. | - | - | - | ✔ | ✔ |
| ¹Zebra has trained the XL color/monochrome classifiers on a large generic dataset. Selecting them requires you to perform transfer learning (that is, you must set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md)). Note, you can perform transfer learning on any trained classifier. |

Some additional points to consider, when the medium (recommended) classifier is not working as expected:

- Do not immediately use another classifier. Instead, try augmenting your data, adjusting the labeling, and tweaking your training settings. You can also add more data to your dataset.
- If performing segmentation, and the foreground (area to detect) is very small and/or thin, try the small CSNET classifier as it tends to give finer segmentation for less complex cases.
- If some images are too small, you can resize them. You can also down-sample (shrink) your images to help reduce memory requirements and decrease prediction time. Do not down-sample if it noticeably affects important features, such as causing a defect to disappear or become faint.
- Although there is no minimum image size requirement, if you have opted for a small classifier and are still not getting the results you need, try using images that are at least 43 pixels.

## Tree ensemble classifier

A tree ensemble classifier is a set of decision trees. A decision tree is a collection of nodes connected by branches. Each node takes in data and either makes a decision on how to best split (branch) the data into 2 other nodes, or makes a final decision on that data (known as a leaf node). Starting with a single node, the data gets processed down the tree until it is no longer splittable and all branches end with a leaf. Those leaves can be considered the classes.

*[Image: MclassTreeEnsembleDecisionTree.png]*

To make the classification making process more robust, tree ensembles use many decision trees, rather than using just one. Each tree takes in and classifies the same set of features; the final classification decision is based on the decision of the majority of the trees. To process the features, node by node, tree ensembles use bagging, randomness and multiple learning algorithms (bootstrap aggregating) during the training process. For more information on tree ensemble classifiers, see [Setting up a classifier](../C55_ML_Feature_classification/S04_Classifier_and_training_settings_for_feature_classification.md).
