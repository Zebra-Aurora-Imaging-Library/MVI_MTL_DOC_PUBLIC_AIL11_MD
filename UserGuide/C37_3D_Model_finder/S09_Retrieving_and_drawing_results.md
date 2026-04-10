---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Retrieving_and_drawing_results
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Retrieving and drawing results"
---

# Retrieving and drawing results

After successfully locating model occurrences in your point cloud using [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md), you can extract the required results from your 3D model finder result buffer using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) and [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md).

## Possible results

You can retrieve several types of results with the 3D Model Finder module. These results provide considerable information on the nature of the occurrence(s) found. In addition to general information about the search, such as the number of occurrences found, the status of the last [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operation, the maximum fit distance used during the search, and the size of the point cloud's range component, you can retrieve the following results for each found occurrence:

- Score, which is based on the occurrence's points coverage of the model.
- Root-mean-square error between the occurrence's points and the model.
- Number of points and number of reserved points.
- Center point coordinates.

Note that certain result types are supported only with certain types of models; the following results are available only for some types of model occurrences:

- Area (not available for surface model occurrences).
- Volume (not available for rectangular plane and surface model occurrences).
- Size (not available for sphere and surface model occurrences).
- Radius (available only for cylinder and sphere model occurrences).
- X-, Y-, and Z-components of the normal unit vector (available only for rectangular plane model occurrences and faces of box model occurrences).

The following results are available only for box model occurrences:

- Minimum and maximum X-, Y-, and Z-coordinates, ignoring rotation.
- Method used to complete the box.
- Number of visible faces.

The following results are available only for cylinder model occurrences:

- X-, Y-, and Z-components of the central axis unit vector.
- X-, Y-, and Z-coordinates of the center of each base.

The following results are available only for rectangular plane model occurrences:

- X-, Y-, and Z-coordinates of the closest point to the origin of the working coordinate system.
- Coefficients of the plane equation.

The following results are available only for surface model occurrences:

- X-, Y-, and Z-position.
- Status of the refine registration.
- Color score.
- Fit score.
- Target score.
- Grip scores (not available for planar surface 3D model finder result buffers).
- Highest grip score and its label (not available for planar surface 3D model finder result buffers).

Certain result types are supported for the faces of a box model occurrence. In addition to score, root-mean-square error, number of points, number of reserved points, center point coordinates, and normal unit vector components, the following results are available for each face of a box occurrence:

- Plane coverage.
- Whether the face was visible in the target.

By default, results are returned in descending order of match score, such that the result with the highest score is returned first. You can change the sorting key and direction using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SORT`](../../Reference/3dmod/M3dmodControl.md) and [`M_SORT_DIRECTION`](../../Reference/3dmod/M3dmodControl.md), respectively. Generally, you should first retrieve the total number of occurrences found for the model in the 3D model finder context, to ascertain the size of the result array needed. For a complete description of all possible results, refer to the description of [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) in the Aurora Imaging Library Reference.

Occurrences are indexed with positive integers starting from 0.

## Drawing results

The [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) function provides several operations for drawing results into a 3D graphics list; for each occurrence, you can draw:

- The geometric shape or surface of the defined model at the location of the occurrence.
- The bounding box.
- The inlier points.
- The reserved points.

Note that the bounding box is the smallest axis-aligned box that contains the geometric shape or surface of the model at the location of an occurrence, depending on the model type. If you draw the geometric shape or surface of the defined model at the location of the occurrence and the bounding box, then the bounding box will be the smallest-axis aligned box that fully contains the drawn shape or surface of the model. It does not necessarily contain all inlier points. For example, if you specify a very large maximum fit distance to use during the search, your occurrences can include inlier points that are far from the geometric shape or surface of the model. They will therefore not be enclosed by the bounding box.

You can also specify how to draw the above graphics with successive calls to [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md). For example, you can set the color for each type of drawn graphic. You can also set the thickness of the outline of the bounding box, and whether it appears as a solid surface, a wireframe, or as points. For a complete description of all possible options for the draw, refer to the description of [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) in the Aurora Imaging Library Reference.

Note that points are represented as squares in a 3D display. You can specify the thickness of inlier or reserved points in number of pixels, and they will always be shown with sides of this length when displayed, regardless of their distance from the 3D display's viewpoint.

To perform a draw operation, you typically allocate a draw 3D model finder context to hold the settings for the draw, using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with either[`M_DRAW_3D_GEOMETRIC_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md)for geometric draw contexts or[`M_DRAW_3D_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md)for surface draw contexts. You then specify draw operations and options for the draw with successive calls to [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md). Finally, you call [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to perform the draw.

Alternatively, you can skip allocating a draw 3D model finder context and instead use a default draw 3D model finder context with default draw settings. To do so, directly call [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) with [`M_DEFAULT`](../../Reference/3dmod/M3dmodDraw3d.md). Note that the default settings specify to draw the geometric shape of the defined model at the location of the occurrence, the bounding box, and the inlier points of each found occurrence, with a default point thickness of one pixel and a default wireframe bounding box appearance.
