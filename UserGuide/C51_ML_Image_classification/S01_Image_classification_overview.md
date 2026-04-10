---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Image_classification
section: Image_classification_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Image_classification / Image classification overview"
---

# Image classification overview

This chapter explains how to perform image classification using machine learning with the Aurora Imaging Library Classification module.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | Note that this chapter expands on topics previously discussed in [Machine learning fundamentals](toc.md#Machine_learning_overview_and_fundamental_concepts). It is recommended to review these topics if you have not already done so. |

Image classification allows you to classifying an entire image; it typically requires a predefined CNN classifier context that was defined by Zebra (an ICNet), and that must be trained with an images dataset context. To train a classifier, you must supply it with many images that are representative of the real-world problem the classifier will solve, along with a label for each image identifying its class. The classifier will learn from these training images how to differentiate the various classes. An example of image classification is identifying different types of images that have complex and similar features, such as different types (classes) of pasta, as shown here.

*[Image: MclassPasta.png]*

To solve this problem, and identify different classes of pasta, a classifier must be trained on many images of each class of pasta (for example, hundreds of images of each class). After an ICNET classifier is trained, it can be used to predict the class to which a previously unseen image belongs. Ultimately, the classifier gives each predicted image a score, per defined class, to indicate the degree to which that class represents the image. The defined class with the highest score is the one that best represents the image. The following example shows an image and its predicted class results. The best class is highlighted.

*[Image: MclassPastaResults.png]*

The classification of pasta above is a multiclass classification problem. Image classification can also be used to perform binary classification, predicting whether or not an image belongs to an invalid or invalid class (such as _BadApple_ or _GoodApple_).

*[Image: MclassCNNGlobalClassification.png]*

For binary type classifications such as identifying the anomalous (bad) images from the non-anomalous ones (good), it is recommended that you try to perform [anomaly detection](../C54_ML_Anomaly_detection/S01_Anomaly_detection_overview.md), as it has been specifically designed for such cases.
