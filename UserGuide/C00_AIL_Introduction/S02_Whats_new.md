---
doctype: UserGuide
part: "Getting started"
chapter: AIL_Introduction
section: Whats_new
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / AIL_Introduction / Whats new"
---

# What's new in Aurora Imaging Library 11

Aurora Imaging Library 11 includes new functionalities, performance optimizations, and general improvements, such as:

- The ability to work with side-by-side installations of version 10.7 and version 11 on the same computer.
- Introduction of Aurora Imaging Object Library, an object-oriented API available in C++, C#, and Python. This first phase includes a basic subset of modules, with more modules available in future releases.
- Introducing multi-band image buffer support, which is particularly useful for deep learning classification and segmentation.
  *[Image: whatsnew_multiband_buffers.png]*
- An overall product rebranding consistent with Zebra machine vision products.
- A code conversion utility, sourceconverter, that upgrades your code to reflect the rebranding and automates the transition from version 10.7 to version 11. See [Converting your source code](../C02_Building_an_application/S03_Going_from_AIL_10.7_to_AIL_11.md).
- New examples and many more additions and improvements to discover.

Within specific modules, added and improved functionality includes:

- Magm (Advanced Geometric Matcher):
  - Support for model extraction from a rotated image region.
  - For single-definition models:
    - Pyramid calculation support to decrease search time.
    - Polarity support to help filter unwanted occurrences.
      *[Image: whatsnew_agm.png]*
- Mcal (calibration):
  - 3D robotics calibration with the Zhang model and specifying a pose when copying results.
- Mdlocr (Deep Learning OCR):
  - Enhanced accuracy with our new fine-tuning feature, allowing you to adjust the reading context for improved results.
  - Introducing a new model that balances accuracy and speed, supports applications with highly variable character heights, and also offers fine-tuning.
    *[Image: whatsnew_finetuning.png]*
- Mim (image processing):
  - Unwarping an image along a path, creating a Gabor filter and its Hilbert transform, and performing morphological transformations using a circular shape.
    *[Image: whatsnew_mimunwarpalongpath.png]*
- M3ddisp and M3dgra (3D display and 3D graphics):
  - Picking 3D graphics in a rectangular region, picking a face from a 3D box, interactive editing of plane graphics, and using a 2D graphics list as a heads-up display (HUD) in a 3D display.
- M3dmod (3D model finder):
  - An additional new mode for featureless planar object detection, support for determining optimum grip regions, and weighted model point assignments for identifying prime occurrences.
    *[Image: whatsnew_3dmod_planar_object.png]*
- M3dmeas (3D measurement):
  - Support for ridge transitions, pair marking of transitions, and projection angle.

[Aurora Imaging Library updates](../../ReleaseNotes/index.md), such as new or modified systems, drivers, features, tools, and add-ons, are available for download. Be sure to check for these, as they can expand the library's capabilities, and improve the development and execution of your applications.

> **Note:** The Aurora Imaging Library release notes, accessible from Aurora Imaging Control Center and/or the Zebra Aurora Imaging Library product support webpages (for example, [zebra.com/ail-info](https://zebra.com/ail-info)), can provide additional documentation, such as new or modified features, systems, and products.
