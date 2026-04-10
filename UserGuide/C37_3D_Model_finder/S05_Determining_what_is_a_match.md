---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Determining_what_is_a_match
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Determining what is a match"
---

# Determining what is a match

Before customizing your search settings, it is necessary to understand how a match between your model and occurrences in the point cloud is determined. The score ([`M_SCORE`](../../Reference/3dmod/M3dmodGetResult.md)) is the primary factor in determining which occurrences are considered matches with the model in your 3D model finder context.

## Score

The **score** is a measure of how well an occurrence matches a model. It is defined as the model coverage, which is the percentage of the model's surface found in the occurrence and covered by inlier points. When you specify your search settings using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md), use [`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md) to set the acceptance value for the score. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. You can set the maximum expected model coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

You can also set the certainty level with [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md). An occurrence will be returned only if its score is greater than or equal to the acceptance value; however, an occurrence with a score greater than or equal to the certainty level is considered a certain match. For more information on the uses of the acceptance and certainty levels, refer to[Customizing search settings](S06_Customizing_search_settings_general.md). You can retrieve the score of an occurrence, using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_SCORE`](../../Reference/3dmod/M3dmodGetResult.md).

The following image shows a point cloud with a cylinder occurrence. The bounding box, inlier points, and geometric shape of the model have all been drawn at the location of the occurrence, using [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). The maximum expected coverage is set to 40%, and the acceptance level is set to 50%. Given that the inlier points sufficiently cover the model's surface at the location of this occurrence, it is returned as a valid occurrence.

*[Image: 3dmod_inlier_points.png]*

To determine which points belong to a certain occurrence, you can set a fit distance, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md), which specifies a point's maximum distance from the geometric shape of the model at the occurrence for inclusion in the occurrence. A point must be within the fit distance of the model's shape at the location of the occurrence to be considered an inlier. Use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) to specify how Aurora Imaging Library establishes the fit distance; by default, it is calculated automatically for geometric models, but you can also explicitly set the fit distance. Note that the fit score calculation is not automatically enabled for surface models. To retrieve the fit score and to specify the fit distance, see [Fit score and maximum fit distance](S08_Search_settings_specific_to_surface_models.md). Also note that while a larger fit distance will generally improve an occurrence's score, since more included points will improve the model coverage, it will also generally increase the RMS error.

Note that [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) can fail to find an occurrence if it contains a very small number of inlier points amid a very large number of irrelevant background points.

## Reserved points

Aurora Imaging Library defines a region around every occurrence in which points are deemed**reserved points**, and cannot be considered part of any other occurrence. Note that reserved points are not included when calculating the score. Aurora Imaging Library reserves points both to ensure that multiple occurrences are not found in the same region, and to speed up the search for subsequent occurrences. Once identified, reserved points are ignored as Aurora Imaging Library searches for more occurrences. You can draw the reserved points of each specified occurrence for all model types by first calling [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_RESERVED_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md) and then calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).

For cylinder and sphere models, you can specify the distance from an occurrence to consider the reserved area. Points found in this area are considered reserved. By default, this is 10% of the occurrence's radius, but you can set it to any percentage of the occurrence's radius using[`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_RESERVED_POINTS_DISTANCE`](../../Reference/3dmod/M3dmodControl.md).

Note that if you define a cylinder model with bases ([`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_CYLINDER`](../../Reference/3dmod/M3dmodDefine.md) or [`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodDefine.md), and [`M_WITH_BASES`](../../Reference/3dmod/M3dmodDefine.md)), any points found on the bases of occurrences will be considered reserved. Points on an occurrence's bases will therefore not affect the score whether or not the model includes bases. However, if you expect cylindrical occurrences with bases, you should still define your cylinder model with bases. By reserving points on the bases of found occurrences, Aurora Imaging Library decreases the search time for subsequent occurrences, since fewer points remain through which to search. You can also use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_BASES`](../../Reference/3dmod/M3dmodControl.md) to set whether your cylinder model includes bases. Note that, just like when adding or deleting a model from your find 3D model finder context using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), changing the settings of a find 3D model finder context or model using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) requires preprocessing the context again, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md), before performing a new search.
