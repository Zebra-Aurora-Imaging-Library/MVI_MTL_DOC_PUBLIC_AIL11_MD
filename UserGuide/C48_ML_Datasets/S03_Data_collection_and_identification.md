---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Datasets
section: Data_collection_and_identification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Datasets / Data collection and identification"
---

# Data collection and identification

The data (images or features) in your datasets must properly represent what you are trying to classify. The quality and quantity of your data directly impacts the classifier's training. To help ensure proper data, collect it using the final imaging setup (the same camera, lens, and illumination) and use real samples (images) of the subject matter, or as close as possible to them.

Your dataset should include all expected variations of what you want to classify, and in sufficient number, such as variations in aspect, color, intensity, rotation, and dimension. Pay special attention to variations that are difficult to obtain and, if necessary, use augmentation to synthesize such variations (for example, rotation and flip). The following example illustrates variations (images) of a single apple object (class).

*[Image: MclassClassesAllVariations.png]*

To classify multiple objects, each of which is a separate class (for example, apples, oranges, and pears), you must provide, for each class, numerous training images of every variation. Aurora Imaging Library internally divides that data, which usually occupies a lot of memory, into several sets (for example, by using mini-batches or bagging). Gathering as much data as possible helps ensure it is evenly consumed by training.

It is recommended to have a minimum of 500 dataset entries (images or sets of features) per class, although simple applications can require less, and complex applications can require more. The number of entries required to properly train a classifier context depends on the complexity of the problem, the number of classes, and the number of variations within the classes. Typically, the data in all your datasets come from the same distribution (the same environment and setup).

A balanced dataset is key to achieving a properly trained classifier. In a balanced dataset, each class should have the same number of entries representing it and, within each class, you should have the same number of entries representing each variation. You can obtain a balanced dataset by performing data augmentation with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). Obtaining a perfectly balanced dataset for segmentation or object detection is not always possible because an image might contain multiple regions with different labels.

> **Note:** These dataset recommendations are for a complete (default) training, which means you are training a classifier from the ground up; other types of training, such as transfer learning, require less data. For more information, see [Training modes](../C49_ML_Training/S03_Fundamental_decisions_and_settings.md).

## Data foresight

The data that you collect and the classes with which it is organized must represent, as much as possible, a well-posed and consistent problem that you want to train the classifier to solve. Be aware that there are usually several ways to formulate a problem and present the data to solve that problem; it is imperative that this is done in a consistent and perceptive manner. You must know the problem that you want to solve and provide the data to solve it, while at the same time being mindful of ambiguities or unwanted associations that could lead to uncertain or even mistaken classifications.

Illumination variances in your data represents one of the ways a classifier can make inadvertent associations. If illumination is irrelevant to determining the class, the data with which you train should contain different illuminations to remove the bias that a specific illumination condition can introduce.

For example, if your dataset has images of good parts taken near a window on a cloudy day and images of defective parts taken near that window on a sunny day, it is likely for the classifier to learn that a dark image means a good part and a bright image means a defective part. It is vital to anticipate the important variations and provide that data; this allows the classifier to properly learn how to solve the problem without unwanted bias.

## Classes and labeling (the ground truth)

The Classification module uses supervised training, which requires you to label your dataset entries with the class they represent. This label is called the ground truth (GT). Class labels must be unique. Datasets often have two or more class labels, although this depends on the problem you are solving. Object detection, for example, requires at least one class, and anomaly detection requires no explicit classes (although implicitly one is inferred).

Although anomaly detection does not require labeling, it can be considered an implied two-class prediction problem. By default, all dataset entries are considered non-anomalous (which can infer a type of ground truth label that identifies the image as non-anomalous). Once these images are learned at training time, the classifier can then implicitly predict two classes of images; that is, the non-anomalous ones, and the anomalous ones/everything else.

For an explicit two-class problem, which applies to other machine learning tasks, you could specify one of two class labels for every dataset entry; that is, every image (or image region), or every set of features in a dataset, is identified as one of these two classes. This can in some cases prove more practical than anomaly detection; for example, if you have a more significant number bad images, it might make sense for the classifier to explicitly learn them, along with the good images. In such cases, your two class labels could be generally defined as _Good_ and _Bad_.

