---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Bead
section: Bead_module
module_tag: bead
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / bead / Bead module"
---

# Aurora Imaging Library Bead module

A bead can be generally defined as a strip of malleable material, such as glue or lead. In many industries, such as manufacturing, beads can be used to, for example, bond objects together. In the following illustration, a bead of white glue has been applied to a mechanical part, and requires inspection.

*[Image: BeadIntroImage.png]*

Using the Aurora Imaging Library Bead module, you can inspect beads according to predefined specifications. For example, you can verify that a bead occurs at a required position, that its width falls within a certain range, that it does not have any missing sections (gaps), or that its gaps do not exceed a specified size.

Beads are inspected with a template that represents a reusable model of a valid bead. To create a template, you can provide a set of points (vertices) along a path. If the path follows a circle or segment, you can just define the shape, rather than providing a set of points. You must then train the template, typically using a training image, to establish the points (trained points) required to validate the bead. Once the template is trained, you can use it to inspect and verify beads in a target image. The following animation illustrates bead template training.

*[Image: BeadTemplateTraining]*

The Aurora Imaging Library Bead module can inspect open or closed beads of any shape. If different beads are present in the training image, the module can inspect the beads against different templates simultaneously.

You can perform many types of annotations with the Aurora Imaging Library Bead module, either when training or verifying beads, such as drawing all of the bead sections that failed due to an insufficient width. After verification, you can ascertain the pass/fail status of the bead or of each bead point, according to the specifications of the template. You can also retrieve measurements, such as width and position values.

The Aurora Imaging Library Bead module also allows you to save your templates and operation settings from a file or memory stream and later restore them.

Note that Aurora Imaging Library extracts beads from images using processes based on the Aurora Imaging Library Measurement module. Such processes use a one-dimensional analysis of differences in pixel intensities; it is typically quite fast and ideal for basic image characteristics often inherent in beads. For more information about this type of edge extraction, see [Search algorithm](../C19_Measurements/S05_Search_Algorithm.md).
