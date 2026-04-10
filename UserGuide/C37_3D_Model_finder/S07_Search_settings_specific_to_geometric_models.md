---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Search_settings_specific_to_geometric_models
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Search settings specific to geometric models"
---

# Search settings specific to geometric models

Besides the search settings mentioned in [Customizing search settings](S06_Customizing_search_settings_general.md), there are search settings specific to geometric models.

> **Note:** Note that changing control type settings of a find 3D model finder context or model requires preprocessing the context again, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md).

## Sphere models

For sphere models, you can also specify the number of fit iterations.

### Maximum fit iterations

When searching a point cloud for sphere occurrences, you can set the maximum number of fit iterations to perform. Performing more iterations improves the accuracy of the fit, but will increase the search time. By default, Aurora Imaging Library performs one fit iteration, but you can call [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_FIT_ITERATIONS_MAX`](../../Reference/3dmod/M3dmodControl.md) to set the maximum number of fit iterations to any integer value. The default value of one fit iteration is often enough to find a sphere occurrence; however, if you need very accurate results, you should increase the maximum number of fit iterations to perform.

## Rectangular plane models

For rectangular plane models, you can also specify the normal and elongation of the rectangular plane. Rectangular planes are typically found throughout the scene, so customizing these settings can help to ensure that the correct rectangular plane occurrences are matched.

### Normals

Using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with the [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) control types, you can select the angle at which to find rectangular plane occurrences. You can restrict rectangular plane occurrences to only those whose normal satisfies the specified condition relative to a specified vector, within a certain tolerance. Set [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md) to the condition that rectangular plane occurrences must satisfy to be returned as a match. The default is [`M_UNCONDITIONAL`](../../Reference/3dmod/M3dmodControl.md), in which case there is no constraint on the normal of the rectangular plane. You can specify to only match rectangular plane occurrences whose normal is parallel ([`M_PARALLEL`](../../Reference/3dmod/M3dmodControl.md)), perpendicular ([`M_PERPENDICULAR`](../../Reference/3dmod/M3dmodControl.md)), or parallel or perpendicular ([`M_PARALLEL_OR_PERPENDICULAR`](../../Reference/3dmod/M3dmodControl.md)) to the vector ([`M_NORMAL_X`](../../Reference/3dmod/M3dmodControl.md), [`M_NORMAL_Y`](../../Reference/3dmod/M3dmodControl.md), [`M_NORMAL_Z`](../../Reference/3dmod/M3dmodControl.md)). In addition, you can also set an angular tolerance ([`M_NORMAL_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodControl.md)). Aurora Imaging Library only returns the occurrence as a match if the normal of the rectangular plane meets the specified [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md), +/- the specified angular tolerance, relative to the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md); the default is 10.0 degrees.*[Image: 3dmod_rectangular_plane_normal.png]*

### Elongation

When searching for rectangular plane occurrences that have a very large size range, you can set the minimum ([`M_ELONGATION_MIN`](../../Reference/3dmod/M3dmodControl.md)) and maximum ([`M_ELONGATION_MAX`](../../Reference/3dmod/M3dmodControl.md)) elongation of the rectangular plane occurrence using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md). The elongation is defined as the maximum side/minimum side of the rectangular plane occurrence, and it is used to prevent lines from being incorrectly identified as rectangular planes. By default, the minimum elongation is 1.0 and maximum elongation is infinitely long.

## Box models

For box models, you can also specify the normal and elongation of the box, the number of visible planes, the reference direction and completion sizes when a single box face is found, and the plane acceptance level, certainty level, and model coverage for individual faces. This section describes settings that you should change to ensure the correct box occurrences are found. It is important to customize these settings to suit your application.

### Number of visible planes

Depending on the location of the 3D sensor(s) acquiring a scene, certain box faces might not be visible. When searching for box occurrences, you can search for boxes with missing faces. Using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodControl.md) and [`M_NUMBER_OF_VISIBLE_FACES_MAX`](../../Reference/3dmod/M3dmodControl.md), you can specify the minimum and maximum number of visible faces (planes) required for a box occurrence to be considered a match, respectively. Note that the score will not be reduced unless the box occurrences don't have the specified number of visible faces.

