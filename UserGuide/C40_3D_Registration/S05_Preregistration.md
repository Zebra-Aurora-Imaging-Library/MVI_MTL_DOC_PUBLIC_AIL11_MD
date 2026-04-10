---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: Preregistration
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / Preregistration"
---

# Preregistration

The registration operation begins with a preregistration iteration, iteration 0. This transforms a point cloud so that the distance between its points, and the reference point cloud's points, is small enough for the ICP algorithm to start.

There are two possible modes for performing the preregistration iteration, specified using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md).

## Preregistration with a transformation matrix

If [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dreg/M3dregControl.md), the specified preregistration transformation matrix that is stored in the pairwise 3D registration context will be applied to the point cloud during the preregistration iteration.

By default, the pairwise registration context does not contain a preregistration transformation matrix; you need to specify one using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md).

There are two common ways to specify the transformation matrix for the preregistration:

- Copy the results from a previous call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md).
- Manually define the values of the transformation matrix. To do so, allocate a transformation matrix object using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md), and then set the matrix's values using [`M3dgeoMatrixPut`](../../Reference/3dgeo/M3dgeoMatrixPut.md) or [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md). Then, copy the transformation matrix object into the pairwise 3D registration context using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md).

## Preregistration with centroids

If [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md) is set to [`M_CENTROID`](../../Reference/3dreg/M3dregControl.md), the point cloud is translated so that the mean position of all its points, called its centroid, overlaps the reference point cloud's centroid.

*[Image: 3dreg_preregister_centroid.png]*

By default, this preregistration transformation only consists of a translation; there is no rotation. However, if a preregistration transformation matrix is supplied using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md), and [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md) is set to [`M_CENTROID`](../../Reference/3dreg/M3dregControl.md), the point cloud will be rotated according to the specified preregistration transformation matrix and translated so that the centroids share the same coordinates. The translation component of the specified preregistration matrix is ignored.
