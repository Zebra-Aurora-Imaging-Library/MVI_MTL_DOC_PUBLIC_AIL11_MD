---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Search_settings_specific_to_surface_models
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Search settings specific to surface models"
---

# Search settings specific to surface models

Besides the search settings mentioned in the [Customizing search settings](S06_Customizing_search_settings_general.md), there are search settings specific to surface models.

> **Note:** Note that changing control type settings of a find 3D model finder context or model requires preprocessing the context again, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md).

## Fit refinement

After finding occurrences of a surface model, you can have [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) refine the fit of the model to get more accurate score and pose information of the occurrence.

By default, for a surface model, Aurora Imaging Library uses the features of the model and target to find occurrences, but it doesn't do any fitting. To get more accurate score and pose information, you should enable refine registration. When enabled, 3D registration is internally performed between the model and each of the found occurrences. You can set [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) to use a predefined or a custom registration context.

There are two predefined 3D registration contexts:

- [`M_FIND_SURFACE_REFINEMENT_FAST`](../../Reference/3dmod/M3dmodControl.md), which prioritizes performance.
- [`M_FIND_SURFACE_REFINEMENT_PRECISE`](../../Reference/3dmod/M3dmodControl.md), which prioritizes accuracy and can take longer.

If the predefined 3D registration contexts don't meet your needs, you can set [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) to [`M_USER_DEFINED_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md). Then, you can inquire the identifier of the internal registration context using [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_USER_DEFINED_REGISTRATION_CONTEXT_ID`](../../Reference/3dmod/M3dmodInquire.md), and use [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) to configure the context. You can also use [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodCopy.md) to initialize the internal context with the settings of a predefined registration context and then adjust a few settings, using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md).

To retrieve the status of the refine registration, you can use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_STATUS_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodGetResult.md). To calculate and retrieve the root-mean square (RMS) error and the fit score of the occurrences, you must enable [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md). For more information, see [Fit score and maximum fit distance](S08_Search_settings_specific_to_surface_models.md).

## Model and target point cloud resolution

Point cloud resolution is a measure of the point cloud's level of detail. In Aurora Imaging Library, point cloud resolution is measured as the average distance between points. The greater the average distance between points, the lower the detail (and density). When the model point cloud and target point cloud have significantly different resolutions, matching will still happen, but Aurora Imaging Library will notify you that the two point clouds are incompatible and that your matching can be affected by this. If the distance between points in the scene is greater than that of the model, Aurora Imaging Library will subsample the model to match the resolution of the target. If the distance between points in the scene is lower than that of the model's, Aurora Imaging Library will upsample the model to match the resolution of the target, using its associated mesh. If a mesh is not provided in the model, Aurora Imaging Library will report an error when upsampling. To save time, you can prevent this by specifying the expected resolution of the scene, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SEARCH_POINT_RESOLUTION`](../../Reference/3dmod/M3dmodControl.md). By default, Aurora Imaging Library sets the resolution of the scene to be the same as the resolution of the defined model.

You can inquire the resolution of the model, using [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_MODEL_RESOLUTION`](../../Reference/3dmod/M3dmodInquire.md), and retrieve the scene resolution compatibility, using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_STATUS_SCENE_RESOLUTION`](../../Reference/3dmod/M3dmodGetResult.md). If the resolutions are not compatible, you can use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SEARCH_POINT_RESOLUTION`](../../Reference/3dmod/M3dmodControl.md) to specify the resolution of the scene. Note that you will need to preprocess the context again, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md).

## Simplifying the scene

