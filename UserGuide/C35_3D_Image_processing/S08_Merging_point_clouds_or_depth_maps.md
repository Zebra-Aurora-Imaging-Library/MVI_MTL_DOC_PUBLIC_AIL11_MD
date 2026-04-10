---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Merging_point_clouds_or_depth_maps
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Merging point clouds or depth maps"
---

# Merging point clouds or depth maps

You can use [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) to combine multiple point clouds into a single point cloud. The function assumes the working coordinate systems of the point clouds are aligned. Optionally, you can pass [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) a subsample context when merging, to reduce the number of points in the resulting point cloud. Note that, if you pass a subsample context that specifies an organized destination point cloud, the resulting merged point cloud will be organized. Alternatively, you can pass a stitch context to [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) when merging multiple organized point clouds, and the resulting merged point cloud will be organized. Note that you cannot pass a stitch context when merging unorganized point clouds. Without one of the aforementioned contexts, the resulting point cloud is always unorganized when using [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md). For information on point cloud organization, see [Organized and unorganized point clouds](S12_Organized_and_unorganized_point_clouds.md).

You can also use [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) to combine multiple depth maps into a single depth map. In this case, you must pass [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) a stitch context when merging.

To align the working coordinate systems of multiple point clouds and then merge, use [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) instead. See [Merging point clouds, retrieving specific results, and drawing results](../C40_3D_Registration/S08_Using_Result_Merge.md)for more information.
