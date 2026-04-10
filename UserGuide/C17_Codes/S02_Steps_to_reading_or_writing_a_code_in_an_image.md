---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Steps_to_reading_or_writing_a_code_in_an_image
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Steps to reading or writing a code in an image"
---

# Steps to reading, grading, or writing a code in an image

The following steps provide a basic methodology for using the Aurora Imaging Library Code module:

1. Optionally, detect the code types of 1D code occurrences in your image using [`McodeDetect`](../../Reference/code/McodeDetect.md). This returns the position of the code occurrences and their code type and encoding scheme. For information, see [Automatically detecting the code type](S07_Automatically_detecting_the_code_type.md).
2. Allocate a code context, using [`McodeAlloc`](../../Reference/code/McodeAlloc.md). A code context is an Aurora Imaging Library object that stores the code models and code operation settings.
   Optionally, specify the initial configuration of the code context as either [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md) or [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), depending on whether robustness or speed is more important, respectively.
3. Add one or more code models to the code context, using [`McodeModel`](../../Reference/code/McodeModel.md), depending on the operation you need to perform. A code model contains control settings to read, grade, train, or write a particular code type. Specify the code type when adding the code model. You can use the combination value [`M_SUPPORTED`](../../Reference/code/McodeModel.md) to determine whether a code model of the specified code type can be added to the code context, given the code type of the code models that already exist in the context.
   A code context can contain multiple code models of 1D code types (excluding GS1 databar, Planet, Postnet, and 4-state); for other code types, a code context can contain at most one code model.
4. If necessary, change the control settings of the code context or the code models, using [`McodeControl`](../../Reference/code/McodeControl.md) (for example, change the encoding scheme for your code type using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md), or change the error correction scheme for your code type using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md)).
   You can use [`McodeTrain`](../../Reference/code/McodeTrain.md)to establish the optimal control type settings for a read or grade operation given a sample set of your images. Then, you can configure the code context and its code models with these results using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESET_FROM_TRAINED_RESULTS`](../../Reference/code/McodeControl.md). For information, see [Training read and grade operation settings](S08_Training_read_and_grade_operation_settings.md).
5. If necessary, specify the number of occurrences to read or grade for 1D code models (excluding GS1 databar, Planet, Postnet, and 4-state), DotCode code models, or Data Matrix code models, using [`McodeControl`](../../Reference/code/McodeControl.md) with[`M_NUMBER`](../../Reference/code/McodeControl.md). For code models of other types, this control type must be set to 1 ([`M_DEFAULT`](../../Reference/code/McodeControl.md)) because you can only read/grade one occurrence of these types of code models.
6. If reading, grading, detecting, training or writing a code, allocate a result buffer to hold the code results, using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md).
7. If reading or grading a code, grab or load an image that contains the code. If the image is large, and contains information which might be misinterpreted as a code, create a child buffer to isolate the code from the rest of the image. Alternatively, use a 2D graphics list to define a rectangular region of interest (ROI) using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). If your image contains other important information besides the code, the presearch feature can help Aurora Imaging Library locate a 2D code; enable the feature using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md).
   If writing a code, allocate the image buffer in which to generate the code.
8. To perform a read operation, use [`McodeRead`](../../Reference/code/McodeRead.md). To perform a grade operation, use [`McodeGrade`](../../Reference/code/McodeGrade.md). To perform a write operation, use [`McodeWrite`](../../Reference/code/McodeWrite.md).
9. Retrieve reading, grading, detecting, training, or writing results, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md).
10. If necessary, draw the results using [`McodeDraw`](../../Reference/code/McodeDraw.md).
11. If necessary, save your code context, using [`McodeSave`](../../Reference/code/McodeSave.md) or [`McodeStream`](../../Reference/code/McodeStream.md).
12. If necessary, save a report containing most of the results from a grade operation as a flat text file, using [`McodeStream`](../../Reference/code/McodeStream.md).
13. Free all allocated objects, using [`McodeFree`](../../Reference/code/McodeFree.md), unless [`M_UNIQUE_ID`](../../Reference/code/McodeAlloc.md) was specified during allocation.

> **Note:** Note that you must take into consideration any particularities of the chosen code type. More detailed and code type-dependent information is described in subsequent sections of this chapter, as well as in the _Aurora Imaging Library Reference_.

Once created, an Aurora Imaging Library code context can be saved and restored as needed. Restoring this information using [`McodeRestore`](../../Reference/code/McodeRestore.md) or [`McodeStream`](../../Reference/code/McodeStream.md), rather than creating the Aurora Imaging Library code context from scratch saves time, especially if the restored code context requires no further modifications.
