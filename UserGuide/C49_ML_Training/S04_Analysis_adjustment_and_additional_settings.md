---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Training
section: Analysis_adjustment_and_additional_settings
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Training / Analysis adjustment and additional settings"
---

# Analysis, adjustment, and additional settings

Ideally, after you set up your training settings, call [`MclassTrain`](../../Reference/class/MclassTrain.md), and review your training results, you realize that your classifier is perfectly trained and you can use it with [`MclassPredict`](../../Reference/class/MclassPredict.md).

However training traditionally takes a substantial amount of time. Although this time includes analyzing training results, modifying training settings, and recalling the training function, it also includes the execution of the training function itself, and how it might be training incorrectly. In such cases, you can end up waiting a long time for the training to complete, only to find out that the training is completely wrong. This can be caused by several factors, such as inappropriate architecture, hyperparameters, and datasets.

For example, a very small initial learning rate (such as, 10^-6) can make a complete training process extremely slow. To help mitigate this, you can call two hook functions (also known as callbacks), one at the end of each mini-batch, and a second at the end of each epoch.

This hooking mechanism helps ensure that training is developing in the correct direction, while it is happening. Specifically, you can use [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) to hook a function to a training event, and then call [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md) to get information about the event that caused the hook-handler function to be called.

Hooking lets you retrieve critical training information, such as the current loss value, training dataset accuracy, and development dataset accuracy. Collecting this information helps you analyze the success of the training and to assess the classifier's performance. The training process can also be interrupted while in the hook (callback) function.

> **Note:** Inherently, training can take a lot of time; you should therefore typically expect to monitor the training process for proper convergence, and to make modifications to the process, or even to abort and restart it, if required. Proper convergence refers to increasing accuracy and minimizing error; in this way, you are converging to a classifier that properly identifies the class to which the data belongs.

Note, not all results and settings are available for every possible classification task, and all different type of configurations. When necessary, the corresponding functions indicate availability (for example, [`MclassGetResult`](../../Reference/class/MclassGetResult.md), [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md), and [`MclassControl`](../../Reference/class/MclassControl.md)).

## Results

To retrieve training results, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with the training result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produced.

Typically, you are not only interested in the accuracy of the classifier, but also in the robustness of that accuracy. A properly trained classifier has both a high accuracy (or, conversely, a low error or loss), and has also been trained in such a way that, if given similar input that it has not seen, will continue to behave as accurately as you expect. As discussed in the following sections, recognizing a properly trained classifier can sometimes be tricky business. To help, you can also get results from a statistics classification result buffer, using [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md).

### Paths and folders

Training and data preparation, for image classification and segmentation, can cause Aurora Imaging Library to write images to local folders. The paths to these folders can vary, depending on whether you are performing image classification or segmentation, or if you are using default, absolute, or relative paths. To retrieve such information, call [`MclassInquire`](../../Reference/class/MclassInquire.md), and specify the path or folder related setting, such as:

- [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md).
- [`M_SEGMENTATION_FOLDER`](../../Reference/class/MclassInquire.md).
- [`M_SEGMENTATION_FOLDER_ABS`](../../Reference/class/MclassInquire.md).
- [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassInquire.md).
- [`M_REGION_MASKS_FOLDER_ABS`](../../Reference/class/MclassInquire.md).
- [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassInquire.md).
- [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassInquire.md).

If the inquired information for [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassInquire.md) or [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassInquire.md) returns an empty string, the default path is being used; to inquire it, add [`M_DEFAULT_PATH`](../../Reference/class/MclassInquire.md) (for example, [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassInquire.md) + [`M_DEFAULT_PATH`](../../Reference/class/MclassInquire.md)).

For information about the default path, or to change it, go to the **Default destination folder** item under the **DL Classifiers** item in the **Aurora Imaging Configurator** utility. Most settings that you can inquire are settable using [`MclassControl`](../../Reference/class/MclassControl.md).

## Recognizing a properly trained classifier

A properly trained classifier is ready for prediction. But what does properly trained mean?

A properly trained classifier captures the general problem and successfully generalizes it so that it has a good accuracy on the trained data, and also on future data that is similar. This generalization requirement represents the main reason to organize data in separate datasets (for example, the training dataset and the development dataset); you must ensure that the classifier does not under-fit, nor over-fit, the problem.

Typical issues to suggest that a classifier is not properly trained include:

- The classifier over-fitting the training data, which usually means the classifier is not general enough to perform properly in the field (during prediction).
- Improperly augmented data, which can cause the classifier to learn how to solve a problem other than the one that was originally presented.

The following images illustrate two kinds of data represented by diamonds and circles, and the kinds of issues a classifier might have trying to differentiate between them. This differentiation, which is also known as the classifier's solution or data split, is indicated by a green line.

