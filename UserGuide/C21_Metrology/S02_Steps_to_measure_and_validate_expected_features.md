---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Metrology
section: Steps_to_measure_and_validate_expected_features
module_tag: met
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / metrology / Steps to measure and validate expected features"
---

# Steps to measure and validate expected features

The following steps provide a basic methodology for using the Aurora Imaging Library Metrology module:

1. Allocate a metrology context to store the features and tolerances of your metrology template and to store global processing settings, using [`MmetAlloc`](../../Reference/met/MmetAlloc.md). When you allocate a metrology context, the global reference frame is automatically created.
2. Allocate a metrology result buffer to hold the results of your calculations, using [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md).
3. Define the features of your metrology template. For each feature:
   1. Add the feature to the template, using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md). Typically, you should add physically measured features before constructed features.
   2. For a physically measured feature, you must set the feature's metrology region in the template, using [`MmetSetRegion`](../../Reference/met/MmetSetRegion.md). The features with which to build constructed features can also have a delimited metrology region.
   3. If necessary, adjust feature settings using successive calls to [`MmetControl`](../../Reference/met/MmetControl.md).
4. Define the geometric tolerances of your metrology template. For each tolerance:
   1. Add the tolerance to the template, using [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md).
   2. If necessary, adjust tolerance settings using successive calls to [`MmetControl`](../../Reference/met/MmetControl.md).
5. If necessary, specify your required global processing settings, using successive calls to [`MmetControl`](../../Reference/met/MmetControl.md).
6. If necessary, reposition features (typically, a reference frame) in the template, depending on the location of the object in the target image, using [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md).
7. Calculate features and validate geometric tolerances for the object in the target image, using [`MmetCalculate`](../../Reference/met/MmetCalculate.md).
8. Retrieve the required results from the result buffer, using [`MmetGetResult`](../../Reference/met/MmetGetResult.md).
9. If necessary, draw specific metrology result features and geometric tolerances in an image buffer, using [`MmetDraw`](../../Reference/met/MmetDraw.md).
10. If necessary, save your metrology context, using [`MmetSave`](../../Reference/met/MmetSave.md) or [`MmetStream`](../../Reference/met/MmetStream.md).
11. Free your metrology context and result buffer, using [`MmetFree`](../../Reference/met/MmetFree.md), unless [`M_UNIQUE_ID`](../../Reference/met/MmetAlloc.md) was specified during allocation.
