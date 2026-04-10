---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: Retrieving_and_drawing_results
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / Retrieving and drawing results"
---

# Retrieving and drawing results

After successfully performing an [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) operation, you can retrieve the required results from your 3D measurement result buffer using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md). You can also copy certain results using [`M3dmeasCopyResult`](../../Reference/3dmeas/M3dmeasCopyResult.md).

## Possible results

You can retrieve several types of results with the 3D Measurement module. These results provide considerable information about the find marker and fit operations.

### Find marker results

For find marker operations, you can retrieve numerous details about the markers/transitions found, the profiles analyzed, the default template, path, or template, as well as general information.

After a successful find marker operation, you can retrieve the following results about a marker or marker transition, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md):

- The maximum and minimum Z-values of the marker or transition ([`M_MAX_Z`](../../Reference/3dmeas/M3dmeasGetResult.md) and [`M_MIN_Z`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The difference between the maximum and minimum Z-values of the marker or transition ([`M_DEPTH`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- Whether the marker was inside or outside the fit ([`M_FIT_INLIER_STATUS`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The position of the marker or transition ([`M_POSITION_...`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The position of the start and end of the marker ([`M_MARKER_POSITION_START`](../../Reference/3dmeas/M3dmeasGetResult.md) and [`M_MARKER_POSITION_END`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The position of the start and end of the transition ([`M_TRANSITION_POSITION_START`](../../Reference/3dmeas/M3dmeasGetResult.md) and [`M_TRANSITION_POSITION_END`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The width ([`M_WIDTH`](../../Reference/3dmeas/M3dmeasGetResult.md)) and width relative to the nominal length of the profile ([`M_WIDTH_RELATIVE`](../../Reference/3dmeas/M3dmeasGetResult.md)) of the marker or transition.
- The spacing ([`M_SPACING`](../../Reference/3dmeas/M3dmeasGetResult.md)) and spacing relative to the nominal length of the profile ([`M_SPACING_RELATIVE`](../../Reference/3dmeas/M3dmeasGetResult.md)) between markers.
- The offset ([`M_OFFSET`](../../Reference/3dmeas/M3dmeasGetResult.md)) of the marker or transition from a specified relative position ([`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md)).
- The polarity of the transition or pair ([`M_POLARITY`](../../Reference/3dmeas/M3dmeasGetResult.md) and [`M_POLARITY_PAIR`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The strength of the marker or transition ([`M_STRENGTH`](../../Reference/3dmeas/M3dmeasGetResult.md)). The strength considers the Z-value variation and slope of the transition.
- The type of transition ([`M_TRANSITION`](../../Reference/3dmeas/M3dmeasGetResult.md)). A transition is either an edge transition, invalid transition, or ridge transition.

You can retrieve the following information about a profile, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md):

- The response of the profile after applying a filter ([`M_PROFILE_RESPONSE`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The size in X of the profile response ([`M_RESULT_SIZE_X`](../../Reference/3dmeas/M3dmeasGetResult.md)). This corresponds to the number of profile points.
- The number of markers found in the profile ([`M_NUMBER_MARKER`](../../Reference/3dmeas/M3dmeasGetResult.md)).

You can retrieve the following information about a default template, path, or template, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md):

- The number of profiles analyzed ([`M_NUMBER_PROFILE`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The projection angle ([`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The sampling distance between profile points ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasGetResult.md)).

You can also retrieve the following general result, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md):

- Whether the find marker operation was successful, or why the find marker operation failed ([`M_STATUS_FIND`](../../Reference/3dmeas/M3dmeasGetResult.md)).

### Fit results

For an [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) operation, you can retrieve numerous details about the fitted 3D geometry and differences between the fitted 3D geometry and template, as well as general information.

After a successful fit operation, you can retrieve the following results about a fitted 3D geometry, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md):

- The components of the fitted 3D geometry's unit vector ([`M_AXIS_...`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The center point of the fitted 3D geometry ([`M_CENTER_...`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The start and end point of the fitted 3D geometry ([`M_START_POINT_...`](../../Reference/3dmeas/M3dmeasGetResult.md) and [`M_END_POINT_...`](../../Reference/3dmeas/M3dmeasGetResult.md), respectively).

You can retrieve the following information to compare the fitted 3D geometry to the defined template, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md):

- The angle between the fitted 3D geometry and the template ([`M_ANGULARITY`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The distance between the center of the fitted 3D geometry and the template ([`M_CENTER_DISTANCE`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The maximum distance between the fitted 3D geometry and the template ([`M_DISTANCE_MAX`](../../Reference/3dmeas/M3dmeasGetResult.md)).

You can also retrieve the following information about the fit operation itself, using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md):

- The root-mean-squared (RMS) error for the fit ([`M_FIT_RMS_ERROR`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- The number of markers used for the fit ([`M_NUMBER_MARKER_FITTED`](../../Reference/3dmeas/M3dmeasGetResult.md)) and the number of marker outliers ([`M_NUMBER_MARKER_OUTLIERS`](../../Reference/3dmeas/M3dmeasGetResult.md)).
- Whether the fit operation was successful, or why the fit operation failed ([`M_STATUS_FIT`](../../Reference/3dmeas/M3dmeasGetResult.md)).

To obtain the fitted 3D geometry itself, copy it into a 3D geometry object using [`M3dmeasCopyResult`](../../Reference/3dmeas/M3dmeasCopyResult.md):

## Drawing results

The [`M3dmeasDraw`](../../Reference/3dmeas/M3dmeasDraw.md) function provides several operations for drawing results in any specified image buffer or 2D graphics list. You can also choose to draw in the display's overlay buffer. By drawing into the display's overlay buffer, you can annotate an image non-destructively (see [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md)).

You can use a previously allocated 2D graphics context (see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md)) to control the drawing color, or use the default 2D graphics context ([`M_DEFAULT`](../../Reference/3dmeas/M3dmeasDraw.md)).

Supported drawing operations include drawing the path or template, the profile region or direction of the search along the profile, a cross at the found marker or transition positions, a cross at the positions of the markers that were used for fit or the positions of the marker outliers, or the fitted 3D geometry.

The [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md) function provides several operations for drawing results into a 3D graphics list. You can draw:

- A cross at the found marker's position.
- A step at the found marker's transition position.
- The extracted points that form the profile.
- The profile region.
- The profile response.
- The defined path or template, or the fitted 3D geometry.
- The projection of the defined path or template, or of the fitted 3D geometry.

You can also specify how to draw the above graphics with successive calls to [`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md). For example, you can set the color and thickness for each type of drawn graphic. You can also specify to draw it as a solid, a wireframe, or as points. For a complete description of all possible options for the draw, refer to the description of [`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md) in the Aurora Imaging Library Reference.

To perform a 3D draw operation, you typically allocate a draw 3D measurement context to hold the settings for the draw, using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), [`M_DRAW_3D_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), or [`M_DRAW_3D_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). You then specify draw operations and options for the draw with successive calls to [`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md). Finally, you call [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md) to perform the draw.

Alternatively, you can skip allocating a draw 3D measurement context and instead use a predefined draw 3D measurement context with default draw settings. To do so, directly call [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md) with [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasDraw3d.md).