If a box face is acquired from a 3D sensor that is perpendicular to this face, the other faces of the box are not visible. To find these box occurrences as well, set [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodControl.md) to 1. In this case, the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md)) and the completion angle tolerance ([`M_COMPLETION_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodControl.md)) are used to determine if the plane is a box occurrence. For information on reference direction, see[Reference direction](S07_Search_settings_specific_to_geometric_models.md). Note that when only one box face is found, and the completion angular tolerance is not met, the occurrence is not returned as a match.

After performing the [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operation, you can retrieve the number of visible faces (planes) that were used to fit the box using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_NUMBER_OF_VISIBLE_FACES`](../../Reference/3dmod/M3dmodGetResult.md).

### Reference direction

When [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodControl.md) is set to 1, you should set the reference direction. The reference direction is used to identify the location of the 3D sensor when it acquired the target point cloud. If a box face is acquired from a 3D sensor perpendicular to this face, the other faces of the box will not be visible. In other words, the reference direction should match the position of your 3D sensor or the direction of the 3D sensor's line of sight. To set the reference direction and the direction in which to extrude boxes, use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md). There are several ways to specify the direction mode:

- If the exact position of the 3D sensor is known and you want to extrude the box in the direction of the 3D sensor's line of sight, set [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md)to [`M_AWAY_FROM_POSITION`](../../Reference/3dmod/M3dmodControl.md), and then set [`M_DIRECTION_REFERENCE_X`](../../Reference/3dmod/M3dmodControl.md), [`M_DIRECTION_REFERENCE_Y`](../../Reference/3dmod/M3dmodControl.md), and [`M_DIRECTION_REFERENCE_Z`](../../Reference/3dmod/M3dmodControl.md) to the 3D sensor's position. *[Image: 3dmod_box_away_from_position.png]*
- If only the direction of the 3D sensor's line of sight is known and you want to extrude the box in this direction, set [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_TOWARDS_DIRECTION`](../../Reference/3dmod/M3dmodControl.md), and then set [`M_DIRECTION_REFERENCE_X`](../../Reference/3dmod/M3dmodControl.md), [`M_DIRECTION_REFERENCE_Y`](../../Reference/3dmod/M3dmodControl.md), and [`M_DIRECTION_REFERENCE_Z`](../../Reference/3dmod/M3dmodControl.md) to the components of the vector that points in this direction. *[Image: 3dmod_box_towards_direction.png]*
- If the exact position of the 3D sensor is known and you want to extrude the box against the direction of the 3D sensor's line of sight, set [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_TOWARDS_POSITION`](../../Reference/3dmod/M3dmodControl.md), and then set [`M_DIRECTION_REFERENCE_X`](../../Reference/3dmod/M3dmodControl.md), [`M_DIRECTION_REFERENCE_Y`](../../Reference/3dmod/M3dmodControl.md), and [`M_DIRECTION_REFERENCE_Z`](../../Reference/3dmod/M3dmodControl.md) to the 3D sensor's position. This direction mode is typically not used.*[Image: 3dmod_box_towards_position.png]*

### Completion size when a single box face is found

When [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodControl.md) is set to 1, a box can be extruded from a single plane. In this case, when only one box face (plane) is found and the completion angular tolerance is met, Aurora Imaging Library compares the plane's two dimensions (length and width) to the specified X, Y, and Z completion sizes ([`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md)); the most dissimilar size determines the missing dimension along which Aurora Imaging Library extrudes the visible face to complete the box.

*[Image: 3dmod_box_extrusion_dimension.png]*

Aurora Imaging Library can attempt to extrude the visible face to a background plane, a staircase plane, or the specified completion size for the missing dimension; use [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md), [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md), [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md), respectively, to enable extrusion to these.

When more than 1 completion method is enabled, Aurora Imaging Library will attempt to complete the box in the following order:

1. If [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) is enabled, the visible face can be extruded to a background plane to complete the box. Aurora Imaging Library will attempt to extrude the face to the closest background plane (includes planes found in the scene or copied to the find box 3D model finder context using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md)with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)) that intersects with the projection of the face and falls inside the size range for the missing dimension (for example, for range-type models, [`M_SIZE_..._MIN`](../../Reference/3dmod/M3dmodControl.md) to [`M_SIZE_..._MAX`](../../Reference/3dmod/M3dmodControl.md)). Note that the background plane must intersect with at least 80% of the projection of the face.
   If the closest background plane would yield an extrusion smaller than [`M_SIZE_..._MIN`](../../Reference/3dmod/M3dmodControl.md), the face cannot be extruded. In this case, no other completion methods will be attempted and the box is rejected. If no background plane intersects with the projection of the face, or the closest background plane is too far to complete the box, Aurora Imaging Library will attempt to complete the box using the next available completion method.
