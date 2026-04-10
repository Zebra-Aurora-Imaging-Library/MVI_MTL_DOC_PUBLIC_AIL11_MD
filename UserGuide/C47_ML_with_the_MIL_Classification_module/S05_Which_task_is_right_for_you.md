---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_with_the_MIL_Classification_module
section: Which_task_is_right_for_you
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Classification_and_classifier_overview / Which task is right for you"
---

# Which task is right for you

The problem you want to solve will indicate the machine learning task you should perform. Once you know the task, you will know the type of classifier you should use.

|  |
||
| Classify entire images (for example, image identification or boolean validation) | ✔ |
| Classify image regions on a pixel level | ✔ |
| Detect instances of regions (objects) in an image | ✔ |
| Detect anomalous images or anomalous pixels in images | ✔ |
| Classify numerical data (typically representing image features) | ✔ |
| Predict with an imported machine learning model | ✔ |
| ¹ The Classification module lets you import a trained ONNX model into an ONNX classifier context and use it with [`MclassPredict`](../../Reference/class/MclassPredict.md) for your machine learning task. For more information, see [ONNX](../C50_ML_Prediction/S05_ONNX.md). |

## Image classification (predefined CNN classifier)

Image classification refers to classifying an entire image; it typically requires a predefined CNN classifier context that was defined by Zebra (an ICNet), and that must be trained with an images dataset context. An example of image classification is identifying different types of images that have complex and similar features, such as different types of fabrics or different types of pasta.

*[Image: MclassFabricsOverviewExample.png]*

You can use image classification to predict whether an entire image is one of several classes (such as _Pasta1_, _Pasta2_, or _Pasta3_), or to perform a boolean type of prediction; that is, predict if an image belongs to a bad or invalid type of class (such as _NotEnoughPasta_) or if it belongs to a good or valid type of class (such as _EnoughPasta_).

The classifier gives each image a score, per defined class, to indicate the degree to which that class represents the image. The class with the highest score is the one that best represents the image. The following example shows an image and its class results. The best class is highlighted.

*[Image: MclassPastaResults.png]*

## Segmentation (predefined segmentation classifier)

Segmentation refers to a somewhat coarse pixel level classification of regions in an image; it typically requires a predefined segmentation classifier context that was defined by Zebra (a CSNet), and that must be trained with an images dataset context. Training such a classifier can let you predict the class of similar regions within similar images on a pixel level. An example of segmentation is identifying the pixels representing the presence of defects that are difficult to distinguish from the surfaces on which they occur, such as scratches or pits on steel, as shown here.

*[Image: MclassSegExampleCSNET.png]*

Segmentation is often performed when the classification is based on a small area (for example, a defect like a small scratch), relative to the size of the entire scene, and you cannot otherwise delimit the area (for example, with fixturing). In such cases, a local view of the data can improve robustness. That is, rather than being interested in the image as a whole (for example, establishing whether an image is good or bad), you are interested in the pixel data of regions that can potentially occur in that image (for example, identifying the pits or scratches that make the image invalid).

Segmentation can also prove useful for other purposes, in particular because you can use the class of each pixel to perform post-processing operations, such as carrying out a type of blob extraction. For example, in the steel defect example, you can see the scratch and pit as two types of blobs extracted from the steel surface background (you can also use this information to calculate a position).

Ultimately, the classifier gives each pixel a score, per defined class, to indicate the degree to which that class represents the pixel. The defined class with the highest score is the one that best represents the pixel.

> **Note:** With segmentation, the background is itself a class, and it is always processed. If, for example, none of the defect classes are found, the predicted class will be the background.

If you want to classify image regions and do not need the class of each pixel within, you might consider performing object detection. For more information, see [Object detection versus segmentation](S05_Which_task_is_right_for_you.md).

## Object detection (predefined object detection classifier)

Object detection refers to classifying instances of objects (regions) in an image; it typically requires a predefined object detection classifier context that was defined by Zebra (an ODNet), and that must be trained with an images dataset context. Training such a classifier lets you predict the class of similar objects (regions) within similar images. An example of object detection is finding instances of defects that are difficult to distinguish from the surfaces on which they occur, such as knots on wood, as shown here.