Background points include those of objects that are too big or too small to be part of an occurrence of the model. You should enable [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) to remove background points for scenes that are very complex. Note that background removal does not use the floor plane (defined using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)) to establish which points to remove. You can enable [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) to remove floor points. Use the [`M_REMOVE_FLOOR_DIRECTION`](../../Reference/3dmod/M3dmodControl.md) and [`M_REMOVE_FLOOR_OFFSET`](../../Reference/3dmod/M3dmodControl.md) control types to set the direction and offset from the floor plane in which to remove points. You can use [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to draw the points that were considered background points and floor points (control the drawing operation using [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_BACKGROUND_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md) and [`M_DRAW_FLOOR_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md), respectively). Note that removing the background and/or floor takes some time, but sometimes makes the match faster.

For example, in the following case, enabling [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) removes the background box (in turquoise), making it easier to search for the model in the target point cloud.

*[Image: 3dmod_surface_remove_background.png]*

## Obtaining the most accurate model representation for score and fit

Scene projection and outlier removal can both remove points from your model. Scene projection removes the points of the model that would not be visible to the 3D sensor at the position of the occurrence before comparing the model to that occurrence for the score and fit. Outlier removal removes stray points of the model before the search begins. Typically, enabling scene projection and outlier removal will improve the score and result in a more accurate fit because extra points that would not be found in the occurrence are removed. See [Surface models](S04_Defining_and_adding_models_to_your_3D_model_finder_context.md) to learn more about outlier removal.

### Scene projection

To prevent the occurrences' scores from being lowered due to how the scene was acquired, you can enable [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SCENE_PROJECTION`](../../Reference/3dmod/M3dmodControl.md). This applies occlusion handling, which happens before comparing the model to the occurrence during the refine registration and score calculation.

When enabled and given the line of sight of the 3D sensor, points in the model that would be occluded at the position of an occurrence will be discarded. Aurora Imaging Library uses the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md)) to determine the line of sight and direction of projection. The default (0.0, 0.0, 1.0), provides a projection along the 3D sensor's Z-axis.

For example, when your target scene has points that are not visible due to the positioning of the 3D sensor, you can enable scene projection to remove points of the model before the score is calculated. This avoids lowering [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md) too much, which can result in false positives.

## Constraining results

To ensure that the found occurrences suit your needs, the 3D Model Finder module offers several ways to constrain the results of the search. You can set tolerances for extra points in the occurrences, color, neighboring point distances, and occurrence orientations.

### Target acceptance and extra points in the occurrences

Besides setting the minimum model score ([`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md)), you can set the target acceptance level, using [`M_ACCEPTANCE_TARGET`](../../Reference/3dmod/M3dmodControl.md), which sets the minimum target score. The target score is a measure, as a percentage, of the total points found in the occurrence that are also found in the model. A setting of 100% for the target score means that you will not tolerate any extra points; all points in the occurrence must have a matching point in the model. A setting of 0% (default) allows for any number of extra points. Essentially, the target score acceptance level allows you to control how tolerant your search is to details not present in the original model.*[Image: 3dmod_surface_coverage.png]*

### Color score

The color score is a measure, as a percentage, of the similarity between the color data of the model and the color data of the target. Note that by default, Aurora Imaging Library does not calculate the color score. You must enable [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_USE_COLOR`](../../Reference/3dmod/M3dmodControl.md) to calculate and obtain this result (using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_SCORE_COLOR`](../../Reference/3dmod/M3dmodGetResult.md)).

When enabled, besides being able to obtain the color score, you can specify the minimum color score required for an occurrence to be considered a match (using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_ACCEPTANCE_COLOR`](../../Reference/3dmod/M3dmodControl.md)). Aurora Imaging Library will only return occurrences with a color score greater than or equal to the color acceptance level.

The following example shows the occurrences found when [`M_USE_COLOR`](../../Reference/3dmod/M3dmodControl.md) is enabled and [`M_ACCEPTANCE_COLOR`](../../Reference/3dmod/M3dmodControl.md) is set to 0% compared to 50%. The model is similar in shape to all 7 objects in the target, but only 3 of them have similar color data.

*[Image: 3dmod_surface_color.png]*

Note that you can sort the results based on their color score ([`M_SCORE_COLOR`](../../Reference/3dmod/M3dmodControl.md)) with [`M_SORT`](../../Reference/3dmod/M3dmodControl.md).

### Fit score and maximum fit distance

By default, Aurora Imaging Library does not calculate the root-mean square (RMS) error and the fit score; you need to enable [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) to obtain these results (using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_RMS_ERROR`](../../Reference/3dmod/M3dmodGetResult.md) and [`M_SCORE_FIT`](../../Reference/3dmod/M3dmodGetResult.md)).

When enabled, besides being able to obtain these results, you can filter out occurrences based on the minimum required fit score ([`M_FIT_SCORE_MIN`](../../Reference/3dmod/M3dmodControl.md)) and the maximum fit distance ([`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) and [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)). In addition, you can sort the results based on their fit score ([`M_SCORE_FIT`](../../Reference/3dmod/M3dmodControl.md)) with [`M_SORT`](../../Reference/3dmod/M3dmodControl.md).

