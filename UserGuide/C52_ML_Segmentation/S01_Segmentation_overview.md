---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Segmentation
section: Segmentation_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Coarse_segmentation / Segmentation overview"
---

# Segmentation overview

This chapter explains how to perform segmentation using machine learning with the Aurora Imaging Library Classification module.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | Note that this chapter expands on topics previously discussed in [Machine learning fundamentals](toc.md#Machine_learning_overview_and_fundamental_concepts). It is recommended to review these topics if you have not already done so. |

Segmentation allows you to a classify regions within an image at a somewhat coarse pixel level; it typically requires a predefined segmentation classifier context that was defined by Zebra (a CSNet), and that must be trained with an images dataset context. To train a classifier, you must supply it with many images that are representative of the real-world problem the classifier will solve, along with a label identifying the class for each relevant region in the images. The classifier will learn from these training images how to differentiate the various classes, and let you predict the class of similar regions within similar images. An example of segmentation is detecting the presence of defects that are difficult to distinguish from the surfaces on which they occur, such as minor scratches or pits on steel, as shown here.

*[Image: MclassSegExampleCSNET.png]*

Segmentation is often performed when the classification is based on a small area (for example, a defect like a small scratch), relative to the size of the entire scene, and you cannot otherwise delimit the area (for example, with fixturing). In such cases, using a local view of the data can improve the classification's robustness. Simply put, rather than being interested in the image as a whole, you are interesting in regions that can potentially occur in that image.

You can use segmentation to predict the class to which regions within an entire image belong (multiple predicted class results per image). For example, rather than classifying the whole image as good or bad, you want to predict the class to which the defects within the image belong (such as _Circle_, _Rectangle_, or _Triangle_).

*[Image: MclassSegGlobal.png]*

Segmentation can also prove useful for other reasons, such as, if the approximate locations of the classes (for example, the defects) are important for the application.

> **Note:** If you are only interested in locating regions, and you do not require the pixel information within, you might want to consider performing object detection. For more information, see [Object detection versus segmentation](../C47_ML_with_the_MIL_Classification_module/S05_Which_task_is_right_for_you.md).