*[Image: MclassObjectDetectionExample.png]*

Once an instance of a region (object) is found, object detection identifies the class to which the entire region belongs. This distinguishes it from segmentation, which identifies the class of every pixel in the region.

### Object detection versus segmentation

Both object detection and segmentation can be seen as classifying regions in an image. Also, use cases for object detection, such as finding various type of defects, are often like those for segmentation.

In general, object detection is preferred when you want to simplify locating instances of objects within an image (that is, you want to identify regions as a whole), while segmentation is performed when you require the high level of precision that it provides (that is, you want to identify the region's pixels). More specifically:

| Object detection | Segmentation |
| --- | --- |
| The class and location of each instance is returned. Instances are found (predicted) when a rectangular region in the target represents one of the defined classes. For example, the rectangular regions enclosing the _SmallKnot_ and _LargeKnot_ classes. Each rectangular region found represents one class (you have no information about the pixels within the region). | The class of each pixel is returned. Finding the class of every pixel can prove critical, if this high level of detail is needed. If not needed, it can prove unnecessarily challenging to identify multiple occurrences of a class. For example, if 2 large knots are side by side and some pixels overlap, object detection can find these as 2 instances of large knots, while segmentation will identify all the pixels as large knots and differentiating them might prove more difficult (there is no notion of instances or occurrences in segmentation). |
| The location of instances returned by object detection can prove useful; for example, you can ignore results that do not occur in a predefined part of the target. | The location of segmentation results is not inherently provided (you would have to create some post processing operations to get their general location). |
| Regions are defined by rectangles; this can be seen as a quick way to label your data. | Regions can be defined by brushing over the required areas; this can be more time consuming than enclosing regions with rectangles. |

## Anomaly detection (predefined anomaly detection classifier)

Anomaly detection refers to finding anomalous images; it requires a predefined anomaly detection classifier context that was defined by Zebra (an ADNet), and that must be trained with an images dataset context that contains only good (non-anomalous images). This can be particularly useful when valid images are plentiful and easy to acquire, while invalid images are rare and can have a variety of abnormalities that make them invalid, such as scratches, chips, dents, and foreign objects.

*[Image: MclassAnomalyDetectionOverview.png]*

In addition to identifying anomalous images, anomaly detection can also localize the anomalies by identifying their coarse pixel location.

*[Image: MclassAnomalyDetectionOverviewLocalization.png]*

Since only valid (non-anomlaous) images are used in training, there is no labeling required for your datasets, which can significantly reduce development time and costs.

Although effective as a stand-alone machine learning task, anomaly detection can also be useful as a preliminary training step for other machine learning tasks, such as image classification, segmentation, and object detection. For example, if you ultimately want to identify a variety of different defects in an image, you can use anomaly detection to find all defective images first. This lets you know which images should be sent for manual labeling so the specific defective class description can be assigned, and which images do not need any manual labeling since you have already determined they are defect free. Reliably reducing the amount of image data that requires manual labeling can often cut costs and development time significantly.

## Feature classification (tree ensemble classifier)

Feature classification refers to classifying numerical data; it requires a tree ensemble classifier context that must be trained with a features dataset. In the following example, the classifier identifies the shapes in the image after extracting numerical data from each shape, such as rectangularity, compactness, and elongation. Numerical data for feature classification does not have to originate from an image.

*[Image: MclassFeatureClassificationPredResultsShapes.png]*

For training purposes, you must feed the classifier numerous sets of feature values. Each set is an entry in a features dataset and must be labeled with the class that represents the data. For example, you could have 3 classes called _Disk_, _Square_, and _Cross_. In your dataset, you would provide hundreds of entries that contain feature values (such as rectangularity, compactness, and elongation) and each of those entries would have the label of the class (shape) those features represent.

By taking in the labeled sets of feature values, the classifier learns the criteria with which to split the data, node by node, until it is able to properly identify the class that best represents that data (the feature values). Once such a classifier is trained, it should be able to properly identify any similar data that you give it.

*[Image: MclassTreeEnsembleGlobal.png]*