Note that [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) and its related settings have no effect on [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md), and vice versa. Even if refine registration is enabled, you must enable [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) to calculate and retrieve the root-mean square (RMS) error and the fit score.

### Resting plane and occurrence orientation

The resting plane is used to provide contextual information about how the model occurrence lies within the scene. When matching, Aurora Imaging Library considers all poses of the occurrence. You can, however, specify a resting plane so that Aurora Imaging Library only considers occurrences that sit on this plane. This does not affect any of the scores, but it filters occurrences that do not meet this constraint. Note that defining a resting plane does not decrease the search time because occurrences are filtered near the end of the search.

To define constraints on the occurrence's position, you will need to identify the resting plane of the model by copying a corresponding plane to the model, using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_RESTING_PLANE`](../../Reference/3dmod/M3dmodCopy.md). Note that to use a resting plane, you must also copy a floor plane ([`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)) to the find surface or planar surface 3D model finder context; the floor plane denotes the base of the scene. The resting plane uses the floor plane as a reference when establishing the tilt angle of the occurrence. To inquire the number of resting planes defined, use [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_NUMBER_RESTING_PLANE`](../../Reference/3dmod/M3dmodInquire.md).

By default, Aurora Imaging Library will only return occurrences that rest on the plane, +/- 10.0 degrees, since the found angle is not always exact. You can change the resting plane angular tolerance, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_RESTING_PLANE_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodControl.md). The tolerance value is relative to the model's resting plane. You should set a larger resting plane angular tolerance if you expect the occurrences to be at different angles.

## Score mode

To calculate the model score, target score, and color score, Aurora Imaging Library associates points using a positional comparison between model and target points, relative to local volumes within a partitioned 3D space. You can set the mode with which to associate model and target points for score calculations, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SCORE_MODE`](../../Reference/3dmod/M3dmodControl.md).

When [`M_SCORE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_STRICT`](../../Reference/3dmod/M3dmodControl.md), a point is either fully associated with another point or not associated at all. For example, when calculating the model score, a model point is considered fully covered (fully associated) if there is a target point in the same volume, and uncovered if not. This is the default score mode for a find surface 3D model finder context.

When [`M_SCORE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md), points can be partially associated. For example, when calculating the model score, if a model point's position is close to the boundary of a volume adjacent to the one it occupies and there is a target point in either of the volumes, the point is considered half covered; there must be target points in both volumes to fully cover the model point. This is the default score mode for a find planar surface 3D model finder context.

When your model has significant planar regions, [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md) is typically more reliable than [`M_STRICT`](../../Reference/3dmod/M3dmodControl.md). If the model points in the planar region fall in an array of volumes, but they are close to the boundaries shared with adjacent volumes, the target points might fall in the adjacent volumes. In this case, the score will partially increase with the [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md) mode, but it will not increase with the [`M_STRICT`](../../Reference/3dmod/M3dmodControl.md) mode. A slight improvement in positional accuracy, such that the target points fall in the same volumes as the model points, leads to a similar score with the [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md) mode, whereas the [`M_STRICT`](../../Reference/3dmod/M3dmodControl.md) mode produces a much higher score. You should use the [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md) mode for the most accurate results when dealing with a model that has planar regions.

## Reference axis

When an occurrence of a surface model is found, the model's reference axis determines the coordinates returned as the actual occurrence's found position. Position results return the coordinates of the model's reference axis origin transformed at the model occurrence.

By default, the reference axis origin is at the origin of the working coordinate system of the original point cloud from which the model was defined. This does not necessarily correspond to the center of the model. When adding the model using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), you can move the origin of the working coordinate system to the required position. For example, you can use [`M_CENTROID`](../../Reference/3dmod/M3dmodDefine.md) to move the origin of the working coordinate system to the point cloud's centroid (center of mass), setting the model's reference axis origin to this position.

