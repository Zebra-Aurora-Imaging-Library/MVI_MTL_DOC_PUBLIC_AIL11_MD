---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_with_the_MIL_Classification_module
section: Basic_concepts
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Classification_and_classifier_overview / Basic concepts"
---

# Basic concepts for machine learning

The basic concepts and vocabulary conventions for the Aurora Imaging Library Classification module are:

- **Anomaly detection network (ADNet)**. Internal classifier (network) that you can specify when you allocate a predefined anomaly detection classifier context to perform anomaly detection; for example, [`M_ADNET`](../../Reference/class/MclassAlloc.md).
- **Artificial Intelligence (AI)**. The simulation of intelligence in machines.
- **Classes **. The unique categories in a finite number of categories that represent the conclusions to a classification problem. Fusilli and macaroni, for example, represent 2 classes for categorizing pasta. Classes can hold supplementary data that is typically used as a visual aid, such as an identifying icon image or color. You can add classes to a dataset context. Classes are also known as class definitions, labels, or outputs.
- **Classification**. Applying mathematical learning processes, such as machine learning or deep learning, to solve identification and categorization issues known as classification problems. Typically, such problems are not easily solvable using traditional image processing techniques.
- **Classifier**. The mathematical architecture that must be trained to perform classification (predict the class to which a target belongs). You can have a predefined CNN classifier, a predefined segmentation classifier, a predefined object detection classifier, or a tree ensemble classifier. A classifier is also known as a network or a model.
- **Classifier context**. An Aurora Imaging Library object that stores a classifier and its settings. The terms classifier context and classifier are often used interchangeably.
- **Convolutional neural network (CNN)**. A type of neural network typically used to learn from, and predict for, image data.
- **Dataset**. A type of database containing a labeled series of entries (images or features) with which to train a classifier.
- **Dataset context**. An Aurora Imaging Library object that stores a dataset and its settings. The terms dataset context and dataset are often used interchangeably.
- **Decision tree**. A part of ML that tries to process (separate) data as a collection of decision-based nodes connected by branches.
- **Deep learning (DL)**. A part of ML focusing on learning techniques related to neural networks.
- **Image classification network (ICNet)**. Internal classifiers (networks) that you can specify when you allocate a predefined CNN classifier context to perform image classification; for example, [`M_ICNET_M`](../../Reference/class/MclassAlloc.md).
- **Machine learning (ML)**. A part of AI focusing on systems that can learn from data to make decisions with minimal human assistance or explicitly programmed procedures.
- **Neural network**. Algorithms based on biological neuron network systems that attempt to recognize and distinguish patterns and relationships in the provided data.
- **Object detection network (ODNet)**. Internal classifier (network) that you can specify when you allocate a predefined object detection classifier context to perform object detection; for example, [`M_ODNET`](../../Reference/class/MclassAlloc.md).
- **Predefined CNN**. A classifier that uses a CNN that was predefined by Zebra, and that must be trained with an images dataset. Once trained, you can use the predefined CNN classifier to perform image classification (classify an entire image). CNN and image classification are used interchangeably unless otherwise specified.
- **Predefined object detection**. A classifier that uses an object detection specialized network that was predefined by Zebra, and that must be trained with an images dataset. Once trained, you can use the predefined object detection classifier to segment an image.
- **Predefined segmentation**. A classifier that uses a segmentation specialized network that was predefined by Zebra, and that must be trained with an images dataset. Once trained, you can use the predefined segmentation classifier to segment an image.
- **Predicting**. Using a trained classifier to identify the class to which the target (image, feature list, or dataset) belongs. Predicting, predict, and prediction are used interchangeably and are also known as classification or inference. Such terms are sometimes used even when the actual Aurora Imaging Library prediction function is not. For example, training can be seen as a type of prediction, since the classifier is essentially learning how to predict.
- **Receptive field**. The portion of the input image that is visible to the classifier to make a prediction.
- **Segmentation network (CSNet)**. Internal classifiers (networks) that you can specify when you allocate a predefined segmentation classifier context to perform segmentation; for example, [`M_CSNET_M`](../../Reference/class/MclassAlloc.md).
- **Source layer**. The input (initial) layer of a classifier (for example, CNN, segmentation, object detection, or ONNX).
- **Training**. The process in which the classifier learns to predict the class to which the data belongs.
- **Training context**. An Aurora Imaging Library object that stores the training settings with which to train a classifier. You can have a CNN training context, a segmentation training context, or a tree ensemble training context.
- **Tree ensemble**. A classifier that uses multiple decision trees and bootstrap aggregating. After training this classifier with a features dataset, you can use it to perform feature classification.
- **Weights**. Internal and hidden parameters within the classifier that transform data. Classifiers can have millions of weights. During most types of training, modifications to the training mode controls (hyperparameters) can affect how weights are established. Once a classifier is trained, its weights no longer change. Weights are sometimes referred to as learnable parameters.

Additional basic concepts and vocabulary conventions for the Aurora Imaging Library Classification module, more specifically related to [datasets](../C48_ML_Datasets/ChapterInformation.md), [training](../C49_ML_Training/ChapterInformation.md) and [prediction](../C50_ML_Prediction/ChapterInformation.md), are listed in their respective chapters.
