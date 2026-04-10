---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Training
section: Basic_concepts
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Training / Basic concepts"
---

# Basic concepts for training

The basic concepts and vocabulary conventions for training are:

- **Bootstrap aggregating (bagging)**. Selecting entries to train a tree ensemble classifier. Entries are either in-the-bag or out-of-bag (per tree). Aurora Imaging Library uses bootstrapping, bagging, randomness, and multiple learning algorithms to maximize the accuracy and performance of a tree ensemble classifier.
- **Complete training**. A training mode that resets the classifier's weights. This is for completely restarting the training of a predefined CNN or segmentation classifier, or for training an untrained CNN or segmentation classifier.
- **Confusion matrix**. A type of table, in a matrix format, that presents information about how many entries were, for each class, correctly classified, and how many were, for each class, confused with other classes, during training. A confusion matrix is also known as an error matrix.
- **Epoch**. One full cycle of the dataset during training of a predefined CNN or segmentation classifier.
- **Fine tuning**. A training mode designed for a predefined CNN or segmentation classifier that is mostly trained. You can fine tune a classifier if you want to add additional training data without restarting the entire training process. Additional data must have the same image size and the same number of classes.
- **In-the-bag**. The dataset entries that Aurora Imaging Library randomly selects (per tree) during bootstrap aggregating, to train a tree ensemble classifier. The random selection is done with replacement; the randomly selected entries are available for reselection.
- **Intersection over union (IOU)**. A metric that evaluates a classifier's performance by measuring the overlap between the ground-truth and predicted region. For example, in segmentation, if two regions are identical and completely overlap, then the IOU is equal to 1.
- **Loss**. The result of a mathematical loss (or cost) function that Aurora Imaging Library uses to evaluate the confidence associated with the classification during training. After each epoch, Aurora Imaging Library uses the loss value to adjust the classifier's weights to help achieve a lower loss at the end of the next epoch.
- **Mini-batch**. A subset of entries in an images dataset. Since such datasets typically contain numerous entries which require a considerable amount of memory to manage, the training process randomly splits them into groups, or mini-batches, to improve efficiency.
- **Out-of-bag**. The dataset entries that are not in-the-bag (per tree). Aurora Imaging Library uses these to evaluate the performance of the classifier's training and regulate over-fitting.
- **Overfitting**. When a classifier is trained too precisely on the training data, that it performs poorly when generalizing to other data that is similar. To prevent overfitting and develop a properly generalized classifier, Aurora Imaging Library uses a development dataset or out-of-bag entries.
- **Train engine**. The processing device (for example, the GPU or CPU) on which training is performed.
- **Training mode**. The process by which Aurora Imaging Library trains a predefined CNN or segmentation classifier and the extent to which it should use previously learned information. You can train with a complete process, a transfer learning process, or a fine tuning process. Changing the training mode affects how the training process establishes and evolves the classifier's weights. Training mode controls are also known as hyperparameters.
- **Transfer learning**. A training mode where the network weights for the feature extraction layers are those from the predefined CNN or segmentation classifier. This mode is used to adapt a pretrained classifier to a new, similar classification problem.