If, after defining your model, the reference axis is not at a practical position for your application, you can still change the position of the reference axis origin, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SET_POSITION_...`](../../Reference/3dmod/M3dmodControl.md). This repositions the model's reference axis origin by moving the origin of the model's working coordinate system to the specified position.

The following images show the axes of the working coordinate system ([`M_DRAW_AXES_POSITION`](../../Reference/3dmod/M3dmodControlDraw.md)) drawn before (left) and after (right) changing the Z-coordinate of the model's reference axis origin ([`M_SET_POSITION_Z`](../../Reference/3dmod/M3dmodControl.md)). Note that the model's axes (visible in the images within the model) are relative to the bounding box of the model and remain unchanged.

*[Image: 3dmod_reference_axis.png]*

Modifying the reference axis origin can be useful if you cannot define a unique model for an object of interest. In this case, define a model that is unique and at a fixed location relative to your object of interest and move your reference axis.

The reference axis origin does not have to reside within the model, meaning that you can place the reference axis origin outside of the model boundaries.

Note that you can transform the model, using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dmod/M3dmodDefine.md), or later, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_TRANSFORM_MODEL`](../../Reference/3dmod/M3dmodControl.md). This does not affect the working coordinate system; when you transform the model, the orientation of the model changes but the reference axis does not change. To see how the model is oriented, you can draw the model's axes, using [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_AXES`](../../Reference/3dmod/M3dmodControlDraw.md).

The following images show the axes of the model ([`M_DRAW_AXES`](../../Reference/3dmod/M3dmodControlDraw.md)) drawn before (left) and after (right) transforming the model. Note that the reference axis (not pictured) remains unchanged.

*[Image: 3dmod_draw_axes.png]*

## Modifying and reusing results

After finding occurrences of a surface model, you can reuse the results to speed up the next search if you expect occurrences to be found in similar locations. To do so, enable [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REUSE_RESULT`](../../Reference/3dmod/M3dmodControl.md) and call [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) with the result buffer containing the results to reuse. [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) will search for occurrences at the stored positions, and then resume with the search if more occurrences are expected. Note that before passing the result buffer to [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md), you can use [`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md) to transform or delete results for an occurrence if there is a known movement between scans or you don't expect to find the occurrence in the next scene.

You can reuse the results of a search multiple times if you expect occurrences to be found in similar locations in multiple future searches, but you must avoid passing the original result buffer to [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) after the initial search, so as not to change the stored results. You should set up two result buffers: one for the initial search and another for use with subsequent searches. Before each subsequent search, copy into its result buffer the results from the initial search, using [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_RESULT`](../../Reference/3dmod/M3dmodCopyResult.md), and reuse this result buffer for the search. Note that if you only want to reuse the results once, you do not need to copy the results to a separate result buffer; you can call [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) with the original result buffer.

If you are modifying results, using [`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md), and you want the modification to be present for each search, you should perform the modifications on the result buffer with the original results, in-place (the same result buffer for the source and the destination). If you want to reuse the original (unmodified) results in a future search, you must specify different result buffers for the source and destination to avoid overwriting the original results.

## Find surface 3D model finder context

For a find surface 3D model finder context, you can also specify a grip label image and weight image for the surface model, as well as the complexity of the target scene and the perseverance to use to find occurrences.

### Grip label image

Aurora Imaging Library considers the model's entire surface when calculating an occurrence's score. If you want to calculate scores for specific regions to determine the best surface of the occurrence for gripping, you can copy a grip label image to a surface model in a find surface 3D model finder context, using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_GRIP_LABEL_IMAGE`](../../Reference/3dmod/M3dmodCopy.md). When an occurrence of a model with a grip label image is found, a score is calculated for each labeled surface.

The grip score is the percentage of the model's labeled surface found in the occurrence and covered by inlier points. You can use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with the [`M_SCORE_GRIP()`](../../Reference/3dmod/M3dmodGetResult.md) macro to retrieve the grip score for a specified label. Use [`M_SCORE_GRIP_BEST`](../../Reference/3dmod/M3dmodGetResult.md) to retrieve the highest grip score among all labels, and [`M_SCORE_GRIP_BEST_LABEL`](../../Reference/3dmod/M3dmodGetResult.md) to retrieve the label with the highest grip score.

### Weight image

By default, Aurora Imaging Library considers all points of the model equally when searching for occurrences. If you want to place more importance on certain points, you can copy a weight image to a surface model in a find surface 3D model finder context, using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_WEIGHT_IMAGE`](../../Reference/3dmod/M3dmodCopy.md). Weights indicate how significant it is for a given point to be found in the occurrence; the higher the weight, the greater the influence on the model score. The absence of a point with a high weight will reduce the score more than the absence of one with a low weight. Note that all pixels in a weight image must be positive.

### Scene complexity and perseverance

