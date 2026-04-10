---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: Pairwise_alignment
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / Pairwise alignment"
---

# 3D Registration using the Iterative Closest Point algorithm

The Aurora Imaging Library 3D Registration module registers point clouds using the Iterative Closest Point algorithm. The function [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) internally iterates through successive estimates of the transformation required to align the working coordinate systems of two point clouds, until a specified stop condition is reached. Once a stop condition is reached, [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) returns the transformation matrix that transforms the points of one point cloud such that the average overall distance between its points, and the reference point cloud's points, is as low as possible. With these results, you can then use [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) to combine the point clouds into a single point cloud.

With each iteration, points from one point cloud are paired with the closest points in the reference point cloud, and the distance between the points is minimized using a transformation. New point pairs are made with each iteration. After several iterations, if the registration was successful, then the point clouds should, at least partially, overlap. This means that the transformation that moves the points of a point cloud, to its reference point cloud, is also the transformation that aligns the point cloud's working coordinate system with the reference point cloud's working coordinate system. As a result, the same coordinate values are used to describe equivalent points from each point cloud.

Consider the following animation:

*[Image: 3dreg_icp_iterations]*

> **Note:** Note that the 3D Registration module only works on point clouds, not depth maps. To convert a depth map into a point cloud, use [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

## Subsampling point clouds

Optionally, you can subsample point clouds prior to registration. This can speed up the registration. You can enable subsampling for all point clouds involved in the registration operation using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md), or enable subsampling for a point cloud associated with a specific registration element with [`M_SUBSAMPLE_REFERENCE`](../../Reference/3dreg/M3dregControl.md) or [`M_SUBSAMPLE_TARGET`](../../Reference/3dreg/M3dregControl.md). Subsampling for all point clouds is applied before any subsampling for a point cloud associated with a specific registration element. A point cloud will be subsampled twice if both are enabled. For example, you can first apply a general subsampling to all point clouds, and then apply a more specific subsampling to individual point clouds, if needed. Registration contexts and registration elements have internal sumbsampling contexts for controlling the subsampling techniques.

