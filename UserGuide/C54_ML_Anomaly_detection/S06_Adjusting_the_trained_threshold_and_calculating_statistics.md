---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Anomaly_detection
section: Adjusting_the_trained_threshold_and_calculating_statistics
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Anomaly_detection / Adjusting the trained threshold and calculating statistics"
---

# Adjusting the trained threshold and calculating statistics

Ideally, training images contain all expected variations of good images, and any difference would be an anomaly. In practice, this is rarely the case. To help determine how different an image can be, before it is considered anomalous, Aurora Imaging Library automatically establishes a threshold for the trained classifier, which you can optionally modify (fine tune) to help fit your application's needs.

Evaluating the threshold requires using the trained classifier to predict on your testing dataset. Since this is intended to mimic a real life scenario, the testing dataset must contain some anomalous images. If after testing you are satisfied with your results, it means the trained threshold needs no adjustment and you can deploy.

To specify anomalous data in your testing dataset, you must call[`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_INDEX()`](../../Reference/class/MclassControl.md), and set [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) to [`M_TRUE`](../../Reference/class/MclassControl.md). By default, every dataset entry is considered not anomalous, unless you explicitly identify it this way; that is, you specify that one or more classes are anomalous (for example, you set [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) to [`M_TRUE`](../../Reference/class/MclassControl.md) for _Class01_), and then you assign entries in the dataset to that class (for example, the class of anomalous entries have their ground truth set to_Class01_). If multiple classes are identified as anomalous (for example, _Class01_, _Class02_, and _Class03_ all have [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) set to [`M_TRUE`](../../Reference/class/MclassControl.md)), their anomalous data is used as a type of super-anomalous-class.

Adjusting the threshold is often done with performing a statistical analysis of your dataset ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)). That is, you can analyze a trained classifier's performance by computing metrics on predicted datasets, which can give you some guidance on adjusting the threshold.

## Score threshold

The trained classifier's threshold level is based on an internal calculation established during training that evaluates scores, and when they more likely indicate that a result is anomalous or not. By increasing or decreasing the trained classifier's threshold (score evaluation) control, which you would do prior to performing a prediction on a test dataset with the trained classifier, you can influence which images and pixels are or are not anomalous.

To fine tune the trained threshold, you must call [`MclassControl`](../../Reference/class/MclassControl.md) with the identifier of the trained classifier context and adjust the [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) setting (this is done post training). Lowering the [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) value can allow you to detect more anomalous cases, but at the potential cost of incorrectly detecting good (non-anomalous) cases as anomalous ones. Conversely, increasing the [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) value can decrease the likelihood of incorrectly detecting good non-anomalous cases as anomalous, but at the potential cost of missing actual anomalous ones.

The following is an example of a score analysis in a real-life scenario, such as a testing dataset, and therefore contains anomalous results.

*[Image: MclassAnomalyDetectionScoreThreshold.png]*

With regards to this example, note:

- If you perform this score analysis on another real life scenario, such as another testing dataset or on prediction results from a production line, the trained threshold would not land at the same location as in this example (where it lands depends on the target images in that scenario).
- If you perform this score analysis on the development dataset, there would be no anomalous score results (red line), since the development dataset must only have good images (since there are only good images, no labeling is needed). Also, in the development dataset, the trained threshold is going to be to the right of the highest score.
- If you perform this score analysis in an ideal scenario, the curves (anomalous/non-anomalous) would not overlap.

As previously discussed, there are no labeling requirements for the training and development datasets. Optionally, the testing dataset is labeled with anomalous images only if you want to evaluate the threshold and compute statistics ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)). It can be labeled as a classification or segmentation dataset depending on the type of statistics you need; that is, if you need image statistics or pixel statistics.

Also, as previously discussed, the trained classifier's threshold level ([`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md)) is based on an internal calculation established during training. This value is always represented as 50 initially, as this is the convention for indicating the relative point from which you can make adjustments. For example, to make it easier to find anomalous cases, you can lower [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) to 45, and to make it harder to find anomalous cases, you can increase it to 55. At any time you can put back the originally trained threshold by setting [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) to 50 (or [`M_DEFAULT`](../../Reference/class/MclassControl.md)).

To get a real-life sense for the classifier's performance after analyzing different statistics, you can perform a variety of drawing operations, such as drawing anomalous pixels, scores, heat maps, and contours. For more information, see [Drawing results](S07_Prediction_for_anomaly_detection.md).

To retrieve information about calculated statistics, call [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md). For more information about the statistics you can calculate, refer to the _ClassAnomalyDetectionCompleteTrain.cpp_ example in Aurora Imaging Example Launcher.