|   |   |   |
| --- | --- | --- |
| *[Image: MclassRecognizeProperlyTrained01.png]* | *[Image: MclassRecognizeProperlyTrained02.png]* | *[Image: MclassRecognizeProperlyTrained03.png]* |
| The classifier separates the data badly, given the numerous errors. Do not consider this classifier properly trained; its solution is too general as it under-fits the problem. | The classifier too precisely separates every detail of the data and disregards any underlying trend. This solution is unlikely to work on new data (prediction). Do not consider this classifier properly trained; it is not general enough as it over-fits the problem. | The classifier almost perfectly separates the data by capturing an underlying trend. You should consider this classifier properly trained; it has effectively generalized a solution. |

A properly trained classifier might also have to meet the following requirements:

- Exceeds the minimum accuracy or IOU.
- Does not exceed the maximum false positive and false negative rates.
- Does not exceed maximum advanced error metrics.
- Operates within an acceptable execution speed on the target platform.

As previously discussed, Aurora Imaging Library uses the development dataset, which is required for training a CNN, to help regulate training issues such as overfitting (to a certain degree, tree ensemble classifiers use bagging for this). Nevertheless, such issues can still occur and might be a sign that there is an underlying problem with how the training data is represented as a whole (and not how it was split).

## Calculating and retrieving statistics on a trained classifier

In addition to retrieving results from a training or prediction buffer using [`MclassGetResult`](../../Reference/class/MclassGetResult.md), you can retrieve results from a statistics classification result buffer, using [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md). Although some of the results are similar, [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md) provides more information, is specialized for a specific task, and helps you better gauge how well the trained classifier performs at prediction (information that you can use to re-train in a better way). To calculate and retrieve such results, you must:

