---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Datasets
section: Basic_concepts
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Datasets / Basic concepts"
---

# Basic concepts for datasets

The basic concepts and vocabulary conventions for datasets are:

- **Augmentation**. Creating plausible variations of a source dataset entry to increase the number of entries.
- **Dataset entry**. One row of fields in a dataset. The data defined in an entry's fields include the class (label) to which the entry belongs (the ground truth), an image (file name and path) or set of features (numerical values) representing that class, and a UUID (key) that uniquely identifies that entry. Dataset entries are also known as samples or inputs. Each entry in a features dataset is also known as a set of features or a feature set.
- **Descriptor**. Specifies a region in an entry image. Descriptors can be bounding boxes (for object detection), polygons, or masks (for segmentation). A single region can contain up to 1 mask descriptor, any number of polygon descriptors and up to 1 one box descriptor.
- **Development dataset**. The dataset that evaluates the performance of the classifier's training and regulates overfitting. Entries in the development dataset, training dataset, and testing dataset should be unique to their set. Tree ensemble classifiers do not typically require a development dataset; such classifiers use out-of-bag entries, which can evaluate the training's performance.
- **Entry image**. The image specified in an entry of an images dataset.
- **Features dataset**. A dataset that contains features (lists of values) that can be used with a tree ensemble classifier.
- **Ground truth (GT)**. The class to which a dataset entry belongs. You should determine this with direct human observation or, in some cases, assisted labeling.
- **Images dataset**. A dataset that holds the images with which to train a predefined image classification, segmentation, object detection, or anomaly detection classifier.
- **Labeling**. Specifying the class definition (ground truth) that a dataset entry represents. Training requires labeled entries. For segmentation and object detection, each dataset entry should have multiple class definitions (one for each region).
- **Source dataset**. A dataset that holds all the data with which to train a classifier. Typically, a source dataset is split into a training dataset, a development dataset, and a testing dataset.
- **Testing dataset**. An optional dataset containing data that was not seen by the classifier during training. A testing dataset can serve as a final check to assess the performance of the classifier. Entries in the testing dataset, training dataset, and development dataset should be unique to their set.
- **Training dataset**. The dataset that trains the classifier (the training dataset entries update the classifier's weights). Entries in the training dataset, development dataset, and testing dataset should be unique to their set.
- **Training image**. An image that is used to train a classifier.
- **Universally unique identifier (UUID)**. A unique key that identifies dataset entries, authors, and class definitions.
