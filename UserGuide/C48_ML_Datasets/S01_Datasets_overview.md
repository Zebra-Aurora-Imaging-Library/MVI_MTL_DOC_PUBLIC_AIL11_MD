---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Datasets
section: Datasets_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Datasets / Datasets overview"
---

# Datasets overview

Datasets are the lifeblood of machine learning. Regardless of the task (image classification, segmentation, object detection, anomaly detection, or feature classification), a well trained classifier is always the goal, and this is only possible with properly constructed datasets. Investing time into building good datasets will make your classifier more robust and save you time in training.

A good dataset includes data that you have collected, such as training images, that is correctly labeled and representative of the task to perform. The quality, quantity, and proportionality of accurate data is critical to building a good dataset. Having a proper set of labeled data is a prerequisite for using the Aurora Imaging Library Classification module (and, in general, for using supervised classification technologies).

> **Note:** Although anomaly detection does not require training images that are explicitly labeled, you must only train with non-anomalous (good) images. This type of vetted (supervised) data lets the training infer that every image is _Good_, which can be considered a type of implied label. Even if your images are labeled (for example, if they were used for another machine learning task), the anomaly detection training considers them non-anomalous images by default. Inadvertently training with one or more supposedly good images that are actually anomalous (and not labeled as such) will likely result in a poorly trained classifier. If you train with dataset entries that refer to images that are explicitly labeled as anomalous, the training process will notify you with an error (note that your testing dataset can reference images labeled as anomalous if you are performing a prediction with it to validate the trained classifier).

It is recommended to decouple dataset creation from usage. Since constructing a dataset involves disk storage, it should be done first, properly, and on its own. Once the dataset is ready, you can use it (for example, import it) for training or prediction. For more information, see [Guidelines for managing an images dataset](S07_Guidelines_for_managing_an_images_dataset.md).

## Images dataset versus features dataset

You can allocate two types of datasets with Aurora Imaging Library: one for image data and one for feature data. The one you use depends on the task you want to perform.

| Dataset | Task |
| --- | --- |
| Images dataset ([`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md)) | ✔ | ✔ | ✔ | ✔ | - |
| Features dataset ([`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md)) | - | - | - | - | ✔ |

All datasets share a common organizational structure that is like a table; each row represents one complete and representative instance of your data (an entry), and each column represents the multiple parts of information you need to define that data.

Since an images dataset holds images, each entry (which is like a row) should contain information about that image (which is like that row's columns), such as the path where the image is stored. Image classification and anomaly detection require an entry image; segmentation, object detection, and anomaly detection (if you want to compute pixel level statistics) should also specify multiple regions (for example, regions identifying defects).

Since a features dataset holds numerical data, each entry should contain values that identify features, such as a series of blob features (for example, length, width, and height).

Regardless of the information held (whole images, images and regions, or features), each entry should also hold the class label (ground truth) that identifies the data it represents (for anomaly detection, each entry has an implied non-anomalous label). For example, if you are performing image classification to identify apples and oranges, each entry must not only specify an image of a specific apple or orange, it must also codify that data with the corresponding class label (_Apple_ or _Orange_). For segmentation or object detection, there would be multiple class labels per entry, corresponding to each region (for example, an image with 3 objects would have 3 regions, and each region would have a class label identifying the type of object). For feature classification, there would be multiple feature values and 1 class label identifying what those values mean.

If you are performing anomaly detection, for example, to find anomalies in bottle tops, each entry for training must specify an image of a non-anomalous bottle top (if the entry has a class label, it is by default assumed to be not anomalous, unless it was explicitly labeled as anomalous - at training time, images labeled as anomalous will produce an error).

The following is an example of dataset entries for the different machine learning tasks. Note, as discussed later, dataset entries can also hold other information, such as a unique key ([UUID](S03_Data_collection_and_identification.md)).

|   |
| --- |
| Example of dataset entries for image classification |
| _ImageFileLocation-01_ | _ClassLabel_ |
| _ImageFileLocation-02_ | _ClassLabel_ |
| ... | ... |
| Example of dataset entries for anomaly detection |
| _NonNnomalousImageFileLocation-01_ | If there is a _ClassLabel_, it is ignored by default. |
| _NonNnomalousImageFileLocation-02_ | If there is a _ClassLabel_, it is ignored by default. |
| ... | ... |
| Example of dataset entries for segmentation and object detection |
| _ImageFileLocation-01_ This image has 3 regions. | _ClassLabel_, _ClassLabel_, _ClassLabel_ |
| _ImageFileLocation-02_ This image has 4 regions. | _ClassLabel_, _ClassLabel_, _ClassLabel_, _ClassLabel_ |
| ... | ... |
| Example of dataset entries for feature classification |
| _FeatureValue-01, FeatureValue-02, FeatureValue-03_ | _ClassLabel_ |
| _FeatureValue-01, FeatureValue-02, FeatureValue-03_ | _ClassLabel_ |
| ... | ... |

Although each entry represents a real world example of your data, it is the dataset as a whole, that must be representative of the entire real world environment from which the data exists. For example, if your factory (the real world) occasionally captures blurry images (approximately 10% of the time), and half your dataset contains blurry images, then your dataset as a whole is not a proper representation of the problem you are trying to solve, even though the individual entries (examples) are. Only a dataset that has proper entries and is as a whole properly populated can result in a well trained classifier.

Since datasets for anomaly detection training must only contain non-anomalous images, it can seem like they misrepresent a real world scenario. However, this classifier is specifically designed to learn the acceptable cases, and to see anything else as unacceptable. In addition, it is expected that anomaly detection is typically used for cases where the vast majority of the images you get are in fact not anomalous (getting anomalous images should actually be rare). When providing non-anomalous training images, you must ensure that all varieties of non-anomalous images are included; for example, if you accept certain imperfections, then include images with those imperfections. During testing, you can include anomalous images in the testing dataset, to validate the trained classifier can properly predict them.

> **Note:** It is highly recommended to use Aurora Imaging CoPilot for managing your images datasets, and to have installed all related Aurora Imaging Library updates. For more information, see [Requirements, recommendations, and troubleshooting](../C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md).
