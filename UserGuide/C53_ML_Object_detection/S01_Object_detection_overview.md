---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Object_detection
section: Object_detection_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Object_detection / Object detection overview"
---

# Object detection overview

This chapter explains how to perform object detection using machine learning with the Aurora Imaging Library Classification module.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | Note that this chapter expands on topics previously discussed in [Machine learning fundamentals](toc.md#Machine_learning_overview_and_fundamental_concepts). It is recommended to review these topics if you have not already done so. |

Object detection lets you to classify regions (objects) within an image. It typically requires a predefined object detection classifier context that was defined by Zebra (an ODNet), and that must be trained with an images dataset context. To train an object detection classifier, you must give it many images that represent the real-world problem it must solve, along with a label identifying the class of each rectangular region (object) in the images. The classifier learns from these training images how to detect and differentiate the classes. Once the classifier is trained, it can predict the class of similar regions within similar images. An example of object detection is finding instances of small and large knots on wood.

*[Image: MclassObjectDetectionOverview.png]*

Object detection is often performed when you want to identify regions in an image that you can bound within a rectangle (for example, rectangular regions enclosing potential defects like small or large knots on wood). With object detection:

- The class and location of each instance is returned (each instance found represents one class).
- Instances are found (predicted) when a rectangular region in the target represents one of the defined classes (for example, the rectangular regions enclosing the _SmallKnot_ and _LargeKnot_ classes).
- The location of instances returned by object detection can prove useful; for example, you can ignore results that do not occur in a predefined part of the target.
- Regions are defined by rectangles; this can be seen as a quick way to label your data.

The use cases for object detection, such as finding various type of defects, are often like those for segmentation. In general, object detection is preferred when you want to simplify locating instances of objects within an image, and you do not need the high level of precision that segmentation performs. If you require the kind of precision that a pixel level segmentation provides, see [Segmentation](../C52_ML_Segmentation/ChapterInformation.md). For more information about the differences between object detection and segmentation, see [Object detection versus segmentation](../C47_ML_with_the_MIL_Classification_module/S05_Which_task_is_right_for_you.md).