- Allocate a statistics classification context specific to your machine learning task, by calling [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md), [`M_STAT_CNN`](../../Reference/class/MclassAlloc.md), [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), or [`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). If necessary, you can control this context using [`MclassControl`](../../Reference/class/MclassControl.md).
- Allocate a corresponding statistics classification result buffer, by calling [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).
- Perform a prediction using the trained classier and a target dataset, using [`MclassPredict`](../../Reference/class/MclassPredict.md). The target dataset can be the test dataset you quarantined when you split your data before training. This dataset should include the required ground truth entries. The results of the prediction are put in the dataset.
- Calculate statistics on the dataset, using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). This function requires the statistics classification context (in a preprocessed state) and the dataset that holds the prediction results. The results of the statistics calculation are put in the statistics classification result buffer.
- Get results from the statistics classification result buffer, using [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md).
- Free all allocated objects.

A sample of the results you can retrieve, such as the confusion matrix, F1 score, and IOU are described below.

## Advanced analysis of your training

An advanced analysis of your training refers to a more in-depth and granular investigation into what was properly and improperly classified. Such techniques can shed light on why your training is proving unsuccessful and how to fix it.

### Confusion matrix

The confusion matrix is a type of table, in a matrix format, that presents information about how many entries were correctly and incorrectly classified during training. A correctly classified entry means that its predicted class is the same as the ground truth for that class (the expected class).

For example, the following confusion matrix was calculated after the training was done using a development dataset with 3 classes (_A_, _B_, and _C_) and 500 entries (images) representing each one.

*[Image: MclassTrainingResultConfusionMatrix.png]*

The cells on the diagonal, where the classes in the ground truth row on the left intersect with the corresponding classes in the predicted column at the top indicate the number of development dataset entries that were properly classified as that class. If the classifier is perfect, the number of predicted classes would always equal the number of ground truth (expected) classes; this would result in a value of 0 in every other cell. In this example, the perfect result would be to have 500 in every intersecting cell.

*[Image: MclassTrainingResultConfusionMatrixPerfec.png]*

By examining the values in the intersecting cells, and in the other cells, you can understand the proportions between true positives, false positives, true negatives, and false negatives. The terminology for these values applies to binary classifiers that predict a target as either belonging to a class, or not belonging to a class. The values, however, can also be calculated from a confusion matrix for a classifier with more than 2 classes (for example, the 3x3 confusion matrix above).

- True positive (TP): A correct prediction that a target belongs to a specific class.
- False positive (FP): An incorrect prediction that a target belongs to a specific class.
- True negative (TN): A correct prediction that a target does not belong to a specific class.
- False negative (FN): An incorrect prediction that a target does not belong to a specific class.

For multiclass classification problems, there are more than just positive and negative classes. In these cases, you must calculate the values independently for each class by summing over the corresponding cells in the confusion matrix. The TP for a reference class is the cell at the intersection of the ground truth and the prediction for that class. The FP is the sum of the remaining cells in the prediction column for the class. The FN is the sum of the remaining cells in the ground truth row for the class. The TN is the sum of any remaining cells. In the first confusion matrix above, the cells in matrix represent the following, for each class:

*[Image: MclassTrainingResultConfusionMatrixPosNeg.png]*

Using class B as an example, summing over the respective cell gives the following values:

- TP = 400.
- FP = (6+1) = 7.
- TN = (491+3+0+499).
- FN = (10+90) = 100.

You can use these values to calculate the following metrics for your classifier. Again, for multi-class classification problems, the metrics must be calculated independently for each class.

- Accuracy = (TP+TN) / (TP+FP+TN+FN).
  - This tells you: Of all the predictions, how many are correct?
- Precision = TP / (TP+FP).
  - This tells you: Of the items predicted to belong to a certain class, how many truly belong to that class?
- Recall (also known as sensitivity) = TP / (TP+FN).
  - This tells you: Of the items that truly belong to a certain class, how many are predicted to belong to that class?
  - This is a particularly important metric when you are working with unbalanced datasets, as a classifier is less likely to predict a class if it is underrepresented in the dataset.
- F1 score = 2*(precision*recall) / (precision+recall).
  - This tells you: The overall performance of a classifier, equally weighting precision and recall.
  - Do not rely too heavily on this metric if precision and recall are not of equal importance to your application.
- IOU = TP / (TP+FN+FP).
  - This tells you: A score between 0 and 1 indicating the performance of a segmentation classifier.
  - This is only a meaningful metric for a segmentation confusion matrix, where each cell represents a number of pixels. For more information about IOU, see [Training analysis](../C52_ML_Segmentation/S05_Training_and_analysis_for_segmentation.md).

The table below shows the metrics for all classes in the confusion matrix, using the same process as shown before with class B. You can also take the average for each class to get metrics for your classifier as a whole.

| Reference class | TP | FP | TN | FN | Accuracy | Precision | Recall | F1 score | IOU (segmentation only) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A | 491 | 10 | 990 | 9 | 0.987 | 0.980 | 0.982 | 0.981 | 0.936 |
| B | 400 | 7 | 993 | 100 | 0.929 | 0.983 | 0.800 | 0.880 | 0.789 |
| C | 499 | 93 | 907 | 1 | 0.937 | 0.843 | 0.998 | 0.914 | 0.841 |
| Average | - | - | - | - | 0.951 | 0.935 | 0.927 | 0.925 | 0.855 |

Depending on your dataset balancing and your classifier's application, a weighted average in proportion to the number of entries for each class might be more useful as a metric. For example, if you have class A with only 10 entries, then an average will weigh that class equally to class B with 1000 entries. If correctly predicting class A is critical to your application then this might be okay, but otherwise it will skew your results.

Since training a classifier is an iterative process, you need a way to evaluate each iteration. There is no singular best metric to use when evaluating a classifier, and you should not trust a metric unless you know how to interpret it. Carefully select the metrics that most accurately reflect the problem you are trying to solve with your classifier.

Although counter intuitive, it can be preferable for certain applications to actually have some false classifications. Generally, if a classification is incorrect, it is either a false positive or false negative. The classifier is either wrong about what it thought was right (false positive classification) or it is wrong about what it thought was wrong (false negative classification). This information can prove invaluable, not only to help you adjust your training, but also to develop a better classifier for your specific application. For example, you might want to err on the side of false positives for a defective class, or you might want to err on the side of false negatives for a good class to maintain yields and reduce waste.

### Score distribution

The classifier returns a score for each class. The image that you are classifying is, as expected, associated with the class with the highest score. The highest score is always greater than `100/_N_`, where _N_ is the number of classes.

It is possible that false positives are more important than false negatives, and vice versa. Other than using the score to identify the best class, you can use it to take further decisions about the classified image. For example, a weak highest score, such as 51% in a binary classification problem, can result in rejecting the part to minimize the false positive rate.

Analyzing the distributions of the scores, per expected class, can facilitate setting up a threshold decision to establish what is the lowest acceptable highest score before the classification result is rejected. An example of this is shown in the following image.

*[Image: MclassTrainingResultScoreDistribution.png]*

You can construct score distribution information like this using results that you can retrieve by calling [`MclassGetResult`](../../Reference/class/MclassGetResult.md).

### Improving a deployed network with fine tuning

It might be difficult to collect enough data to fully cover all variations of the final application. The development dataset accuracy might be overestimating the real performance of the application. In such cases, the deployed system can also collect additional data of interest to add to the training dataset. The increased dataset can be further used to improve the network through fine-tuning.

A threshold on the score can be used to select the images to collect, typically the ones classified with a low confidence. These images will need to be manually verified and labeled later by an expert and added to the existing training and development datasets. If, after fine-tuning, the performance of the network is improved, the new network can be re-deployed in the field.

To improve your classifier's training, it is recommended to:

- Plan for mechanisms to efficiently collect more data from a deployed system.
- Archive the datasets to potentially further improve a network and assess the performance improvement.

## Expected trends and fluctuations

Despite fluctuating values, you should expect the following while the training is occurring:

- The loss value, which is updated after each epoch, generally decreases on average.
- The accuracy or IOU of the training and development datasets, after each epoch, generally increases on average.

These overall trends indicate that training is proceeding successfully. The amplitude of the fluctuations typically decreases with larger batch sizes. The loss value decreases very fast over the first iterations, thus a logarithmic scale is commonly used to display the loss so you can observe its long-term evolution.

After many epochs, if the error rates reach steady states and their values are far from expectations, the training process can be canceled before it ends, by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_STOP_TRAIN`](../../Reference/class/MclassControl.md).

