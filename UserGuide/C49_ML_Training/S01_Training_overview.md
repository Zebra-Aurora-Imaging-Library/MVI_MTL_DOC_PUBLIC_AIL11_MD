---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Training
section: Training_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Training / Training overview"
---

# Training overview

Training is the process in which the classifier learns to predict the class to which the data belongs.

Training requires a classifier context (the classifier you are training), a training context (the settings to train the classifier), and dataset contexts (and data to train the classifier). You must specify these contexts when you call [`MclassTrain`](../../Reference/class/MclassTrain.md). Note, you have little direct control over the classifier context; your control lies in training it. That is, [`MclassTrain`](../../Reference/class/MclassTrain.md) uses the training context (which you can modify, for example, by calling [`MclassControl`](../../Reference/class/MclassControl.md)), and the data, to train the classifier context.

Training time varies considerably depending on the complexity of the application, the available hardware, and the accuracy required. To produce a properly trained classifier, [`MclassTrain`](../../Reference/class/MclassTrain.md) might have to run for an extended period, and you might have to call it several times after modifying your training settings. In some cases, you might need to modify your datasets also, and then go back to training. Once the training process returns the results you require, you can predict with it.

## Typical trains of thought

An untrained classifier context behaves like a random guess; for example, an untrained classifier would have an error rate (or accuracy) of approximately 50% when solving a 2 class (binary) problem.

In many cases, the goal of training a classifier is to lower its error rate (increase its accuracy) using representative data (datasets) and adjusting training settings (training context). Theoretically, and especially for many CNN based classifiers, you can see training as an optimization problem that minimizes error (in some cases, this is a cost/loss function). If everything is perfect, training results in a classifier that has no error and is 100% accurate. Although this is ideal, it is unrealistic when using deep learning or machine learning technologies.

Training is an iterative optimization process; at each iteration, the error is calculated for all the images in the training dataset. The classifier's internal parameters (weights) are updated to minimize that error.

Due to memory limitations, it is typically impossible to load all the data simultaneously, especially when using a CNN based classifier, since the data is images (often, thousands of images). To address this,the data is divided into mini sets (sometimes called mini-batches). The training process calculates the error (sometimes known as loss) and generally updates the weights for each mini set to minimize that error. Although training often processes each mini set independently, the goal is to minimize the global error.

> **Note:** This section describes general training concepts which can vary depending on the machine learning task. For example, for tree ensemble classifiers, a training process similar to what is described occurs with bootstrapping (bagging), while for anomaly detection, there is the concept of sampling (using a part of the image instead of the whole image). Also, unlike most other classifiers that train on images, anomaly detection does not have the notion of loss or epoch (or related concepts).

### Additional general details

Keep in mind that if you are training a classifier with a limited amount of data, you can achieve a low error rate (high accuracy) at training time, but get a high error rate (low accuracy) at prediction time. Limited data often causes training to over-fit the classifier on that data; when this happens, the classifier did not generalize the problem and does not perform well when given similar images it has not seen. Limited data can occur when, for example, you need to train with a specific defect, but you do not have many images of that defect because it happens infrequently. For more information about how to recognize a properly trained classifier, see [Analysis, adjustment, and additional settings](S04_Analysis_adjustment_and_additional_settings.md).

It is often preferable to train from the ground up, when the classifier's internal weights are unset and totally based on your training settings and data. For CNN based classifiers, this is typically known as a complete training. If your data is limited, you might consider using a pretrained CNN classifier as your starting point. Ideally, the pretrained classifier that you use has already learned to generalize a similar problem to a certain degree, and its internal parameters have been adjusted accordingly. You can therefore build off those adjustments by continuing to train it with your data. This type of training, for CNN based classifiers, is considered transfer learning. For more information, see [Training mode controls](S03_Fundamental_decisions_and_settings.md). Training an already trained tree ensemble classifier is considered a warm start.

The following image represents a general overview of what is required to train a classifier (in general, one that is CNN based).

*[Image: mclasstheoreticaloverview.png]*

The set of images with which you train the classifier must be representative of the actual application. The images should come from the final imaging setup (for example, the same camera, lens, and illumination), and should include the various aspects of the product, as well as its expected variations (for example, changes in scale, rotation, translation, illumination, color, and focus). The size of the images is determined by the application, such as the dimensions of the objects or features to classify, as well as by the specific predefined CNN classifier that you are using.

## Train engine

The hardware (CPU/GPU) used for training is referred to as the train engine. By default, Aurora Imaging Library typically uses the best available GPU (GPU training is highly recommended). You can modify this by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TRAIN_ENGINE`](../../Reference/class/MclassControl.md) or in the **Aurora Imaging Configurator** utility.

You must have installed the appropriate Aurora Imaging Library add-on to train on your GPU. For more information, see [Aurora Imaging Library add-ons and updates](../C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md).
