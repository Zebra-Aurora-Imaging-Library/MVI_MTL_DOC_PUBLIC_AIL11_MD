---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Steps_to_performing_a_model_search
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Steps to performing a model search"
---

# Steps to performing a model search

The following steps provide a basic methodology for using the Geometric Model Finder module:

1. Allocate your Model Finder context, using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md).
2. Define and add your model(s) to this Model Finder context, using [`MmodDefine`](../../Reference/mod/MmodDefine.md) or [`MmodDefineFromFile`](../../Reference/mod/MmodDefineFromFile.md).
3. If necessary, mask any irrelevant, inconsistent, or featureless areas of your models, using [`MmodMask`](../../Reference/mod/MmodMask.md).
4. Specify your required search settings for both the context and the individual model(s), using successive calls to [`MmodControl`](../../Reference/mod/MmodControl.md).
5. Preprocess your Model Finder context, using [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md).
6. Allocate a result buffer to hold the results of your search, using [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md).
7. Search the target for occurrences of models in your Model Finder context, using [`MmodFind`](../../Reference/mod/MmodFind.md).
8. Retrieve the required results from the result buffer, using [`MmodGetResult`](../../Reference/mod/MmodGetResult.md).
9. If necessary, save your Model Finder context, using [`MmodSave`](../../Reference/mod/MmodSave.md) or [`MmodStream`](../../Reference/mod/MmodStream.md).
10. Free all your allocated objects using [`MmodFree`](../../Reference/mod/MmodFree.md), unless [`M_UNIQUE_ID`](../../Reference/mod/MmodAlloc.md) was specified during allocation.

For information about using calibrated model source images and targets, see [Camera calibration](S17_Calibration.md).