## Examination

At the end of the training, the overall evolution of the error rate and the loss is the first area of analysis.

The error trends of both the training dataset and the development dataset should decrease over time. Given enough time (a large number of epochs), the training dataset error should eventually reach a minimum, and the classifier should perform as well as the expert in the domain.

The development dataset error is also expected to decrease to a minimum, but the error might be unacceptably greater than the training dataset error. Also, if at some point in time, the development dataset error, after reaching a minimum, starts increasing (gets worse), the classifier is probably over-fitting the data.

These are some examples of typical results you can get after training.

- The proper evolution of loss:
  *[Image: MclassTrainingResultProperLossEvolution.png]*
- Under-fitting symptoms:
  *[Image: MclassTrainingResultUnderFitting.png]*
- Over-fitting symptoms:
  *[Image: MclassTrainingResultOverFitting.png]*

The following graphs show a proper evolution of the error rate (the graph on the left) and the loss (the graph on the right) of the training dataset (in green) and the development dataset (in purple) during the execution of [`MclassTrain`](../../Reference/class/MclassTrain.md). The error rate eventually converges to 0% and the loss gets down to 0.0015872. Note, the error rate can be seen as a kind of compliment to accuracy; the lower the error, the higher you can consider the accuracy.

*[Image: MclassTrainingErrorGraph.png]*

You should abort the training process if the values level off prematurely or at an unacceptable level.

## Adjustments

Observing issues in training, such as over- or under-fitting, is known as bias and variance analysis. This is done so you can understand how your classifier was trained and figure out the adjustments you can make so you can retrain in a better way.

### Bias and variance analysis

The bias and variance analysis is typically based on observing errors related to the training dataset and development dataset. This analysis can help guide what you should adjust to improve the training when you recall [`MclassTrain`](../../Reference/class/MclassTrain.md).

The following image summarize the issues and what you can do; the subsequent subsections elaborates on this.

*[Image: MclassTrainingResultBiasAndVariance.png]*

### High training dataset error

The training dataset error measures the number of misclassified training dataset entries relative to the total number of training dataset entries. Ideally, the training dataset error converges to the Bayes error rate, which is the lowest possible error for any classifier.

Typically, human and Bayes errors are close, and it is expected that the difference between the training dataset error and the human error, the avoidable bias, can be reduced to its minimum. A high training dataset error often indicates that the classifier is under-fit.

Given no major mislabeling error in the dataset, such as a systematic labeling error, if the training dataset error is high, try taking the following bias reduction actions:

- Train longer by increasing the maximum number of epochs.
- Use a predefined CNN classifier with a larger capacity. Smaller classifiers can fail to handle complex problem.
- Try to use inputs (for example, images) with more features, such as color. A lack of features can hinder robust classifications.
- Adjust training mode related settings (hyperparameters), such as increasing the initial learning rate.

### High development dataset error

The development dataset error measures the number of misclassified development dataset entries relative to the total number of development dataset entries. Ideally, the development dataset error should converge as close as possible to the training dataset error.

A high development dataset error often indicates that the classifier is over-fit.

Given no major mislabeling error in the dataset, such as a systematic labeling error, if the development dataset error is high, try taking the following variance reduction actions:

- Collect and label more data to increase the training dataset.
  You can also add augmented data to the training dataset to regularize the training process.
- Adjust training mode related settings (hyperparameters), such as increasing the initial learning rate.

Note, investigating the development dataset errors together with the training dataset errors helps determine if the classifier was trained more than it should have been.

## Cut your losses

On occasion, despite repeatedly training and adjusting and retraining, it can seem impossible to draw any more improvements from your classifier. Some of the most difficult cases can occur when your classifier is able to split your data to a fairly good degree, but has stubbornly plateaued at a level that just isn't good enough, as certain key indicators, such as loss and error rates, are no longer improving.

In these cases, it is best to stop training, and try implementing more advanced techniques and examinations, such as analyzing the confusion matrix, scrutinizing the score distribution, and having a deployed application intended to collect data that you can then use to improve training and update the application. It is possible that such examinations indicate problems with your data, which could mean improving or restructuring your datasets.
