---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Metrology
section: 3D_Fitting
module_tag: 3dmet
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Metrology / 3D Fitting"
---

# Performing 3D fitting

The Aurora Imaging Library 3D metrology module can perform a fitting operation. The fitting operation tries to fit a specified type of 3D geometry to a point cloud or depth map; you can fit one of the following geometries: cylinder, line, plane, or sphere. The fitting operation minimizes the average root-mean-squared (RMS) error between the 3D geometry and the inlier points of the point cloud or depth map. The RMS error is minimized over several iterations.

For a single iteration, the RMS error is only calculated for points that are considered inliers for that iteration (as specified using [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) with the [`OutlierDistance`](../../Reference/3dmet/M3dmetFit.md) parameter) to avoid fitting the specified 3D geometry to spurious points in the point cloud or depth map. Spurious points are typically caused by image noise, object deformity, or occlusion. You can let [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) automatically determine the outlier distance, using [`M_AUTO_VALUE`](../../Reference/3dmet/M3dmetFit.md). Retrieve the computed value using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_OUTLIER_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md).

At the beginning of the fitting operation, an initial fit estimate is calculated (as specified using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_ESTIMATION_MODE`](../../Reference/3dmet/M3dmetControl.md)) to define an initial position of the 3D geometry relative to the point cloud or depth map. In the next iteration, points are defined as inliers or outliers depending on whether they are within the outlier distance from the surface of the 3D geometry. The RMS error between the geometry and inliers is calculated, and the 3D geometry is then repositioned to minimize the RMS error. This continues with each iteration, until one of the stop conditions is met.

Optionally, when performing a fitting operation with a fit 3D metrology context that has [`M_ESTIMATION_MODE`](../../Reference/3dmet/M3dmetControl.md) set to [`M_FROM_GEOMETRY`](../../Reference/3dmet/M3dmetControl.md), you can specify a 3D geometry object (allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)and defined using an appropriate function from the [`M3dgeo...`](../../Reference/3dgeo/M3dgeoPlane.md)module) to use for the first iteration. Otherwise, you do not specify a 3D geometry object for a fitting operation.

## Steps to perform a fitting operation

The following steps provide a basic methodology for performing a fitting operation:

1. Allocate a fit 3D metrology context, using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_FIT_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md).
2. Allocate a fit 3D metrology result buffer, using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_FIT_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md).
3. Control the settings of the fit 3D metrology context using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). These settings specify stop conditions for the fitting operation, and determine how the initial fit estimate is calculated using [`M_ESTIMATION_MODE`](../../Reference/3dmet/M3dmetControl.md).
   > **Note:** Note that, if [`M_ESTIMATION_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_FROM_GEOMETRY`](../../Reference/3dmet/M3dmetControl.md), use [`M3dmetCopy`](../../Reference/3dmet/M3dmetCopy.md) with [`M_ESTIMATE_GEOMETRY`](../../Reference/3dmet/M3dmetCopy.md) to copy a 3D geometry into the fit 3D metrology context. This geometry will then be used as the initial fit estimate. By default, the fit 3D metrology context contains an undefined geometry object, and will cause an error if you initiate the fit operation with this estimation mode.
4. Call [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) to perform the fitting operation. In the function call, you must specify the type of 3D geometry to fit to the point cloud or depth map, and how points are determined to be outliers or inliers.
5. Retrieve results from the fit 3D metrology result buffer using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md), including the status of the fitting operation. Retrieve the fitted geometry using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_FITTED_GEOMETRY`](../../Reference/3dmet/M3dmetCopyResult.md).

## Specifying points or pixels not to use for fitting

If you need to only fit to a subset of the points of a point cloud, or pixels of a depth map, you can do so by cropping a point cloud (using [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md)) or specifying a raster ROI for a depth map (using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)), respectively. This is useful, for example, when there are multiple objects in the point cloud or depth map to which a 3D geometry could be fit.

For information about cropping a point cloud, see [Cropping or masking points](../C35_3D_Image_processing/S05_Cropping_or_masking_points.md).

When specifying an ROI for a depth map, note that [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) fits the geometry to all depth map pixels inside the ROI that are not outside the specified outlier distance ([`OutlierDistance`](../../Reference/3dmet/M3dmetFit.md)). Therefore, your ROI should mask out any objects or parts of objects that you do not want considered for the fit operation.

*[Image: 3dmap_geometry_fit.png]*

## Using a fitted geometry to mask points from a point cloud

You can use a fitted geometry to specify which points to mask from a point cloud. That is, any points that the geometry was fitted to will have their confidence score set to 0, using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_OUTLIER_MASK`](../../Reference/3dmet/M3dmetCopyResult.md). Once the confidence score is set to 0, these points will be excluded from analysis and processing operations. This can be useful to remove a plane from the background.

The snippet below shows how to use a fitted plane geometry to mask points from a point cloud:

> **Code example:** [userguide.3d_metrology.masking_circular_bases_of_cylinders_for_fitting](userguide.3d_metrology.masking_circular_bases_of_cylinders_for_fitting)