2. If [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md) is enabled, the visible face can be extruded to a staircase plane to complete the box. A staircase plane is any plane found in the scene or copied to the find box 3D model finder context using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md)with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md) whose edge touches the extrusion of the face (if the box were extruded to this plane, the two would seem like a staircase).
   *[Image: 3dmod_box_extrusion_staircase_plane.png]*
   Aurora Imaging Library will attempt to extrude the face to the staircase plane that yields the largest extrusion (farthest) within the specified size constraints of the model.
   If no staircase plane exists within the allowable distance, the next available completion method is used.
3. If [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md) is enabled, Aurora Imaging Library will extrude the face to the completion size of the missing dimension (most dissimilar) to complete the box.

Note that the completion sizes refer to the box model's dimensions and are not related to the axes of the working coordinate system.

*[Image: 3dmod_box_no_of_visible_planes.png]*

### Acceptance level, certainty level, and model coverage for individual faces

Besides setting the acceptance level, certainty level, and model coverage for the entire occurrence, you can specify these settings for the individual faces of a box occurrence:

- Plane acceptance level ([`M_PLANE_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md)): A plane will only be considered a visible face of the box occurrence if its score is greater than or equal to the defined plane acceptance level.
- Plane certainty level ([`M_PLANE_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md)): If the score of a plane is greater than or equal to the specified plane certainty level, the plane is considered a visible face of the box without searching for better faces.
- Plane maximum expected coverage ([`M_PLANE_MAX_COVERAGE`](../../Reference/3dmod/M3dmodControl.md)): A face (plane) with a coverage greater than or equal to the specified plane maximum expected coverage has a score of 100%.

To detect very occluded faces, you can set [`M_PLANE_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md), [`M_PLANE_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md), and [`M_PLANE_MAX_COVERAGE`](../../Reference/3dmod/M3dmodControl.md) to low values while maintaining high values for their equivalent whole box occurrence settings: [`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md), [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md), and [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

### Normals

Much like rectangular plane models, you can also set normal constraints for box models. A box occurrence is only considered a match if the normal of one of its faces meets the specified parallel condition ([`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md)) when compared to the vector specified with [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) (within the specified tolerance [`M_NORMAL_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodControl.md)). The default normal condition is [`M_UNCONDITIONAL`](../../Reference/3dmod/M3dmodControl.md). See the [Normals](S07_Search_settings_specific_to_geometric_models.md) subsubsection of the [Rectangular plane models](S07_Search_settings_specific_to_geometric_models.md) subsection to learn more.

For example, if your scene has boxes that sit flat on the floor (that is, axis-aligned with the XY-plane and parallel to the Z-axis) and boxes that are at an angle from the floor, you can set the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) to the positive Z-axis (0.0, 0.0, 1.0) and set the normal condition to [`M_PARALLEL`](../../Reference/3dmod/M3dmodControl.md) to only match box occurrences that sit on the floor.*[Image: 3dmod_box_normal_parallel.png]*

You can also specify to only match box occurrences if the normal of one of its visible faces is parallel ([`M_VISIBLE_FACE_PARALLEL`](../../Reference/3dmod/M3dmodControl.md)) to the vector. In other words, the face cannot be inferred. For example, if you set [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) to the positive Z-axis (0.0, 0.0, 1.0) and the face whose normal is parallel to the Z-axis is not visible, then the box occurrence will not be matched.*[Image: 3dmod_box_visible_face_normal_parallel.png]*

### Elongation

When searching for box occurrences that have a very large size range, you can set the minimum ([`M_ELONGATION_MIN`](../../Reference/3dmod/M3dmodControl.md)) and maximum ([`M_ELONGATION_MAX`](../../Reference/3dmod/M3dmodControl.md)) elongation of the box occurrence using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md). The elongation is defined as the maximum side/minimum side of any face of the box occurrence, and it is used to prevent lines from being incorrectly identified as boxes. By default, the minimum elongation is 1.0 and maximum elongation is infinitely long.
