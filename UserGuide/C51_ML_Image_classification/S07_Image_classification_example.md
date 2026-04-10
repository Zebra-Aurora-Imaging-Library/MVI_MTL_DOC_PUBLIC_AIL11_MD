---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Image_classification
section: Image_classification_example
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Image_classification / Image classification example"
---

# Image classification examples

The following examples demonstrate how to perform image classification with the Aurora Imaging Library Classification module. To run/view these and other examples, use Aurora Imaging Example Launcher.

- The example _ClassCNNCompleteTrain.cpp_ demonstrates how to train a predefined (Zebra defined) CNN classifier context to classify 3 different types of fabric.
  *[Image: MclassFabricsExample.png]*
- The example _ClassPrintedChar.cpp_ demonstrates how to restore a pretrained (Zebra defined and trained) classifier context to perform a type of OCR image classification.
  *[Image: MclassOCRExample.png]*
- The example _ClassSeaFoodInspect.cpp_ demonstrates how to restore a pretrained (Zebra defined and trained) classifier context to accept or reject mussels depending on whether they have pieces of shell on them.
  *[Image: MclassMusselsExample.png]*
- The example _Mclass.cpp_ demonstrates how to restore a pretrained (Zebra defined and trained) classifier context to identify and categorize different kinds of pasta.
  *[Image: MclassPasta.png]*
  In addition to running the _Mclass.cpp_ general example from Aurora Imaging Example Launcher, you can also view it here:
  > **Code example:** [Mclass.cpp](Mclass.cpp)