Information about the complexity of the scene is used to know how much of the target data belongs to occurrences. To specify the scene complexity for a find surface 3D model finder context, use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SCENE_COMPLEXITY`](../../Reference/3dmod/M3dmodControl.md). The default value is [`M_MEDIUM`](../../Reference/3dmod/M3dmodControl.md). For example, if the target scene contains one occurrence of the model on a simple background, setting [`M_SCENE_COMPLEXITY`](../../Reference/3dmod/M3dmodControl.md) to [`M_LOW`](../../Reference/3dmod/M3dmodControl.md) should be sufficient to find the occurrence. Whereas, if the target scene is busy and contains many features similar to the model, you should set [`M_SCENE_COMPLEXITY`](../../Reference/3dmod/M3dmodControl.md) to [`M_MEDIUM`](../../Reference/3dmod/M3dmodControl.md) or [`M_HIGH`](../../Reference/3dmod/M3dmodControl.md). Higher settings result in a more robust operation, but typically take more time.*[Image: 3dmod_surface_scene_complexity.png]*

Perseverance affects the number of times the algorithm tries to find occurrences before giving up. For a find surface 3D model finder context, you can set the algorithm's search perseverance, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_PERSEVERANCE`](../../Reference/3dmod/M3dmodControl.md). The default value is 20.0. Increasing perseverance allows the algorithm to search the target scene more extensively and ensures multiple searches are performed to find a possible match.

When looking at your scene, you should generally first define scene complexity to reflect your objects of interest and then tweak perseverance to compensate for a poorly defined model (for example, it has multiple areas that look alike), a target that is complex, similar objects in the scene, the search not finding the expected number of occurrences, or when increasing the setting of [`M_SCENE_COMPLEXITY`](../../Reference/3dmod/M3dmodControl.md) is insufficient. Note that setting scene complexity to high and increasing perseverance results in a more robust operation, which might take more time.

If setting scene complexity and perseverance is not sufficient, you can enable an exhaustive search, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_EXHAUSTIVE_SEARCH`](../../Reference/3dmod/M3dmodControl.md). When exhaustive search is enabled, all possibilities are tried, and the perseverance setting is ignored.

## Find planar surface 3D model finder context

For a surface model in a find planar surface 3D model finder context, you can also specify the expected occlusion levels of the model and target planes.

### Model plane occlusion

If you expect one or more of the model's planar regions to be partially occluded at the position of an occurrence, you can set the model plane occlusion level, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_MODEL_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md). This is useful when there are overlapping objects in the scene and the model's complete planar regions are not visible, or if the model's planar regions are curved or warped in the scene. The default value is [`M_LOW`](../../Reference/3dmod/M3dmodControl.md). If less than 30% of a planar region is occluded in the scene, [`M_LOW`](../../Reference/3dmod/M3dmodControl.md) should be sufficient to find the occurrence. If the occlusion level is between 30% and 50%, between 50% and 80%, or above 80%, you should set [`M_MODEL_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_MEDIUM`](../../Reference/3dmod/M3dmodControl.md), [`M_HIGH`](../../Reference/3dmod/M3dmodControl.md), or [`M_VERY_HIGH`](../../Reference/3dmod/M3dmodControl.md), respectively. If different planar regions have differing occlusion levels, you should use the occlusion level for the highest occluded planar region.

### Target plane occlusion

If you expect objects in the scene to have larger planar regions than the model, you can set the target plane occlusion level, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_TARGET_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md). This is useful when an object in the scene has a large planar region and the model contains a portion of that planar region. The default value is [`M_LOW`](../../Reference/3dmod/M3dmodControl.md). If the model contains more than 70% of the objects planar region, [`M_LOW`](../../Reference/3dmod/M3dmodControl.md) should be sufficient to find the occurrence. If the model contains between 50% to 70%, between 20% to 50%, or below 20% of the object's planar region, you should set [`M_TARGET_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_MEDIUM`](../../Reference/3dmod/M3dmodControl.md), [`M_HIGH`](../../Reference/3dmod/M3dmodControl.md), or [`M_VERY_HIGH`](../../Reference/3dmod/M3dmodControl.md), respectively. The planar portion of the object included in the model must reach most of the boundary of the whole planar region. For example, your model can be the handle of trowel, but not a cropped section from the middle of the handle. Any artificial boundaries introduced by cropping must be minimal compared to the true boundary.
