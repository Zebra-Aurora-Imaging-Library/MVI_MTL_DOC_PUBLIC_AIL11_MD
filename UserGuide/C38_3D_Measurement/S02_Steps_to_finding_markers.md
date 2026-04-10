---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: Steps_to_finding_markers
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / Steps to finding markers"
---

# Steps to finding markers

The following steps provide a basic methodology for using the 3D Measurement module:

1. Allocate a profile, path, or template 3D measurement context, using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIND_MARKER_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), [`M_FIND_MARKER_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), or [`M_FIND_MARKER_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md).
   If using a template 3D measurement context, you can also allocate a fit 3D measurement context, using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIT_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), to fit a geometry after finding the markers.
2. Allocate a profile, path, or template 3D measurement result buffer, using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) with [`M_FIND_MARKER_PROFILE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), [`M_FIND_MARKER_PATH_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), or [`M_FIND_MARKER_TEMPLATE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md).
3. If using a path or template 3D measurement context, allocate a 3D geometry object, using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). Then, define and add the path or template to the path or template 3D measurement context, using [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md).
4. Specify your required settings, such as profile extraction settings, if required, and essential marker characteristics, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md).
5. Search for markers, using [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md).
6. If using a template 3D measurement context, you can fit a geometry to the found markers, using [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md).
   Alternatively, you can find the markers from the template's perpendicular profiles and perform the fit simultaneously, using [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) with a template 3D measurement context and a depth map.
7. Retrieve the required results from the 3D measurement result buffer, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md).
8. If required, copy results from the path or template 3D measurement result buffer into a 3D geometry object, container, or image buffer, using [`M3dmeasCopyResult`](../../Reference/3dmeas/M3dmeasCopyResult.md).
9. If required, use [`M3dmeasDraw`](../../Reference/3dmeas/M3dmeasDraw.md) to draw extracted features from the 3D measurement context or result buffer into an image buffer or 2D graphics list. To draw extracted features into a 3D graphics list, perform the following:
   1. Allocate a draw profile, path, or template 3D measurement analysis context, using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), [`M_DRAW_3D_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), or [`M_DRAW_3D_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), to hold the settings for the draw. Note that you can skip this step and use a default context instead.
   2. Specify the draw operations and options for the draw, using [`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md).
   3. Draw the features, using [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).
10. If required, save your 3D measurement context, using [`M3dmeasSave`](../../Reference/3dmeas/M3dmeasSave.md) or [`M3dmeasStream`](../../Reference/3dmeas/M3dmeasStream.md).
11. Free all your allocated objects, using [`M3dmeasFree`](../../Reference/3dmeas/M3dmeasFree.md), unless [`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAlloc.md) was specified during allocation.