You can control the subsampling techniques by calling [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with the identifier of the internal subsampling context (which you can inquire using [`M3dregInquire`](../../Reference/3dreg/M3dregInquire.md) with [`M_SUBSAMPLE_CONTEXT_ID`](../../Reference/3dreg/M3dregInquire.md), [`M_SUBSAMPLE_REFERENCE_CONTEXT_ID`](../../Reference/3dreg/M3dregInquire.md), or [`M_SUBSAMPLE_TARGET_CONTEXT_ID`](../../Reference/3dreg/M3dregInquire.md)). For example, you can set the subsampling mode ([`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md)) to [`M_SUBSAMPLE_GEOMETRIC`](../../Reference/3dim/M3dimControl.md). This mode can help the registration reach convergence by removing points that contribute to sliding or unstable transformations. For more information about subsampling modes, see [Subsampling a point cloud](../C35_3D_Image_processing/S11_Sampling_a_point_cloud_or_a_3D_geometry.md).

## Point pair refinements

During the pairwise 3D registration process, Aurora Imaging Library pairs points between the target and reference point clouds. You can control how the operation establishes these pairings, to help reduce divergence and minimize inclusion of outlier points. For example, you can set a distance beyond which points will not be paired ([`M_PAIRS_CREATION_MAX_POINT_DISTANCE`](../../Reference/3dreg/M3dregControl.md)), or you can decide how many pairings are permitted per reference point ([`M_PAIRS_CREATION_PER_REFERENCE_POINT_MODE`](../../Reference/3dreg/M3dregControl.md)).

> **Note:** Note that if the point pairing is already known, you should use [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md) to find the transformation between the point clouds. For more information, see [Finding the transformation between 2 sets of points with a known pairing](../C35_3D_Image_processing/S04_Finding_the_transformation_between_2_sets_of_points.md).

When the distance between the reference and target point clouds is significant, you should enable [`M_PAIRS_CREATION_FROM_TARGET`](../../Reference/3dreg/M3dregControl.md) to help prevent divergence. This setting performs a bidirectional comparison, first finding target points relative to reference points, and then vice versa. Doing so increases processing time. However, if you choose [`M_AUTO`](../../Reference/3dreg/M3dregControl.md), this will limit the increase in processing time since Aurora Imaging Library applies the setting to certain iterations only.

When point clouds are far apart, besides enabling [`M_PAIRS_CREATION_FROM_TARGET`](../../Reference/3dreg/M3dregControl.md), you can set [`M_PAIRS_CREATION_PER_REFERENCE_POINT_MODE`](../../Reference/3dreg/M3dregControl.md) to [`M_AUTO`](../../Reference/3dreg/M3dregControl.md). This will automatically adjust the number of created pairs per reference point at each iteration, so that, as the point clouds converge, there are fewer pairs per reference point. By default, [`M_PAIRS_CREATION_PER_REFERENCE_POINT_MODE`](../../Reference/3dreg/M3dregControl.md) is set to [`M_SINGLE`](../../Reference/3dreg/M3dregControl.md). If for any reason you want to always have multiple pairs created regardless of point cloud proximity, you can use [`M_MULTIPLE`](../../Reference/3dreg/M3dregControl.md), which will create up to 6 pairs per reference point for each iteration.

To limit the number of pairings to which a target point can belong, use [`M_PAIRS_LIMIT_PER_TARGET_POINT_MODE`](../../Reference/3dreg/M3dregControl.md). If set to [`M_AUTO`](../../Reference/3dreg/M3dregControl.md), this setting adjusts the number of pairings based on point cloud proximity, permitting fewer pairings when the distance is slight. When point clouds are very close, the ideal setting for [`M_PAIRS_LIMIT_PER_TARGET_POINT_MODE`](../../Reference/3dreg/M3dregControl.md) is [`M_SINGLE`](../../Reference/3dreg/M3dregControl.md).

If you are expecting a limited number of outliers or minimal noise in your scene, use [`M_PAIRS_REJECTION_MODE`](../../Reference/3dreg/M3dregControl.md). This setting uses a statistical calculation to reject pairs that are too far apart. However, if the scene contains a lot of outliers, [`M_PAIRS_REJECTION_MODE`](../../Reference/3dreg/M3dregControl.md) might not be enough, and you should set a maximum distance with [`M_PAIRS_CREATION_MAX_POINT_DISTANCE`](../../Reference/3dreg/M3dregControl.md). For example, when the scene is not cropped for whatever reason but you have an initial guess of the target's location, registration could diverge if a maximum distance is not set.

|   |   |
| --- | --- |
| *[Image: 3dreg_pairing_default.png]* | Default settings find a single pairing per reference point ([`M_PAIRS_CREATION_PER_REFERENCE_POINT_MODE`](../../Reference/3dreg/M3dregControl.md) set to [`M_DEFAULT`](../../Reference/3dreg/M3dregControl.md) or [`M_SINGLE`](../../Reference/3dreg/M3dregControl.md)). Each blue line represents a pairing. |
| *[Image: 3dreg_pairing_creation_ref_auto.png]* | Setting [`M_PAIRS_CREATION_PER_REFERENCE_POINT_MODE`](../../Reference/3dreg/M3dregControl.md) to [`M_AUTO`](../../Reference/3dreg/M3dregControl.md) permits the pairing of each reference point to multiple target points. Green lines represent the additional pairings. |
| *[Image: 3dreg_pairing_creation_target_enabled.png]* | Enabling [`M_PAIRS_CREATION_FROM_TARGET`](../../Reference/3dreg/M3dregControl.md) finds pairings from target points to reference points. Orange dashed lines represent the pairings from target to reference. Note that every point in the target is accounted for. |
