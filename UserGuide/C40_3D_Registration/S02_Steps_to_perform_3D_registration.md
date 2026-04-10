---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: Steps_to_perform_3D_registration
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / Steps to perform 3D registration"
---

# Steps to perform the pairwise 3D registration operation

To perform pairwise 3D registration, you must:

1. Allocate a pairwise 3D registration context using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_PAIRWISE_REGISTRATION_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md).
2. Allocate a pairwise 3D registration result buffer using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md) with [`M_PAIRWISE_REGISTRATION_RESULT`](../../Reference/3dreg/M3dregAllocResult.md). The registration results will be written to this result buffer.
3. Specify the number of registration elements in your pairwise 3D registration context using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/3dreg/M3dregControl.md). Each registration element controls how to perform the registration of its associated point cloud container. The number of registration elements should correspond to the number of containers with 3D-processable point clouds that you intend to register.
4. Define the mode of preregistration using[`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md). For the ICP algorithm to begin, a point cloud must initially be positioned relatively close to its reference point cloud; otherwise, the points will be too far apart to be paired. The different preregistration modes offer different ways to specify a rough location for your point cloud.
5. If necessary, adjust the default settings and stop conditions of the iterative pairwise 3D registration operation, using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md). For example, you can specify the registration operation to stop after a certain number of iterations. You can also specify to subsample all point clouds prior to registration.
6. If necessary, specify to save pairs information at each iteration, using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_SAVE_PAIRS_INFO`](../../Reference/3dreg/M3dregControl.md) set to [`M_TRUE`](../../Reference/3dreg/M3dregControl.md). You must save pairs information if you intend to draw results, or copy results into an image buffer or container. Saving pairs information is not necessary if you only need to copy results into a transformation matrix.
7. Call [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md), specifying the previously allocated pairwise 3D registration context and pairwise 3D registration result buffer in which to write the registration results.
8. Retrieve the required results from the pairwise 3D registration result buffer, using [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) and [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md). You can retrieve the transformation matrix that aligns any point cloud in the pairwise 3D registration context to any other point cloud from the same pairwise 3D registration context.
9. If necessary, use [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) to create a distance image, overlap mask, or pair index image, which contain data associated with (respectively) the distance between points, overlapping (paired) points, or indices of paired reference and target points. You can also use [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) to copy the subsampled version of the reference or target point cloud into a container. Note that to copy into an image buffer or container, you must have set [`M_SAVE_PAIRS_INFO`](../../Reference/3dreg/M3dregControl.md) to [`M_TRUE`](../../Reference/3dreg/M3dregControl.md) prior to the registration operation.
10. If necessary, use the results to merge the points clouds into one new point cloud using [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md). Before merging, [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) can either transform the point clouds such that their working coordinate systems are aligned to the global coordinate system or aligned (fixtured) to a specified point cloud's working coordinate system.
11. To draw a result of the 3D registration operation into a 3D graphics list, perform the following:
   1. Ensure that you have set [`M_SAVE_PAIRS_INFO`](../../Reference/3dreg/M3dregControl.md) to [`M_TRUE`](../../Reference/3dreg/M3dregControl.md) prior to the registration operation.
   2. Allocate a draw 3D registration context to hold the settings for the draw, using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md). Note that you can skip this step and use a default context instead.
   3. Specify the draw operations and options for the draw, using [`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md).
   4. Draw the points and/or other features of the required result, using [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md).
12. If necessary, save your pairwise 3D registration context and pairwise 3D registration result buffer using [`M3dregSave`](../../Reference/3dreg/M3dregSave.md) or [`M3dregStream`](../../Reference/3dreg/M3dregStream.md).
13. Free your pairwise 3D registration context and pairwise 3D registration result buffer using [`M3dregFree`](../../Reference/3dreg/M3dregFree.md), unless [`M_UNIQUE_ID`](../../Reference/3dreg/M3dregAlloc.md) was specified during allocation.

Note that for applications that are merging point clouds from multiple fixed 3D sensors into a single point cloud, you can avoid performing steps 1-8 multiple times. To do so, save ([`M3dregSave`](../../Reference/3dreg/M3dregSave.md)) the result buffer, and then restore ([`M3dregRestore`](../../Reference/3dreg/M3dregRestore.md)) and reuse it with [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md).
