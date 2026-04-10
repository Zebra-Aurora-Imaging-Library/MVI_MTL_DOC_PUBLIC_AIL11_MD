---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Smoothing_a_point_cloud_and_invalidating_outlier_points
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Smoothing a point cloud and invalidating outlier points"
---

# Smoothing a point cloud and invalidating outlier points

Point cloud noise can make subsequent processing less accurate (for example, reconstructing a surface using [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md)). If the point cloud is noisy, you can apply a smoothing filter to it, using [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md). Filtering reduces noise by changing the positions of points, but not does eliminate them. You can invalidate outlier points, using [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md).

## Smoothing a point cloud

[`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) reduces noise that can arise during point cloud acquisition. The following image shows a noisy point cloud that has been smoothed with a moving least squares (MLS) filter ([`M_SMOOTH_MLS`](../../Reference/3dim/M3dimControl.md)) and a bilateral filter ([`M_SMOOTH_BILATERAL`](../../Reference/3dim/M3dimControl.md)) ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_FILTER_MODE`](../../Reference/3dim/M3dimControl.md)). Although both results are similar, the bilateral filter result maintains sharper edges (visible in the image where the top surface meets the front surface).

*[Image: 3dim_Smoothing.png]*

The MLS filter applies smoothing equally to surface and edge points. The bilateral filter, on the other hand, smooths points on faces, leaving edge points much less affected. Deciding which filter to use depends on your particular point cloud and the requirements of your application. If edge preservation is not a concern, use the MLS filter since it is easier to set, requiring less fine-tuning with the control types mentioned above. To preserve edges or folds, use the bilateral filter. With the bilateral filter, if you set the edge-preserving control ([`M_NORMALS_WEIGHT_FACTOR`](../../Reference/3dim/M3dimControl.md)) to a value that is too low, little smoothing occurs, even to non-edge points; if set too high, results will resemble an MLS filter output.

The bilateral filter uses the source point cloud's normal values. If a normals component does not already exist in your source point cloud, the [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) function will calculate point normals and create a normals component. In this case, use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_USE_SOURCE_NORMALS`](../../Reference/3dim/M3dimControl.md) set to [`M_FALSE`](../../Reference/3dim/M3dimControl.md). Note that this is more efficient than separately calculating a normals component using [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md). Nevertheless, the method used for computing normals could improve filter results. Typically, the bilateral filter works best with normals that were calculated using the [`M_MESH`](../../Reference/3dim/M3dimControl.md) or [`M_TREE`](../../Reference/3dim/M3dimControl.md) neighbor search mode ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md)). However, normals created using the [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) mode can give faster filtering results.

When using the bilateral filter, you can adjust how well edges are preserved with [`M_NORMALS_WEIGHT_FACTOR`](../../Reference/3dim/M3dimControl.md). A smaller value results in more edge preservation. You can also set the [`M_WEIGHT_MODE`](../../Reference/3dim/M3dimControl.md) control type (available for either the bilateral or MLS filter). [`M_WEIGHT_MODE`](../../Reference/3dim/M3dimControl.md) sets how the Gaussian weights are determined for the smoothing operation. Typically, for bilateral smoothing, the weight mode is best left at the default setting ([`M_RELATIVE`](../../Reference/3dim/M3dimControl.md)), since the [`M_ABSOLUTE`](../../Reference/3dim/M3dimControl.md) setting is dependent on point cloud resolution, which can make it difficult to accurately configure the other filter context control settings.

Note that both the bilateral and MLS filters are expensive in terms of processing time. However, applying a smoothing filter to reduce noise results in a more accurate point cloud representation. For example, in the image above, the original point cloud was acquired from a box with flat sides. Despite the smooth-sided source, the acquisition process introduced significant noise. Once filtered, the resulting smoothed point cloud is a superior representation of the original object that you can use for further processing.

Smoothing a point cloud using [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) changes the positions of points, but does not eliminate them. If you want to invalidate outlier points, use [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md).

## Invalidating outlier points

You can use [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) to invalidate outlier points. Point clouds or depth maps originating from 3D scans of a scene often contain a certain amount of noise, in the form of extraneous points (outliers). [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) determines outliers based on their distance from other points, the density of neighboring points, or by using statistical calculations. For example, you can specify to determine outliers based on the mean and standard deviation of the local average distance distribution, which considers the average distance of all points to their closest neighbors. [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) sets the confidence of outlier points to zero; the points will still exist but are invalidated. This function can also create a mask image that you can use to crop points. See [Cropping using a mask image](S05_Cropping_or_masking_points.md).
