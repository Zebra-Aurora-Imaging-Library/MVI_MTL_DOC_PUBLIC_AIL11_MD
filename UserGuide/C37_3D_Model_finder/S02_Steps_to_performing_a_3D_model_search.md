---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Steps_to_performing_a_3D_model_search
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Steps to performing a 3D model search"
---

# Steps to performing a 3D model search

The following steps provide a basic methodology for using the 3D Model Finder module:

1. Allocate a find 3D model finder context, using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md). Optionally, if you have previously saved a 3D model finder context to a file, you can instead restore it, using [`M3dmodRestore`](../../Reference/3dmod/M3dmodRestore.md).
2. Allocate a 3D model finder result buffer, using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md).
3. Define and add a model to the 3D model finder context, using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md).
4. Specify your required search settings for both the 3D model finder context and the model, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md).
5. Preprocess the 3D model finder context, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md).
6. Search a point cloud for occurrences of the model in your 3D model finder context, using [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md).
7. Retrieve the required results from the 3D model finder result buffer, using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md).
8. If required, copy a group of results from your 3D model finder result buffer into a 3D geometry object, an image buffer, or a transformation matrix object, using [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md). For example, you can define the geometry of a 3D geometry object from a found occurrence, create image masks, or copy fixturing coefficients into a transformation matrix object.
9. To draw the results of a found 3D model occurrence into a 3D graphics list, perform the following:
   1. Allocate a draw 3D model finder context to hold the settings for the draw, using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with either[`M_DRAW_3D_GEOMETRIC_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md)(to draw occurrences of geometric models) or[`M_DRAW_3D_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md)(to draw occurrences of surface models). Note that you can skip this step and use a default context instead.
   2. Specify the draw operations and options for the draw, using [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md).
   3. Draw the result of a found occurrence, using [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).
10. If required, save your 3D model finder context, using [`M3dmodSave`](../../Reference/3dmod/M3dmodSave.md) or [`M3dmodStream`](../../Reference/3dmod/M3dmodStream.md).
11. Free all your allocated objects, using [`M3dmodFree`](../../Reference/3dmod/M3dmodFree.md), unless [`M_UNIQUE_ID`](../../Reference/3dmod/M3dmodAlloc.md) was specified during allocation.
