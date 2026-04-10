---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Cropping_or_masking_points
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Cropping or masking points"
---

# Cropping or masking points

To crop or mask points in a point cloud, use [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md). Source components are copied into the destination container, where the destination container's confidence component determines a point's viability after the crop operation. Specifically, rejected points (those cropped out) are assigned a 0 confidence value in the destination's confidence component. You can crop using a specified 3D geometry object, or mask points using an image buffer. You can also apply a transformation matrix on your point cloud to fixture it before performing the cropping operation.

## Cropping using a 3D geometry

When cropping using a 3D geometry, you can use a closed geometry to define a region in 3D space. Points inside the region are kept; points outside the region are cropped out. Specify [`M_INVERSE`](../../Reference/3dim/M3dimCrop.md) to reverse which points are kept.

The following animation shows cropping using a 3D cylinder geometry.

*[Image: 3dim_CroppingIn3D_Cylinder]*

Besides other geometries, you can crop using a plane geometry. By definition, points on one side of the plane are kept, and points on the other side are cropped out. Specify [`M_INVERSE`](../../Reference/3dim/M3dimCrop.md) to reverse which points are kept. Note that the direction of the plane's normal vector determines the default side for which points are kept.

The following animation shows cropping using a 3D plane geometry.

*[Image: 3dim_CroppingIn3D_Plane]*

## Cropping using a mask image

You can crop using a mask image, where pixels with a 0 value determine points to crop, after projection onto the mask image buffer. [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) supports three types of mask image buffers:

- Uncalibrated image masks.
- Uniformly-calibrated image masks.
- 3D-calibrated image masks.

### Uncalibrated image masks

With an uncalibrated image mask, [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) maps points to the same location in the mask image buffer as the point's location in the range component of its point cloud container. The image buffer's size in X and Y must be the same as the source container's range component. If you perform an operation that returns a mask image buffer, you can use it as an uncalibrated image mask, provided its size matches the source container's range component.

For example, you can create an uncalibrated image mask using the background of a point cloud. You can fit a 3D plane geometry object to the background of your point cloud, using [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) with [`M_PLANE`](../../Reference/3dmet/M3dmetFit.md). Next, retrieve the size of the source point cloud's range component, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmet/M3dmetGetResult.md), and allocate an image buffer of this size using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md). You can then use [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_INLIER_MASK`](../../Reference/3dmet/M3dmetCopyResult.md) to copy the mask constructed by the points that were considered inliers during the fit into the buffer. You can then pass the buffer as an uncalibrated image mask to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md); if you specify [`M_INVERSE`](../../Reference/3dim/M3dimCrop.md) when calling [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md), you will remove only those points that fit your background plane from the point cloud, leaving your foreground points untouched.

You can also create an uncalibrated image mask using [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md). In this case, your point cloud contains anomalous points, known as outliers, which might represent noise introduced during point cloud acquisition. Use [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) to create a mask image in which pixels that correspond to outlier points are set to a non-zero value, while the rest of the pixels are set to zero. When passing this mask to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md), specify [`M_INVERSE`](../../Reference/3dim/M3dimCrop.md) to crop the outlier points, since these are identified with non-zero values in the mask. See [Invalidating outlier points](S06_Smoothing_a_point_cloud_and_invalidating_outlier_points.md).

### Uniformly-calibrated image masks

With a uniformly-calibrated image mask, [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) projects points orthogonally onto the mask, which means that points are projected parallel to the Z-axis, onto the XY-plane. To create such a mask, you can start by generating a fully corrected depth map of the point cloud using [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md), which orthogonally project points onto the XY (Z=0) plane. Next, you can use the Model Finder module to locate the target object in the resulting depth map, and then clear to 0 unwanted pixels as required. Alternatively, you can use a graphical user interface to interactively block out or mask areas in the depth map image, before passing the mask image to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md).

The following image shows cropping using a uniformly-calibrated mask image.

*[Image: 3dim_CropWithMask.png]*

### 3D-calibrated image masks

You can also crop using a 3D-calibrated image mask. In this case, [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) projects points along paths that follow the frustum of the camera's view (instead of orthogonally), as represented by the mask image's associated camera calibration information. To do so, use an image of the scene that is associated with a camera calibration of type [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md). This is useful when selecting points using a graphical user interface, where you can select or block out points on screen, thereby defining the mask image. Since the camera image is 3D-calibrated, the region selected from the display specifies points within a 3D volume. To apply the crop, pass the calibrated mask image buffer to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md).

## Specifying the organization type

When calling [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md), you can specify how to organize the resulting point cloud. The following settings are available:

- **Unorganized** ([`M_UNORGANIZED`](../../Reference/3dim/M3dimCrop.md)). Specifies to store results in an unorganized point cloud, removing any invalid points in the process. This option typically uses less memory, at the cost of losing organization.
- **Shrink** ([`M_SHRINK`](../../Reference/3dim/M3dimCrop.md)). Specifies to copy, from source to destination, the smallest region that holds all valid points. With this option, point cloud organization is kept. This is useful when cropping a small, localized area of a larger, organized point cloud.
- **Same** ([`M_SAME`](../../Reference/3dim/M3dimCrop.md)). Specifies to keep the source point cloud's organization, and to modify only the [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) component, invalidating the cropped points by setting their confidence to 0; all other source components are copied unchanged into the destination container. [`M_SAME`](../../Reference/3dim/M3dimCrop.md) is a fast option if cropping in place, since no separate allocation or copy operations are required. The [`M_SAME`](../../Reference/3dim/M3dimCrop.md) option is also useful if the relation to the original point positions are needed in the resulting point cloud. [`M_SAME`](../../Reference/3dim/M3dimCrop.md) is the default option.

For information on point cloud organization, see [Organized and unorganized point clouds](S12_Organized_and_unorganized_point_clouds.md).

## Fixturing

You can optionally apply a transformation matrix on your point cloud to fixture it before performing the cropping operation. To do so, use [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) with a [`M_TRANSFORMATION_MATRIX_OBJECT_ID`](../../Reference/3dim/M3dimCrop.md). The matrix object must have been previously allocated using[`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The transformation matrix must be any valid affine transformation matrix, where the last row is (0,0,0,1). If you do not want to transform your point cloud, you can instead use [`M_IDENTITY_MATRIX`](../../Reference/3dim/M3dimCrop.md) to specify an identity matrix, which will not transform or rotate your point cloud in any way.

Note that passing a transformation matrix to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) is the equivalent to first calling [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md), and then calling [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) on the resulting transformed point cloud.

Typically, you store the resulting transformed points to a different container if your source point cloud has multiple instances of regions that need to be analyzed. For more information, refer to [Fixturing in 3D overview](../C45_Fixturing_in_3D/S01_Fixturing_in_3D_overview.md).
