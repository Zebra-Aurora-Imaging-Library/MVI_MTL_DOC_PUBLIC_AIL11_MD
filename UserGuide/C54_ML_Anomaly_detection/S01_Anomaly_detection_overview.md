---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Anomaly_detection
section: Anomaly_detection_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Anomaly_detection / Anomaly detection overview"
---

# Anomaly detection overview

This chapter explains how to perform anomaly detection using machine learning with the Aurora Imaging Library Classification module.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | Note that this chapter expands on topics previously discussed in [Machine learning fundamentals](toc.md#Machine_learning_overview_and_fundamental_concepts). It is recommended to review these topics if you have not already done so. |

Anomaly detection lets you identify invalid images (for example, images with defects), after having trained the classifier on valid images (for example, images without defects). This can be particularly useful when valid images are plentiful and easy to acquire, while invalid images are rare and can have a variety of abnormalities that make them invalid, such as scratches, chips, dents, and foreign objects.

*[Image: MclassAnomalyDetectionOverview.png]*

Since only valid (non-anomalous) images are used in training, there is no labeling needed for your datasets. This can be quite advantageous, as labeling is typically a long and costly process. You can also use preexisting datasets that have already been labeled, such as datasets built for image classification, segmentation, and object detection. In these cases, existing labels remain unaffected.

In addition to identifying anomalous images, anomaly detection can also localize the anomalies by identifying their coarse pixel location.

*[Image: MclassAnomalyDetectionOverviewLocalization.png]*

Note that with anomaly detection, you can retrieve image type results (similar to image classification) or pixel type results (similar to segmentation). For example, you can retrieve whether an image is anomalous, or which pixels in an image are anomalous.

Although effective as a stand-alone machine learning task, anomaly detection can also be useful as a preliminary training step for other machine learning tasks, such as image classification and segmentation. For example, if you ultimately want to identify a variety of different defects in an image, you can use anomaly detection to find all defective images first. This lets you know which images should be sent for manual labeling so the specific defective class description can be assigned, and which images do not need any manual labeling since you have already determined they are defect free. Reliably reducing the amount of image data that requires manual labeling can often cut costs and development time significantly.

## Testing with the trained threshold and statistics

Once the anomaly detection classifier is trained, a threshold is established to differentiate the anomalous images from the non-anomalous ones. At this point, you can optionally use the trained classifier to predict on your testing dataset. Since the testing dataset is intended to mimic a real life scenario, it must contain some anomalous images. If after testing you are satisfied with your results, it means the trained threshold needs no adjustment and you can deploy.

If you are not identifying all the anomalous images you expect, or if you are misidentifying good images as anomalous, then you can adjust your trained threshold, and rerun the prediction testing (there is no retraining needed). Adjusting the threshold is often done with performing a statistical analysis of your dataset ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)). That is, you can analyze a trained classifier's performance by computing metrics on predicted datasets, which can give you some guidance on adjusting the threshold. For more information, see [Adjusting the trained threshold and calculating statistics](S06_Adjusting_the_trained_threshold_and_calculating_statistics.md).
