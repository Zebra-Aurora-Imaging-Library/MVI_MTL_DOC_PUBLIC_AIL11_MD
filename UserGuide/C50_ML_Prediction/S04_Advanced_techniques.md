---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Prediction
section: Advanced_techniques
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Predicting_and_advanced_techniques / Advanced techniques"
---

# Advanced techniques

This section describes advanced techniques for prediction.

## Assisted labeling

You can use prediction to help label your dataset by calling [`MclassPredict`](../../Reference/class/MclassPredict.md)with a trained classifier context and a dataset context (rather than a target image or a set of features). The ground truth of an entry in the dataset can then be set to the predicted label. This process is known as assisted labeling or active learning.

Using [`MclassPredict`](../../Reference/class/MclassPredict.md) this way is typically done when you have so much data with which to train (hundreds of thousands of images or features), that it is unrealistic to manually label all of it (unless the data gathering process naturally sorts out the classes). To assist with labeling, a random subset of the entries (images or sets of features), such as a few hundred per class, can be labeled by prediction, and then used in the dataset to continue training the classifier.

After prediction, you need to add the new label within your dataset. You should only accept the predicted label as the ground truth if you are very confident in the predicted label result; for example, when you conclude that the score is robust and above an extremely high value, such as 99%.

Adding this new labeled data to the training dataset is known as exploitation. In general, you would have an expert label a new subset of the remaining data which is known as exploration. This entire process, iterated until a satisfactory performance, assumes that the data picked and labeled by the expert constitutes a dataset of good quality and quantity.

When performing the prediction operation on a target dataset, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md). To get information about the prediction events that caused the hook-handler function to execute, call [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md). This is useful, for example, to add the new label within your dataset if the prediction score of a particular class is above the specified threshold.

## Identifying anomalies

Although anomaly detection can be an effective stand-alone machine learning task, it can also be useful as a type of preliminary identification step for other machine learning tasks, such as image classification, segmentation, and object detection.

For example, if you ultimately want to identify a variety of different defects in an image, you can use anomaly detection to find all defective images first. This lets you know which images should be sent for manual labeling so the specific defective class description can be assigned, and which images do not need any manual labeling since you have already predicted that they are defect free.

Reliably reducing the amount of image data that requires manual labeling can often cut costs and development time significantly.

## Extracting

In some applications, there might be a need to extract and to preprocess a region of an image to be classified. For example:

- Only an ROI within an image needs to be classified and the location of this ROI can be established using other means such as from a pattern matching occurrence.
- Multiple ROIs within an image with different labels need to be extracted, either manually or automatically using a segmentation technique, for example.
- Fixturing, if applicable, might be used to also correct an image or a ROI of geometric variations (rotation, scale, or translation). The reduction of possible variations simplifies the classification problem, increasing the chance to obtain a better/faster trained network.

You might also need to extract and preprocess a region of the target image before the prediction in your final application. For more information, see[Child buffers, regions of interest, and fixturing](../C02_Building_an_application/S10_Child_buffers_regions_of_interest_and_fixturing.md).
