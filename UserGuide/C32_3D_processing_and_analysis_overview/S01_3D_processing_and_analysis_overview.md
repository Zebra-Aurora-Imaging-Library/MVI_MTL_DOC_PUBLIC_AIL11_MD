---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_processing_and_analysis_overview
section: 3D_processing_and_analysis_overview
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_processing_and_analysis_overview / 3D processing and analysis overview"
---

# Aurora Imaging Library 3D processing and analysis overview

You can perform a variety of procedures to process and analyze 3D information.

The Aurora Imaging Library 3D modules support display, processing, and analysis of 3D data (depth maps and point clouds). You can generate depth maps from point cloud data, or convert a depth map to a point cloud. You can also create and work with 3D geometries, such as a 3D box, cylinder, or sphere.

You can perform arithmetic and statistical operations, as well as comparative measurements between point clouds, depth maps, and 3D geometries. For example, you can fit a 3D geometry to a point cloud or depth map and calculate distances between points and the fitted surface. You can also register points obtained from multiple views of the same 3D scene so that the same point in a scene has the same coordinates when represented in different point clouds (pairwise registration).

You can scale, rotate, and translate 3D data, compute 3D profiles to examine a cross-section of data, and merge multiple point clouds. You can also sample a subset of points or reconstruct surfaces (meshes) for point clouds.

Most Aurora Imaging Library 3D functions work with 3D data stored in an Aurora Imaging Library 3D-processable point cloud or depth map container, which itself holds information in components. Each component is an Aurora Imaging Library object that stores an aspect of the 3D data in a buffer. For example, the range component in a 3D-processable point cloud container stores the world coordinates of 3D points.

For processing and display of 3D data, a container must be in an Aurora Imaging Library 3D-processable point cloud or depth map format or a 3D-displayable format, respectively.

A point cloud or 3D geometry is not associated with a calibration context. Instead, 3D coordinates are expected to be real world values. A depth map container is also not associated with a calibration context; 3D properties (for example, [`M_3D_OFFSET_X`](../../Reference/buf/MbufControlContainer.md) and [`M_3D_OFFSET_Y`](../../Reference/buf/MbufControlContainer.md)) are applied to the X and Y-coordinates stored in the range component to achieve world coordinates. However, when working with a depth map image buffer using the 3D processing and analysis modules, the buffer must be associated with a calibration context and be fully corrected.

You can grab point clouds from a 3D sensor or generate point clouds from a laser line profiling setup. You can also load point clouds from PLY and STL files, or create them manually in your application. PLY and STL are 3D data formats that can be used to store point clouds acquired from 3D sensors or generated from CAD drawings. Using the Aurora Imaging Library 3D Reconstruction module, you can also extract 3D information from 2D images to create a point cloud that you can pass to other 3D modules for processing and analysis. The 3D Reconstruction module can also perform stereo vision triangulation to compute the 3D world coordinates of points from their pixel location in the image plane of two or more cameras.

## What you need to know

Before using the Aurora Imaging Library 3D modules, you should read [Working with 3D](../C02_Building_an_application/S12_Working_with_3D.md). If you require in-depth information while working, you can refer to [Part 6](toc.md#3D_related_information) of the Aurora Imaging Library User Guide. The above-mentioned sections in [Chapter 2](../C02_Building_an_application/ChapterInformation.md) and [Part 6](toc.md#3D_related_information) elaborate on the following concepts: 3D data containers and grabbing, converting 3D data for processing and 3D display, displaying 3D data and graphics, calibration, storing the output of 3D functions, regions of interest, fixturing in 3D, using the 3D geometry module, and 3D reconstruction using laser line profiling.
