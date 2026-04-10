---
doctype: UserGuide
part: "Miscellaneous"
chapter: About_the_Help
section: User_guide
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / About_the_Help / User guide"
---

# Aurora Imaging Library User Guide and examples

The User Guide is structured to allow you to start processing your images with as little information overload as possible. After testing the installation, as well as verifying that the required Aurora Imaging Library header file and libraries are installed, it is expected that you read [Part 1: Getting started](toc.md#Getting_started).

[Part 2: 2D processing and analysis](toc.md#2D_processing_and_analysis) and [Part 4: 3D processing and analysis](toc.md#3D_processing_and_analysis) deal with enhancement, transformation and analysis operations. It is expected that you read only the chapters that you need; for the chapters on image processing, you can read only the required sections.

[Part 3: 2D related information](toc.md#2D_related_information) and [Part 5: 3D related information](toc.md#3D_related_information) go into more detail about setting up and managing your images, displays and digitizers, as well as describing other topics that might be useful but not critical to getting started.

Besides Chapters 1 and 2, this user guide has been written for using Aurora Imaging Library under Microsoft Windows 11 and select versions of Windows 10 (see [Requirements to run Aurora Imaging Library](../C02_Building_an_application/S01_Requirements.md)). Using Aurora Imaging Library under Linux is very similar; refer to [Using Aurora Imaging Library under Linux](../C61_Using_AIL_under_Linux/ChapterInformation.md) for a list of the differences.

In general, Host refers to the CPU in one's computer, while system refers to a set of hardware components capable of grabbing, storing, processing, and/or displaying images.

Descriptions of the individual functions are found in the [Aurora Imaging Library Reference](../../Reference/index.md).

## Examples

Throughout this manual, some examples have been provided to simplify concepts and get you started quickly. To view, run and edit any example, use Aurora Imaging Example Launcher, accessible from Aurora Imaging Control Center.

To view a list of general examples, [click here](../../Examples/index.md).

> **Note:** Note that some examples will display images in grayscale even if the images are grabbed with a color camera. This is due to the fact that those examples perform statistical and/or analysis operations that do not support color processing.

Aurora Imaging Library's examples offer many advantages:

- The [MappStart](../../Examples/MappStart.md) example is very simple, but it allows you to quickly test Aurora Imaging Library's installation and become familiar with the basics behind running an Aurora Imaging Library application.
- The examples use your default settings which have been specified at installation; therefore, very little time must be spent configuring your programming environment. However, if you want to change default settings, or are experiencing problems running the examples, you can use Aurora Imaging Configurator to change the defaults (click on the **Default Values** tab in the **General** folder). You can also access the same settings from Aurora Imaging Example Launcher. Click on the **Default system** button, which will take you directly to the **Default Values** tab of Aurora Imaging Configurator.

Note that some systems cannot run some of the examples because they do not have the hardware capability or enough memory. You should skip these examples or modify them.
