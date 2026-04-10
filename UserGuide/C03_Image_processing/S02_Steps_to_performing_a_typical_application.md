---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Steps_to_performing_a_typical_application
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Steps to performing a typical application"
---

# Steps to performing a typical application

We have described the broad set of operations included in the Aurora Imaging Library Image Processing module. Most applications do not require all of these operations.

Image processing pertains to more than one field and no single application program can solve the problems associated with each one of these fields. Therefore, this section describes what we believe to be a typical problem, where the solution makes use of most of the supported operations. It also outlines the steps to take to implement this solution.

## A typical application

In analyzing an image of a tissue sample, you might want to know the number of cell nuclei that are larger than a certain size, indicating an abnormality.

The following steps provide a basic methodology for performing image processing with Aurora Imaging Library:

1. Grab or load an image of a magnified tissue sample.
   *[Image: cell.png]*
2. Smooth the image to reduce noise produced during the grab.
3. Binarize the image so that the cell nuclei or particles and the background have different values: represent particles in white and the background in black. This will allow you, later, to label each particle with a unique number.
4. Perform an opening operation to remove small particles from the image.
5. Label each particle with a unique consecutive number starting with the label 1.
6. Calculate and read the extreme value of the image. This value also corresponds to the largest label. Since the image particles are labeled with consecutive unique numbers, the largest valued particle is also labeled with a number that corresponds to the number of particles in the image.

## How to encode these steps

The _MImProcessing.cpp_ example shows how to encode these steps, using an existing image of a tissue sample (_Cell.mim_). To run/view this and other examples, use Aurora Imaging Example Launcher. You can also view this example here:

> **Code example:** [MImProcessing.cpp](MImProcessing.cpp)
