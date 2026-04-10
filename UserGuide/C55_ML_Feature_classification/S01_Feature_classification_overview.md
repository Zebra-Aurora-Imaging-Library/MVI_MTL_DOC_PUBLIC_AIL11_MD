---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Feature_classification
section: Feature_classification_overview
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Feature_classification / Feature classification overview"
---

# Feature classification overview

This chapter explains how to perform feature classification using machine learning with the Aurora Imaging Library Classification module.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | Note that this chapter expands on topics previously discussed in [Machine learning fundamentals](toc.md#Machine_learning_overview_and_fundamental_concepts). It is recommended to review these topics if you have not already done so. |

Feature classification allows you to classifying numerical data; it requires a tree ensemble classifier context that must be trained with a features dataset. An example of feature classification is identifying a type of blob (such as a circle, rectangle, or triangle), given a set of values (such as area and perimeter blob features). In such cases, the classifier learns to identify the blob shape, given a set of blob feature values. Although not required, what you are classifying, and the features used for training, typically traces back to an image, as shown here.

*[Image: MclassTreeIntroExample.png]*

For training purposes, you must feed the classifier numerous sets of feature values, such as convex hull, elongation, and perimeter. Each set is an entry in a features dataset and must be labeled with the class that represents the data. For example, you could have 3 classes called _Circle_, _Rectangle_, and _Triangle_. In your dataset, you would provide hundreds of entries that contain feature values (such as convex hull, elongation, and perimeter) and each of those entries would have the label of the class (shape) those features represent.

By taking in the labeled sets of feature values, the classifier learns the criteria with which to split the data, node by node, until it is able to properly identify the class that best represents that data (the feature values). Once such a classifier is trained, it should be able to properly identify any similar data that you give it.

*[Image: MclassTreeEnsembleGlobal.png]*