For an _n_-class problem, you could specify one of _n_-labels (for example, _ClassMetal_, _ClassCarpet_, or _ClassWood_) for every dataset entry (every image or set of features in a dataset is identified as one of these _n_-classes). Another set of labels to use for such problems is _ClassMetalGood_, _ClassMetalWithHole_, or _ClassMetalWithScratch_. Note, for image datasets, you often have one label (class) per image region (therefore, you can have multiple labels and classes for each image) rather than one label per image (which is generally for image classification).

To label whole images and image regions (that is, identify the class they represent), you can use Aurora Imaging CoPilot. Alternatively, you can build a simple utility that calls Aurora Imaging Library.

If you have a large amount of data to label, it might be possible to label some of it, train your classifier context, call the prediction operation to label the unlabeled dataset entries, and then use those newly labeled entries in your training dataset to continue the training process. For more information, see [Assisted labeling](../C50_ML_Prediction/S04_Advanced_techniques.md).

Another option for managing data labeling is using anomaly detection as a preliminary step for other machine learning tasks. For example, if you ultimately want to identify a variety of different defects in an image, you can use anomaly detection to find all defective (anomalous) images first. This lets you know which images should be sent for manual labeling so the specific defective class description can be assigned (that is, the specific type of anomaly), and which images do not need any manual labeling since you have already determined they are defect free (non-anomalous). Reliably reducing the amount of image data that requires manual labeling can often cut costs and development time significantly.

## UUID

A UUID refers to a universally unique identifier that is used for identification purposes across unrelated systems and applications. The following is an example of an automatically generated key (UUID): 87fbb05a-b078-4389-8ad2-2a2f9707c8f4.

Every dataset entry has a UUID value that uniquely identifies it; this is also known as the entry's key. Since datasets can contain numerous entries from multiple datasets constructed on unrelated systems, this universally unique key helps ensure that the entries from all the datasets are not confused with each other.

Aurora Imaging Library automatically generates a UUID when required, such as, when you add an entry to a dataset ([`M_ENTRY_ADD`](../../Reference/class/MclassControl.md)). Although you do not create you own UUIDs, you can use them to access an entry to, for example, inquire about it or modify it.

The Aurora Imaging Library custom data type for UUIDs is _AIL_UUID_. When using an _AIL_UUID_ variable, Aurora Imaging Library defines the **M_DEFAULT_UUID** and **M_NULL_UUID** constants, which allow you to specify the corresponding default UUID value or a null UUID value.

### AIL_UUID utility macros for C users

To perform comparisons with _AIL_UUID_ values in C, use the following macros:

- **M_COMPARE_AIL_UUID(VarA, VarB)**.
  This macro evaluates an equal (==) comparison operation between two variables.
- **M_IS_DEFAULT_UUID(VarA)** or **M_IS_DEFAULT_KEY(VarA)**.
  These macros evaluate whether a variable is equal to the default UUID. These macros are equivalent.
- **M_IS_NULL_UUID(VarB)** or **M_IS_NULL_KEY(VarB)**.
  These macros evaluate whether a variable is equal to a null UUID. These macros are equivalent.

The following are examples of how to use these macros in C:

```
AIL_UUID VarA = M_DEFAULT_UUID;
AIL_UUID VarA = M_DEFAULT_UUID;
```

```
if(M_COMPARE_AIL_UUID(VarA, VarB)) 
 { /* Do something if VarA == VarB; */ }
```

```
if(M_IS_DEFAULT_UUID(VarA)) 
 { /* Do something if VarA == M_DEFAULT_UUID; */ }
```

```
if(M_IS_NULL_UUID(VarB)) 
 { /* Do something if VarB == M_NULL_UUID; */ }
```

Note, you can use _AIL_UUID_ variables normally in a C++ program, since the _AIL_UUID_ data type, in C++, implements the equal (==) and not equal (!=) comparison operators.
