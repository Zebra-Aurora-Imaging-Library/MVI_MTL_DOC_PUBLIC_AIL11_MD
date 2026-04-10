---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Steps_to_perform_a_grading_operation
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Steps to perform a grading operation"
---

# Steps to perform a grading operation

The following steps provide the basic methodology for grading a code using the Aurora Imaging Library Code module:

1. Allocate a code context ([`McodeAlloc`](../../Reference/code/McodeAlloc.md)), optionally detect the type of codes to grade ([`McodeDetect`](../../Reference/code/McodeDetect.md)), add one or more code models of the appropriate type to the code grade context ([`McodeModel`](../../Reference/code/McodeModel.md)), and train and change the control type settings so that your code(s) to grade can be read ([`McodeTrain`](../../Reference/code/McodeTrain.md)). For information on all these steps, see [](../C17_Codes/S02_Steps_to_reading_or_writing_a_code_in_an_image.md).
   A code context can contain multiple code models of 1D code types; for other code types, a code context can contain at most one code model.
   > **Note:** Note that [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support grading 4-state, Pharmacode, Postnet, and Planet 1D code types.
2. If necessary, specify the number of occurrences to grade for 1D, DotCode, or Data Matrix code models, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_NUMBER`](../../Reference/code/McodeControl.md). For code models of other types, this control type must be set to 1 ([`M_DEFAULT`](../../Reference/code/McodeControl.md)) because you can only read/grade one occurrence of these types of code models.
3. Configure your physical setup according to the specifications of your grading standard. For more details, see the section on your grading standard later in this chapter.
4. Allocate a result buffer to hold the code grade results, using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_GRADE_RESULT`](../../Reference/code/McodeAllocResult.md).
5. Grab or load an image that contains the code. If the image is large, and contains information that might be misinterpreted as a code, create a child buffer to isolate the code from the rest of the image. Alternatively, use a 2D graphics list to define a rectangular region of interest (ROI) using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). If your image contains other important information besides the code, the presearch feature can help Aurora Imaging Library locate a 2D code (except DotCode); enable the feature using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md).
6. Optionally, you can perform an [`McodeRead`](../../Reference/code/McodeRead.md) operation. The results of this read operation can be graded. This is done if you do not want to grade the results of every image you need to read. Before performing this [`McodeRead`](../../Reference/code/McodeRead.md) operation, make sure the context is configured for the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.
   Note, if you do not perform an [`McodeRead`](../../Reference/code/McodeRead.md) operation, it will be done internally as part of the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.
7. Use [`McodeGrade`](../../Reference/code/McodeGrade.md) to perform a grade operation.
8. Retrieve grading results, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md).
9. If necessary, save your code context, using [`McodeSave`](../../Reference/code/McodeSave.md) or [`McodeStream`](../../Reference/code/McodeStream.md).
10. If necessary, save a report containing most of the results from a grade operation as a text file, using [`McodeStream`](../../Reference/code/McodeStream.md).
11. Free all your allocated objects, using [`McodeFree`](../../Reference/code/McodeFree.md), unless [`M_UNIQUE_ID`](../../Reference/code/McodeAlloc.md) was specified during allocation.
