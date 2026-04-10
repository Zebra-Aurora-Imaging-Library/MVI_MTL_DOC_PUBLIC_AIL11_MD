---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_with_the_MIL_Classification_module
section: Classification_module
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Classification_and_classifier_overview / Classification module"
---

# Aurora Imaging Library Classification module overview

The Aurora Imaging Library Classification module lets you apply machine learning technologies such as deep learning and decision trees to perform [image classification](S05_Which_task_is_right_for_you.md), [segmentation](S05_Which_task_is_right_for_you.md), [object detection](S05_Which_task_is_right_for_you.md), [anomaly detection](S05_Which_task_is_right_for_you.md), and [feature classification](S05_Which_task_is_right_for_you.md).

To carry out these tasks, the module uses algorithmic architectures known as classifiers. These technologies, and the tasks you can perform with them, help you solve identification and categorization issues that are not easily addressed with traditional image processing techniques.

For example, consider trying to identify a specific type of pasta, among similarly shaped pasta, in images filled with pasta that are randomly positioned, overlapping, and pointing in every direction.

*[Image: MclassPasta.png]*

Categorizing the kind of pasta in such images by defining and distinguishing the groups of features to extract, as is required by traditional image processing techniques, would likely prove problematic. The Aurora Imaging Library Classification module makes this problem more easily solvable by taking a different approach.

First, you would collect and label multiple images of each kind of pasta and add them to a dataset context (the Aurora Imaging Library object that holds the data). Then, you would use the dataset to train a classifier context (the Aurora Imaging Library object that holds the classification technology). During training, the classifier learns to recognize the different kinds of pasta. Once training is done, you can use the trained classifier to properly predict the kind of pasta in your target images.

The Classification module provides the tools to use critical machine learning processes, such as building and labeling a dataset, augmenting and preparing data to expand a dataset, controlling train settings, analyzing train and predict (inference) results, and importing a trained ONNX format classifier for prediction. Many of these processes can be performed interactively, using Aurora Imaging CoPilot.

The Classification module also supports the statistical analysis of datasets ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)). That is, you can analyze a classifier's performance by computing metrics on predicted datasets.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | This part of the documentation ([Machine learning fundamentals](toc.md#Machine_learning_overview_and_fundamental_concepts)) covers what you need to know about classifiers, datasets, training, and prediction, to perform machine learning with Aurora Imaging Library. It is recommended that you read the fundamentals before starting to build your application. Specific instructions vary according to the task you want to perform, and are covered in the [Machine learning tasks](toc.md#Machine_learning_tasks) part. For the most recent documentation of these fundamentals, particularly as they relate to anomaly detection and statistical analysis ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)), check for an updated version of the Aurora Imaging Library Help online at [zebra.com/ail-docs](https://zebra.com/ail-docs). |
