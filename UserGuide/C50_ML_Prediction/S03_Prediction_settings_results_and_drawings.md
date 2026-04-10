---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Prediction
section: Prediction_settings_results_and_drawings
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Predicting_and_advanced_techniques / Prediction settings results and drawings"
---

# Prediction settings, results, and drawings

In general, you call[`MclassPredict`](../../Reference/class/MclassPredict.md) with a trained classifier to make predictions on data that the classifier has never seen, though it has been trained on similar data. You can also call [`MclassPredict`](../../Reference/class/MclassPredict.md) with other intentions, to make predictions on unlabeled data, or to test your classifier and then use the results to make further training modifications. Keep in mind that your prediction results might not be as expected. For example, you might think that your classifier is fully trained, though when you use it to predict on a test dataset, you are surprised to see a lower accuracy than you thought. This is ultimately part of the training process, since you now have the opportunity to adjust your training settings, or even your dataset, to help ensure that predictions are actually what you expect them to be. For more information, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

## Understanding prediction

A trained classifier can only predict what it knows, and it only knows what it was trained on. For example, if your classifier was trained with a dataset that has numerous representative images, such as images with variances in location and blurriness, predicting with that classifier on such images will yield excellent results. Even if images seem quite poor, as long you labeled and trained with them properly, the prediction will succeed. On the other hand, images that have what might be misconstrued as inconsequential differences, such as rotation or scale, can be problematic to predict, if those differences where not learned by the classifier at training time.

*[Image: MclassRotationFail.png]*

If a classifier was never trained on rotated images, it cannot predict these cases, no matter how simple they might seem. It would be like a human trying to predict the words of a language he has never learned. Often, the best way to address such issues is to add the images with the new variances to the dataset, retrain, and then perform the prediction again with the newly trained classifier.

*[Image: MclassRotationPass.png]*

To account for discrepancies in rotation, you can also use [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) to add augmented images with various rotation to your training data. For more information, see [Augmentation](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

## Timeout and stop

To prevent the classification process from taking too long, you can set a maximum calculation time for [`MclassPredict`](../../Reference/class/MclassPredict.md), by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TIMEOUT`](../../Reference/class/MclassControl.md). You can also stop the current execution of [`MclassPredict`](../../Reference/class/MclassPredict.md) (from another thread of higher priority), by specifying[`M_STOP_PREDICT`](../../Reference/class/MclassControl.md).

## Results

The results you can retrieve depend on the machine learning task you are doing. For certain tasks, some of the most important prediction results to retrieve are the best predicted class ([`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResult.md)) and its score ([`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResult.md)). In these cases, the score is a measure, in percentage, of how well a class represents the target. Therefore, the class with the highest score is the class to which the target belongs.

*[Image: MclassContextNetworkClassesResult.png]*

You can also retrieve the status result of the prediction operation ([`M_STATUS`](../../Reference/class/MclassGetResult.md)), allowing you to determine if [`MclassPredict`](../../Reference/class/MclassPredict.md) is currently predicting, has completed successfully, or was terminated because of a timeout limit or a memory issue. To clear the prediction results, call [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_RESET`](../../Reference/class/MclassControl.md) and the identifier of the prediction result buffer.

It is a good practice to predict on your source labeled data to check for mislabeled data. If your classifier predicts a class with a high score, but is incorrect (not the ground truth), then it is worth manually checking that entry and confirming that you have correctly labeled the data.

Note, if you have a testing dataset, you should predict with it, using [`MclassPredict`](../../Reference/class/MclassPredict.md). As previously discussed, predicting with a testing dataset can serve as a quarantined final check for your trained classifier. If the results are what you expect (they should be approximately the same as your training results), you can continue with prediction using your trained classifier. If the results are not what you expect, it is a sign that you should re-train. To get additional information about how to improve the subsequent training of a trained classifier, you can calculate statistics. For more information, see [Calculating and retrieving statistics on a trained classifier](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

### Drawing

You can draw and visually identify various features from a classifier context, dataset context, or a prediction result buffer into an image buffer or LUT buffer using [`MclassDraw`](../../Reference/class/MclassDraw.md). You can also draw from a specific entry in an images dataset using [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md).
