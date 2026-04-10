---
doctype: Reference
module: 3dmod
function: M3dmodControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodControl"
---

# M3dmodControl

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Control a setting of a find 3D model finder context or result buffer.

## Syntax

```c
void M3dmodControl(
    AIL_ID     ContextOrResult3dmodId,  //out
    AIL_INT64  Index,                   //in
    AIL_INT64  ControlType,             //in
    AIL_DOUBLE ControlValue             //in
)
```

## Description

This function controls a specified setting of a find 3D model finder context or result buffer. These settings control the execution of [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). You can inquire about most of these settings using [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md).

> **Note:** Note that changing control type settings of a 3D model finder context requires preprocessing the context again, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md).

To control draw 3D model finder context settings, use [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) instead.

## Parameters

### `ContextOrResult3dmodId` *(out, AIL_ID)*

Specifies the identifier of the find 3D model finder context or result buffer to control. The find 3D model finder context or result buffer must have been previously allocated on the required system using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_..._CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) or [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md), respectively.

### `Index` *(in, AIL_INT64)*

Specifies what to control. Set this parameter to one of the following values:

*For specifying a general context, result buffer or individual model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If a find 3D model finder context is specified, same as [`M_CONTEXT`](../../Reference/3dmod/M3dmodControl.md).

If a find 3D model finder result buffer is specified, same as [`M_GENERAL`](../../Reference/3dmod/M3dmodControl.md). |
| `M_CONTEXT` | Specifies to control a setting of a specified find 3D model finder context. |
| `M_GENERAL` | Specifies to control a general setting of a specified find 3D model finder result buffer. |
| `0` | Specifies to control the 3D model defined in the context. Note that the model must have been added to the context, using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), prior to calling [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with this value. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For controlling a find box, cylinder, rectangular plane, sphere, surface, or planar surface 3D model finder context

The following [`ContextOrResult3dmodId`](../../Reference/3dmod/M3dmodControl.md), [`ControlType`](../../Reference/3dmod/M3dmodControl.md), and [`ControlValue`](../../Reference/3dmod/M3dmodControl.md) parameter settings can be specified for a find 3D model finder context when [`Index`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_CONTEXT`](../../Reference/3dmod/M3dmodControl.md) or [`M_DEFAULT`](../../Reference/3dmod/M3dmodControl.md).

---

### `Find box 3D model finder context ID`

Specifies a find box 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_BOX_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_DIRECTION_MODE`

Sets how to interpret the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md)). The reference direction typically matches the position of your 3D sensor or the direction of the 3D sensor's line of sight.  When you know the exact position of the 3D sensor, set this control type to [`M_AWAY_FROM_POSITION`](../../Reference/3dmod/M3dmodControl.md) or [`M_TOWARDS_POSITION`](../../Reference/3dmod/M3dmodControl.md), and set [`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md) to the 3D sensor's position. When you only know the direction of the 3D sensor's line of sight, set this control type to [`M_TOWARDS_DIRECTION`](../../Reference/3dmod/M3dmodControl.md), and set [`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md) to the components of the vector that points in this direction.  The reference direction establishes when a single plane should be considered a box occurrence (when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1), given the line of sight of the 3D sensor and the specified completion tolerance ([`M_COMPLETION_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodInquire.md)). [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md) specifies how to interpret the reference direction and the direction in which the box should be extruded (typically, [`M_AWAY_FROM_POSITION`](../../Reference/3dmod/M3dmodControl.md) or [`M_TOWARDS_DIRECTION`](../../Reference/3dmod/M3dmodControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AWAY_FROM_POSITION` | Specifies to interpret the reference direction as away from the position specified using [`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md). |
| `M_TOWARDS_DIRECTION` *(default)* | Specifies to interpret the reference direction as towards the direction specified using[`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md). |
| `M_TOWARDS_POSITION` | Specifies to interpret the reference direction as towards the position specified using[`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md). |

#### `M_DIRECTION_REFERENCE_X`

Sets the X-coordinate of the position, or the X-component of the vector, that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md). The reference direction is used to identify the location of the 3D sensor when it acquired the target point cloud, so the reference direction should match the position of your 3D sensor or the direction of the 3D sensor's line of sight.  For a find box 3D model finder context, this control type is only used when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate of the position or the X-component of the vector. |

#### `M_DIRECTION_REFERENCE_Y`

Sets the Y-coordinate of the position, or the Y-component of the vector, that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md). The reference direction is used to identify the location of the 3D sensor when it acquired the target point cloud, so the reference direction should match the position of your 3D sensor or the direction of the 3D sensor's line of sight.  For a find box 3D model finder context, this control type is only used when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate of the position or the Y-component of the vector. |

#### `M_DIRECTION_REFERENCE_Z`

Sets the Z-coordinate of the position, or the Z-component of the vector, that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md). The reference direction is used to identify the location of the 3D sensor when it acquired the target point cloud, so the reference direction should match the position of your 3D sensor or the direction of the 3D sensor's line of sight.  For a find box 3D model finder context, this control type is only used when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-coordinate of the position or the Z-component of the vector. |

#### `M_FIT_DISTANCE`

Sets the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodControl.md). The fit distance is used when determining whether points belong to a certain occurrence. Typically, to set an appropriate value, use the automatic fit distance as a baseline and adjust accordingly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the fit distance. |

#### `M_FIT_DISTANCE_MODE`

Sets how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the fit distance. |
| `M_USER_DEFINED` | Specifies to use the value set with[`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)as the fit distance. |

#### `M_SORT`

Sets the sorting key for result retrieval. This is useful, for example, to retrieve the results of the highest occurrence first (the one with the highest Z-coordinate), instead of retrieving those with the highest score first.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AREA` | Specifies to sort results by the area of the occurrence. |
| `M_CENTER_X` | Specifies to sort the results by the X-coordinate of the occurrence's center point. |
| `M_CENTER_Y` | Specifies to sort the results by the Y-coordinate of the occurrence's center point. |
| `M_CENTER_Z` | Specifies to sort the results by the Z-coordinate of the occurrence's center point. |
| `M_MAX_X` | Specifies to sort the results by the maximum X-coordinate of the occurrence. |
| `M_MAX_Y` | Specifies to sort the results by the maximum Y-coordinate of the occurrence. |
| `M_MAX_Z` | Specifies to sort the results by the maximum Z-coordinate of the occurrence. |
| `M_MIN_X` | Specifies to sort the results by the minimum X-coordinate of the occurrence. |
| `M_MIN_Y` | Specifies to sort the results by the minimum Y-coordinate of the occurrence. |
| `M_MIN_Z` | Specifies to sort the results by the minimum Z-coordinate of the occurrence. |
| `M_NUMBER_OF_POINTS` | Specifies to sort the results by the number of points in the occurrence. |
| `M_SCORE` *(default)* | Specifies to sort the results by the score. |
| `M_SCORE_TARGET` | Specifies to sort results by the target score. |
| `M_VOLUME` | Specifies to sort results by the volume of the occurrence. |

#### `M_SORT_DIRECTION`

Sets whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` *(default)* | Specifies to sort the results in descending order. |
| `M_SORT_UP` | Specifies to sort the results in ascending order. |

#### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

---

### `Find cylinder 3D model finder context ID`

Specifies a find cylinder 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_CYLINDER_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_FIT_DISTANCE`

Sets the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodControl.md). The fit distance is used when determining whether points belong to a certain occurrence. Typically, to set an appropriate value, use the automatic fit distance as a baseline and adjust accordingly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the fit distance. |

#### `M_FIT_DISTANCE_MODE`

Sets how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the fit distance. |
| `M_USER_DEFINED` | Specifies to use the value set with[`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)as the fit distance. |

#### `M_FIT_NORMALS_DISTANCE`

Sets the acceptable deviation between a given point's normal vector and the normal vector of the model at the same point, to decide which points are used when fitting. Model points whose normal vector falls outside the specified deviation are not used to perform the occurrence's fit operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 90.0` *(default)* | Specifies the acceptable deviation from the normal vector of the model at the same point, in degrees. |

#### `M_PERSEVERANCE`

Sets the algorithm's search perseverance when searching for occurrences. This affects the number of times the algorithm tries to find occurrences before giving up. Increasing the perseverance will increase robustness and accuracy, but will increase search time.  You should try increasing the perseverance if the search is not finding the specified number of occurrences or if the target is complex.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the algorithm's search perseverance, as a percentage. |

#### `M_SORT`

Sets the sorting key for result retrieval. This is useful, for example, to retrieve the results of the highest occurrence first (the one with the highest Z-coordinate), instead of retrieving those with the highest score first.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AREA` | Specifies to sort results by the area of the occurrence. |
| `M_CENTER_X` | Specifies to sort the results by the X-coordinate of the occurrence's center point. The center point of the cylinder occurrence is the center point on the cylinder's central axis. |
| `M_CENTER_Y` | Specifies to sort the results by the Y-coordinate of the occurrence's center point. The center point of the cylinder occurrence is the center point on the cylinder's central axis. |
| `M_CENTER_Z` | Specifies to sort the results by the Z-coordinate of the occurrence's center point. The center point of the cylinder occurrence is the center point on the cylinder's central axis. |
| `M_MAX_X` | Specifies to sort the results by the maximum X-coordinate of the occurrence. |
| `M_MAX_Y` | Specifies to sort the results by the maximum Y-coordinate of the occurrence. |
| `M_MAX_Z` | Specifies to sort the results by the maximum Z-coordinate of the occurrence. |
| `M_MIN_X` | Specifies to sort the results by the minimum X-coordinate of the occurrence. |
| `M_MIN_Y` | Specifies to sort the results by the minimum Y-coordinate of the occurrence. |
| `M_MIN_Z` | Specifies to sort the results by the minimum Z-coordinate of the occurrence. |
| `M_NUMBER_OF_POINTS` | Specifies to sort the results by the number of points in the occurrence. |
| `M_SCORE` *(default)* | Specifies to sort the results by the score. |
| `M_SCORE_TARGET` | Specifies to sort results by the target score. |
| `M_VOLUME` | Specifies to sort results by the volume of the occurrence. |

#### `M_SORT_DIRECTION`

Sets whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` *(default)* | Specifies to sort the results in descending order. |
| `M_SORT_UP` | Specifies to sort the results in ascending order. |

#### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

---

### `Find rectangular plane 3D model finder context ID`

Specifies a find rectangular plane 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_RECTANGULAR_PLANE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_FIT_DISTANCE`

Sets the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodControl.md). The fit distance is used when determining whether points belong to a certain occurrence. Typically, to set an appropriate value, use the automatic fit distance as a baseline and adjust accordingly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the fit distance. |

#### `M_FIT_DISTANCE_MODE`

Sets how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the fit distance. |
| `M_USER_DEFINED` | Specifies to use the value set with[`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)as the fit distance. |

#### `M_SORT`

Sets the sorting key for result retrieval. This is useful, for example, to retrieve the results of the highest occurrence first (the one with the highest Z-coordinate), instead of retrieving those with the highest score first.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AREA` | Specifies to sort results by the area of the occurrence. |
| `M_CENTER_X` | Specifies to sort the results by the X-coordinate of the occurrence's center point. The center point of the rectangular plane occurrence is the center point of the rectangular plane's bounding box. |
| `M_CENTER_Y` | Specifies to sort the results by the Y-coordinate of the occurrence's center point. The center point of the rectangular plane occurrence is the center point of the rectangular plane's bounding box. |
| `M_CENTER_Z` | Specifies to sort the results by the Z-coordinate of the occurrence's center point. The center point of the rectangular plane occurrence is the center point of the rectangular plane's bounding box. |
| `M_MAX_X` | Specifies to sort the results by the maximum X-coordinate of the occurrence. |
| `M_MAX_Y` | Specifies to sort the results by the maximum Y-coordinate of the occurrence. |
| `M_MAX_Z` | Specifies to sort the results by the maximum Z-coordinate of the occurrence. |
| `M_MIN_X` | Specifies to sort the results by the minimum X-coordinate of the occurrence. |
| `M_MIN_Y` | Specifies to sort the results by the minimum Y-coordinate of the occurrence. |
| `M_MIN_Z` | Specifies to sort the results by the minimum Z-coordinate of the occurrence. |
| `M_NUMBER_OF_POINTS` | Specifies to sort the results by the number of points in the occurrence. |
| `M_SCORE` *(default)* | Specifies to sort the results by the score. |
| `M_SCORE_TARGET` | Specifies to sort results by the target score. |

#### `M_SORT_DIRECTION`

Sets whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` *(default)* | Specifies to sort the results in descending order. |
| `M_SORT_UP` | Specifies to sort the results in ascending order. |

#### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

---

### `Find sphere 3D model finder context ID`

Specifies a find sphere 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SPHERE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_FIT_DISTANCE`

Sets the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodControl.md). The fit distance is used when determining whether points belong to a certain occurrence. Typically, to set an appropriate value, use the automatic fit distance as a baseline and adjust accordingly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the fit distance. |

#### `M_FIT_DISTANCE_MODE`

Sets how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the fit distance. |
| `M_USER_DEFINED` | Specifies to use the value set with[`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)as the fit distance. |

#### `M_FIT_ITERATIONS_MAX`

Sets the maximum number of fit iterations to perform, when finding a sphere occurrence. More iterations increase fit accuracy, but increase search times. For a very tight tolerance, you can use a wider range with an initial search, and then repeat using a tighter range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the maximum number of sphere fit iterations. |

#### `M_FIT_NORMALS_DISTANCE`

Sets the acceptable deviation between a given point's normal vector and the normal vector of the model at the same point, to decide which points are used when fitting. Model points whose normal vector falls outside the specified deviation are not used to perform the occurrence's fit operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 90.0` *(default)* | Specifies the acceptable deviation from the normal vector of the model at the same point, in degrees. |

#### `M_PERSEVERANCE`

Sets the algorithm's search perseverance when searching for occurrences. This affects the number of times the algorithm tries to find occurrences before giving up. Increasing the perseverance will increase robustness and accuracy, but will increase search time.  You should try increasing the perseverance if the search is not finding the specified number of occurrences or if the target is complex.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the algorithm's search perseverance, as a percentage. |

#### `M_SORT`

Sets the sorting key for result retrieval. This is useful, for example, to retrieve the results of the highest occurrence first (the one with the highest Z-coordinate), instead of retrieving those with the highest score first.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AREA` | Specifies to sort results by the area of the occurrence. |
| `M_CENTER_X` | Specifies to sort the results by the X-coordinate of the occurrence's center point. |
| `M_CENTER_Y` | Specifies to sort the results by the Y-coordinate of the occurrence's center point. |
| `M_CENTER_Z` | Specifies to sort the results by the Z-coordinate of the occurrence's center point. |
| `M_MAX_X` | Specifies to sort the results by the maximum X-coordinate of the occurrence. |
| `M_MAX_Y` | Specifies to sort the results by the maximum Y-coordinate of the occurrence. |
| `M_MAX_Z` | Specifies to sort the results by the maximum Z-coordinate of the occurrence. |
| `M_MIN_X` | Specifies to sort the results by the minimum X-coordinate of the occurrence. |
| `M_MIN_Y` | Specifies to sort the results by the minimum Y-coordinate of the occurrence. |
| `M_MIN_Z` | Specifies to sort the results by the minimum Z-coordinate of the occurrence. |
| `M_NUMBER_OF_POINTS` | Specifies to sort the results by the number of points in the occurrence. |
| `M_SCORE` *(default)* | Specifies to sort the results by the score. |
| `M_SCORE_TARGET` | Specifies to sort results by the target score. |
| `M_VOLUME` | Specifies to sort results by the volume of the occurrence. |

#### `M_SORT_DIRECTION`

Sets whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` *(default)* | Specifies to sort the results in descending order. |
| `M_SORT_UP` | Specifies to sort the results in ascending order. |

#### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

---

### `Find surface or planar surface 3D model finder context ID`

Specifies a find surface or planar surface 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) or [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_CONVERSION_GAMMA`

Sets whether to remove gamma correction before the match. Gamma correction refers to color data that has been processed, typically during data acquisition (that is, by the camera doing the grab), to compensate for a non-linear transformation between a point's component value and its displayed intensity.  If gamma correction has been applied on the RGB source color data, you must remove it.  Note that this control type is only used and should only be enabled when [`M_USE_COLOR`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to remove gamma correction. In this case, your RGB color data should not be in a corrected state (it should be linear). For example, the camera capturing the RGB color data has not applied a gamma correction. |
| `M_ENABLE` | Specifies to remove gamma correction. In this case, your RGB color data should be in a corrected state (it should be non-linear). For example, the camera capturing the RGB color data has applied a gamma correction. |

#### `M_DIRECTION_MODE`

Sets how to interpret the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md)). The reference direction typically matches the direction of the 3D sensor's line of sight.  Set this control type to [`M_TOWARDS_DIRECTION`](../../Reference/3dmod/M3dmodControl.md), and set [`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md) to the components of the vector that points in this direction.  For a find surface or planar surface 3D model finder context, this control type establishes which part of the model would be occluded at the position of an occurrence when calculating the score and [`M_SCENE_PROJECTION`](../../Reference/3dmod/M3dmodControl.md) is enabled, given the line of sight of 3D sensor. [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md) specifies how to interpret the reference direction and the direction of projection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_TOWARDS_DIRECTION` *(default)* | Specifies to interpret the reference direction as towards the direction specified using[`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md). |

#### `M_DIRECTION_REFERENCE_X`

Sets the X-component of the vector that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md). The reference direction is used to identify the location of the 3D sensor when it acquired the target point cloud, so the reference direction should match the direction of the 3D sensor's line of sight.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-component of the vector. |

#### `M_DIRECTION_REFERENCE_Y`

Sets the Y-component of the vector that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md). The reference direction is used to identify the location of the 3D sensor when it acquired the target point cloud, so the reference direction should match the direction of the 3D sensor's line of sight.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-component of the vector. |

#### `M_DIRECTION_REFERENCE_Z`

Sets the Z-component of the vector that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodControl.md). The reference direction is used to identify the location of the 3D sensor when it acquired the target point cloud, so the reference direction should match the direction of the 3D sensor's line of sight.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-component of the vector. |

#### `M_EXHAUSTIVE_SEARCH`

Sets whether to perform very robust but slow exhaustive searching. When exhaustive searching is enabled, all possibilities are tried, and[`M_PERSEVERANCE`](../../Reference/3dmod/M3dmodControl.md)is ignored.  Note that this control type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable exhaustive searching. |
| `M_ENABLE` | Specifies to enable exhaustive searching. |

#### `M_FIT_DISTANCE`

Sets the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodControl.md). The fit distance is used when determining whether points belong to a certain occurrence. Typically, to set an appropriate value, use the automatic fit distance as a baseline and adjust accordingly.  Note that for a find surface or planar surface 3D model finder context, this control type is only used when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodControl.md) and [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the fit distance. |

#### `M_FIT_DISTANCE_MODE`

Sets how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.  > **Note:** Note that for a find surface or planar surface 3D model finder context, this control type is only used when [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) is enabled. In this case, [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) and, if applicable, [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md) only determine the fit distance when calculating the RMS error and the fit score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the fit distance. |
| `M_USER_DEFINED` | Specifies to use the value set with[`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)as the fit distance. |

#### `M_MODEL_NORMAL_SEARCH_MODE`

Sets the search mode for calculating model normals when the defined model has no normals component ([`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For a find surface 3D model finder context, the default value is [`M_AUTO`](../../Reference/3dmod/M3dmodControl.md).  For a find planar surface 3D model finder context, the default value is [`M_TREE`](../../Reference/3dmod/M3dmodControl.md). |
| `M_AUTO` | Specifies to automatically set the search mode based on the point cloud's organization; either [`M_ORGANIZED`](../../Reference/3dmod/M3dmodControl.md) if organized or [`M_TREE`](../../Reference/3dmod/M3dmodControl.md) if unorganized. |
| `M_ORGANIZED` | Specifies to use the point cloud's organizational structure to determine the model normals. This option is supported only for an organized point cloud. |
| `M_TREE` | Specifies to use a KD tree search mode to determine the model normals. |

#### `M_PERSEVERANCE`

Sets the algorithm's search perseverance when searching for occurrences. This affects the number of times the algorithm tries to find occurrences before giving up. Increasing the perseverance will increase robustness and accuracy, but will increase search time.  You should try increasing the perseverance if the search is not finding the specified number of occurrences or if the target is complex and increasing the setting of [`M_SCENE_COMPLEXITY`](../../Reference/3dmod/M3dmodControl.md) is insufficient. You should also try increasing perseverance if the model is not well-defined (for example, it has multiple areas that look alike).  Note that this control type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the algorithm's search perseverance, as a percentage. |

#### `M_REFINE_REGISTRATION`

Sets whether and how to perform refine registration of each occurrence found. By default, for a surface model, Aurora Imaging Library uses the features of the model and target to find occurrences, but it doesn't do any fitting. To get more accurate score and pose information, you should enable refine registration. When enabled, 3D registration is internally performed between the model and each of the found occurrences. You can set [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md)to use a predefined or a custom registration context.  > **Note:** Note that enabling refine registration increases processing time.  To calculate and retrieve the root-mean square (RMS) error and fit score of the occurrences, enable [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable refine registration. |
| `M_FIND_SURFACE_REFINEMENT_FAST` | Specifies to use a predefined registration context that prioritizes performance to fine-tune results. |
| `M_FIND_SURFACE_REFINEMENT_PRECISE` | Specifies to use a predefined registration context that prioritizes accuracy to fine-tune results. |
| `M_USER_DEFINED_REGISTRATION` | Specifies to use a custom registration context. Inquire the identifier of the internal registration context using [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with[`M_USER_DEFINED_REGISTRATION_CONTEXT_ID`](../../Reference/3dmod/M3dmodInquire.md), and then use [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) to configure the context. You can also use [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodCopy.md) to initialize the internal context with the settings of a predefined registration context, and then adjust a few settings using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md). |

#### `M_REMOVE_BACKGROUND`

Sets whether to remove background points from the target point cloud before performing the match. Background points include those of objects that are too big or too small to be part of an occurrence of the model. You should enable[`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md)for scenes that are very complex.  > **Note:** Note that removing the background takes some time, but sometimes makes the match faster.  You can use [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to draw the points that were considered background points ([`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_BACKGROUND_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md)).  Note that [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) does not currently use the floor plane nor the resting plane ([`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md) or [`M_RESTING_PLANE`](../../Reference/3dmod/M3dmodCopy.md)) to establish which points to remove. You can remove floor points, using [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to remove background points. |
| `M_ENABLE` | Specifies to remove background points. |

#### `M_REMOVE_BACKGROUND_SEARCH_MODE`

Sets the search mode for finding the background points when [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically set the search mode based on the organization of the point clouds; either [`M_ORGANIZED`](../../Reference/3dmod/M3dmodControl.md) if organized or [`M_TREE`](../../Reference/3dmod/M3dmodControl.md) if unorganized. |
| `M_ORGANIZED` | Specifies to use the organizational structure of the point clouds to determine the background points. This option is supported only if the model and target point clouds are organized. |
| `M_TREE` | Specifies to use a KD tree search mode to determine the background points. |

#### `M_REMOVE_FLOOR`

Sets whether to remove floor points from the target point cloud before performing the match. Points located either above or below ([`M_REMOVE_FLOOR_DIRECTION`](../../Reference/3dmod/M3dmodControl.md)) the floor plane ([`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)), that fall within the specified distance ([`M_REMOVE_FLOOR_OFFSET`](../../Reference/3dmod/M3dmodControl.md)), are removed when this control type is enabled.  > **Note:** Note that removing the floor points takes some time, but sometimes makes the match faster.  You can use [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to draw the points that were considered floor points ([`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_FLOOR_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to remove floor points. |
| `M_ENABLE` | Specifies to remove floor points. |

#### `M_REMOVE_FLOOR_DIRECTION`

Sets the direction, relative to the defined floor plane, in which to remove points when [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) is enabled. Use [`M_REMOVE_FLOOR_OFFSET`](../../Reference/3dmod/M3dmodControl.md) to set the distance from the floor plane within which to remove points, or to have Aurora Imaging Library automatically establish the removal distance.  You can use [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to draw the points that were considered floor points ([`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_FLOOR_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABOVE` | Specifies to remove points above the floor plane. |
| `M_AUTO` *(default)* | Specifies to remove points in the direction with the least number of valid points. The points on the side of the floor with the larger number of valid points are assumed to belong to actual objects, not noise. |
| `M_BELOW` | Specifies to remove points below the floor plane. |

#### `M_REMOVE_FLOOR_EXPECTED_PERCENTAGE`

Sets the percentage of total points in the target point cloud that are expected to be floor points when [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) is enabled and [`M_REMOVE_FLOOR_OFFSET`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_AUTO_VALUE`](../../Reference/3dmod/M3dmodControl.md). Aurora Imaging Library uses the expected percentage when automatically determining the distance from the floor plane within which to remove points. Use [`M_REMOVE_FLOOR_DIRECTION`](../../Reference/3dmod/M3dmodControl.md) to set whether to remove points above or below the floor plane, or to have Aurora Imaging Library automatically establish the removal direction.  You can use [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to draw the points that were considered floor points ([`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_FLOOR_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `5.0 <= Value < 100.0` *(default)* | Specifies the expected floor points, as a percentage. |

#### `M_REMOVE_FLOOR_OFFSET`

Sets the offset, relative to the defined floor plane, within which to remove points when [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) is enabled. Use [`M_REMOVE_FLOOR_DIRECTION`](../../Reference/3dmod/M3dmodControl.md) to set whether to remove points above or below the floor plane, or to have Aurora Imaging Library automatically establish the removal direction.  You can use [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to draw the points that were considered floor points ([`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_FLOOR_POINTS`](../../Reference/3dmod/M3dmodControlDraw.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_VALUE` *(default)* | Specifies to automatically determine the offset within which to remove points. |
| `Value >= 0.0` | Specifies the offset within which to remove points. |

#### `M_REUSE_RESULT`

Sets whether to reuse the result of the previous search at the beginning of the current search. If you expect occurrences to be found in similar locations to the occurrences found in a previous search, you can enable this control and pass the result buffer containing the results to [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to speed up the operation. The algorithm will search for occurrences at the previously found positions, and then resume with the search if more occurrences are expected.  Note that if you want to reuse the same results for multiple searches, you must avoid passing that result buffer to [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) again, so as not to change the stored results. You should set up two result buffers: one for the initial search and another for use with subsequent searches. Before each subsequent search, copy into its result buffer the results from the initial search, using [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_RESULT`](../../Reference/3dmod/M3dmodCopyResult.md), and use this result buffer for the search.  You can use [`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md) to modify or delete results before performing the next search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to reuse the previous result. |
| `M_ENABLE` | Specifies to reuse the previous result. |

#### `M_SAVE_FIT_INFO`

Sets whether to enable the calculation of the root-mean square (RMS) error and the fit score. When enabled, besides being able to obtain these results, you can filter out occurrences based on the minimum required fit score ([`M_FIT_SCORE_MIN`](../../Reference/3dmod/M3dmodControl.md)) and the maximum fit distance ([`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) and [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)).  Note that [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) and its related settings have no effect on [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md), and vice versa. Even if refine registration is enabled, you must enable [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) to calculate and retrieve the root-mean square (RMS) error and the fit score; when calculating these, [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodControl.md) and [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md) are used, and not the settings of the refine registration.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the calculation of the root-mean square (RMS) error and the fit score. |
| `M_ENABLE` | Specifies to enable the calculation of the root-mean square (RMS) error and the fit score. |

#### `M_SCENE_COMPLEXITY`

Sets the complexity of the scene. It sets how much of the target data belongs to the occurrence. For example, if the target scene contains one occurrence of the model on a simple background, setting [`M_SCENE_COMPLEXITY`](../../Reference/3dmod/M3dmodControl.md) to [`M_LOW`](../../Reference/3dmod/M3dmodControl.md) should be sufficient to find the occurrence. Whereas, if the target scene is busy and contains many similar features as the model, you should set [`M_SCENE_COMPLEXITY`](../../Reference/3dmod/M3dmodControl.md) to [`M_MEDIUM`](../../Reference/3dmod/M3dmodControl.md) or [`M_HIGH`](../../Reference/3dmod/M3dmodControl.md). Higher settings result in a more robust operation, but typically take more time.  Note that this control type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies that the scene has a high complexity. |
| `M_LOW` | Specifies that the scene has a low complexity. |
| `M_MEDIUM` *(default)* | Specifies that the scene has a medium complexity. |

#### `M_SCENE_PROJECTION`

Sets whether to apply occlusion handling to the model. This discards points of the model that would be occluded at the position of an occurrence, given the line of sight of the 3D sensor, before comparing the model to the occurrence during the refine registration and score calculation. This prevents the occurrences' scores from being lowered due to how the scene was acquired. Aurora Imaging Library uses the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md)) to determine the line of sight and direction of projection. The default (0,0,1), provides a projection along the 3D sensor's Z-axis.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable occlusion handling. |
| `M_ENABLE` | Specifies to enable occlusion handling. |

#### `M_SCORE_MODE`

Sets the mode with which to associate model and target points for score calculations. The score mode affects the model score, target score, and color score.  Note that points are associated using a positional comparison between model and target points, relative to local volumes within a partitioned 3D space.  When using the [`M_STRICT`](../../Reference/3dmod/M3dmodControl.md) mode, a point is either fully associated with another point or not associated at all. For example, when calculating the model score, a model point is considered fully covered if there is a target point in the same volume, and uncovered if not.  When using the [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md) mode, points can be partially associated. For example, when calculating the model score, if a model point's position is close to the boundary of a volume adjacent to the one it occupies and there is a target point in either of the volumes, the point is considered half covered; there must be target points in both volumes to fully cover the model point.  Note that the [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md) mode does not always produce higher scores than the [`M_STRICT`](../../Reference/3dmod/M3dmodControl.md) mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For a find surface 3D model finder context, the default value is [`M_STRICT`](../../Reference/3dmod/M3dmodControl.md).  For a find planar surface 3D model finder context, the default value is [`M_FLEXIBLE`](../../Reference/3dmod/M3dmodControl.md). |
| `M_FLEXIBLE` | Specifies to use the flexible mode when calculating the scores. |
| `M_STRICT` | Specifies to use the strict mode when calculating the scores. |

#### `M_SORT`

Sets the sorting key for result retrieval. This is useful, for example, to retrieve the results of the highest occurrence first (the one with the highest Z-coordinate), instead of retrieving those with the highest score first.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER_X` | Specifies to sort the results by the X-coordinate of the occurrence's center point. The center point of the surface occurrence is the center of the surface's bounding box. |
| `M_CENTER_Y` | Specifies to sort the results by the Y-coordinate of the occurrence's center point. The center point of the surface occurrence is the center of the surface's bounding box. |
| `M_CENTER_Z` | Specifies to sort the results by the Z-coordinate of the occurrence's center point. The center point of the surface occurrence is the center of the surface's bounding box. |
| `M_MAX_X` | Specifies to sort the results by the maximum X-coordinate of the occurrence. |
| `M_MAX_Y` | Specifies to sort the results by the maximum Y-coordinate of the occurrence. |
| `M_MAX_Z` | Specifies to sort the results by the maximum Z-coordinate of the occurrence. |
| `M_MIN_X` | Specifies to sort the results by the minimum X-coordinate of the occurrence. |
| `M_MIN_Y` | Specifies to sort the results by the minimum Y-coordinate of the occurrence. |
| `M_MIN_Z` | Specifies to sort the results by the minimum Z-coordinate of the occurrence. |
| `M_NO_SORT` | Specifies not to sort the results; found occurrences are returned in an arbitrary order. |
| `M_NUMBER_OF_POINTS` | Specifies to sort the results by the number of points in the occurrence.  Note that this control value is only used when [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md); otherwise, found occurrences are returned in an arbitrary order. |
| `M_SCORE` *(default)* | Specifies to sort the results by the score. |
| `M_SCORE_COLOR` | Specifies to sort results by the color score.  Note that this control value is only used when [`M_USE_COLOR`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md); otherwise, found occurrences are returned in an arbitrary order. |
| `M_SCORE_FIT` | Specifies to sort results by the fit score.  Note that this control value is only used when [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md); otherwise, found occurrences are returned in an arbitrary order. |
| `M_SCORE_GRIP_BEST` | Specifies to sort results by the best grip score.  Note that this control value is only used if a grip label image that contains non-zero values was copied to the find surface 3D model finder context, using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_GRIP_LABEL_IMAGE`](../../Reference/3dmod/M3dmodCopy.md); otherwise, found occurrences are returned in an arbitrary order. |
| `M_SCORE_TARGET` | Specifies to sort results by the target score. |

#### `M_SORT_DIRECTION`

Sets whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` *(default)* | Specifies to sort the results in descending order. |
| `M_SORT_UP` | Specifies to sort the results in ascending order. |

#### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

#### `M_USE_COLOR`

Sets whether to use the color information of the model and scene when searching for surface occurrences. This is useful when the model is similar in shape to other background objects in the scene, but has a different color distribution.  When enabled, an occurrence will be returned only if its color score is greater than or equal to the color score acceptance level ([`M_ACCEPTANCE_COLOR`](../../Reference/3dmod/M3dmodControl.md)).  The color data is assumed to be in RGB format and stored in either the reflectance or intensity component of the respective point cloud container. The component must be a 3-band 8-bit unsigned buffer and have the same dimensions as the range component. The reflectance component is used if a valid reflectance component exists in the container. If a valid reflectance component does not exist and the container has a valid intensity component, the intensity component is used.  Note that if gamma correction has been applied on the RGB source color data and you enable this control type, you must remove it, using [`M_CONVERSION_GAMMA`](../../Reference/3dmod/M3dmodControl.md) set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use the color information. |
| `M_ENABLE` | Specifies to use the color information. |

### For controlling a box, cylinder, rectangular plane, sphere, or surface 3D model

The following [`ContextOrResult3dmodId`](../../Reference/3dmod/M3dmodControl.md), [`ControlType`](../../Reference/3dmod/M3dmodControl.md), and [`ControlValue`](../../Reference/3dmod/M3dmodControl.md) parameter settings can be specified for a box, cylinder, rectangular plane, sphere, or surface 3D model when [`Index`](../../Reference/3dmod/M3dmodControl.md) is set to [`0`](../../Reference/3dmod/M3dmodControl.md).

---

### `Find box 3D model finder context ID with a box model`

Specifies a find box 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_BOX_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a box model.

#### `M_ACCEPTANCE`

Sets the acceptance level for the score. An occurrence will be returned only if the match score between the target and the model is greater than or equal to this level. The score is a measure, as a percentage, of the model's surface that is covered by inlier points at the location of the occurrence. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. Set the maximum expected coverage with[`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

#### `M_BOX_FACE_PARALLELISM_THRESHOLD`

Sets the maximum shear angle allowed between adjacent rectangles for them to be considered faces of the same box occurrence.  *[Image: box_face_parallelism_threshold_03.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 45.0` *(default)* | Specifies the maximum angle, in degrees. |

#### `M_BOX_FACE_PERPENDICULARITY_THRESHOLD`

Sets the maximum deviation from 90 degrees allowed between adjacent rectangles for them to be considered faces of the same box occurrence.  *[Image: box_face_perpendicularity_threshold.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 90.0` *(default)* | Specifies the maximum angle, in degrees. |

#### `M_CERTAINTY`

Sets the certainty level for the score, as a percentage. If the score is greater than or equal to the specified certainty level, the occurrence is considered a match, without searching the rest of the target for better matches (provided the specified number of occurrences has been found). The certainty level is defined relative to the maximum expected model coverage, such that setting [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) to 100% indicates that certain matches must have a total model coverage equal to the maximum expected model coverage.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the score, as a percentage. If you set the certainty level too high (close to 100.0%), you slow down the search because you force the search algorithm to check the whole point cloud for the best possible match(es). A good certainty level is slightly lower than the expected score, so that the search can finish as soon as a match is found. However, if you set the certainty level too low, false matches might be found. |

#### `M_COMPLETION_ANGLE_TOLERANCE`

Sets the completion angular tolerance, when only one box face (plane) is found. Aurora Imaging Library only extrudes the face if its normal is less than the specified number of degrees away from the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md)). This control type is used only when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.  Note that when only one box face is found, and the completion angular tolerance is not met, the occurrence is not returned as a match.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 90.0` *(default)* | Specifies the completion angular tolerance, when only one box face (plane) is found. |

#### `M_COMPLETION_SIZE_X`

Sets the length along X to use to establish the missing dimension, when only one box face (plane) is found. Aurora Imaging Library compares the plane's two dimensions (length and width) to the specified completion sizes ([`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md)). The most dissimilar size determines the missing dimension. If [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md) is enabled, the [`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md) of the missing dimension will be the length to which Aurora Imaging Library extrudes the visible face to complete the box, unless [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) and/or [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md) are also enabled.  If [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) and [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md) are enabled, Aurora Imaging Library will attempt to extrude the visible face to the background plane and, if not possible, then to an edge of a staircase plane before using the completion size to complete the box. The completion methods are attempted in the following order:  1. [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md). 2. [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md). 3. [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md).  Note that the completion sizes refer to the box model's dimensions and are not related to the axes of the working coordinate system.  This control type is used only when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAME_AS_NOMINAL` *(default)* | Specifies that the length along X, to use to establish the missing dimension, is the same as the nominal length in X of a nominal model. When searching for a range-type model, this corresponds to the middle of the specified size range ([`M_SIZE_X_MAX`](../../Reference/3dmod/M3dmodInquire.md) - [`M_SIZE_X_MIN`](../../Reference/3dmod/M3dmodInquire.md)). |
| `Value > 0.0` | Specifies the length along X to use to establish the missing dimension. |

#### `M_COMPLETION_SIZE_Y`

Sets the length along Y to use to establish the missing dimension, when only one box face (plane) is found. Aurora Imaging Library compares the plane's two dimensions (length and width) to the specified completion sizes ([`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md)). The most dissimilar size determines the missing dimension. If [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md) is enabled, the [`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md) of the missing dimension will be the length to which Aurora Imaging Library extrudes the visible face to complete the box, unless [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) and/or [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md) are also enabled.  If [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) and [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md) are enabled, Aurora Imaging Library will attempt to extrude the visible face to the background plane and, if not possible, then to an edge of a staircase plane before using the completion size to complete the box. The completion methods are attempted in the following order:  1. [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md). 2. [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md). 3. [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md).  Note that the completion sizes refer to the box model's dimensions and are not related to the axes of the working coordinate system.  This control type is used only when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAME_AS_NOMINAL` *(default)* | Specifies that the length along Y, to use to establish the missing dimension, is the same as the nominal length in Y of a nominal model. When searching for a range-type model, this corresponds to the middle of the specified size range ([`M_SIZE_Y_MAX`](../../Reference/3dmod/M3dmodInquire.md) - [`M_SIZE_Y_MIN`](../../Reference/3dmod/M3dmodInquire.md)). |
| `Value > 0.0` | Specifies the length along Y to use to establish the missing dimension. |

#### `M_COMPLETION_SIZE_Z`

Sets the length along Z to use to establish the missing dimension, when only one box face (plane) is found. Aurora Imaging Library compares the plane's two dimensions (length and width) to the specified completion sizes ([`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md)). The most dissimilar size determines the missing dimension. If [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md) is enabled, the [`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md) of the missing dimension will be the length to which Aurora Imaging Library extrudes the visible face to complete the box, unless [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) and/or [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md) are also enabled.  If [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) and [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md) are enabled, Aurora Imaging Library will attempt to extrude the visible face to the background plane and, if not possible, then to an edge of a staircase plane before using the completion size to complete the box. The completion methods are attempted in the following order:  1. [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md). 2. [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md). 3. [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md).  Note that the completion sizes refer to the box model's dimensions and are not related to the axes of the working coordinate system.  This control type is used only when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAME_AS_NOMINAL` *(default)* | Specifies that the length along Z, to use to establish the missing dimension, is the same as the nominal length in Z of a nominal model. When searching for a range-type model, this corresponds to the middle of the specified size range ([`M_SIZE_Z_MAX`](../../Reference/3dmod/M3dmodInquire.md) - [`M_SIZE_Z_MIN`](../../Reference/3dmod/M3dmodInquire.md)). |
| `Value > 0.0` | Specifies the length along Z to use to establish the missing dimension. |

#### `M_COMPLETION_TO_BACKGROUND`

Sets whether the visible face can be extruded to a background plane to complete the box, when only one box face (plane) is found. When enabled, Aurora Imaging Library will attempt to extrude the face to the closest background plane (includes the floor plane, if defined using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md), and planes found in the scene) that intersects with the projection of the face. Note that the background plane must intersect with at least 80% of the projection of the face. If the closest background plane would yield an extrusion smaller than [`M_SIZE_..._MIN`](../../Reference/3dmod/M3dmodControl.md), the face cannot be extruded. In this case, no other completion methods will be attempted and the box is rejected. If no background plane intersects with the projection of the face, or the closest one would yield an extrusion larger than [`M_SIZE_..._MAX`](../../Reference/3dmod/M3dmodControl.md), Aurora Imaging Library will attempt to extrude the face using another completion method, if one is enabled.  The completion methods are attempted in the following order:  1. [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md). 2. [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md). 3. [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md).  This control type is used only when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to complete the box using a background plane. |
| `M_ENABLE` *(default)* | Specifies to attempt to complete the box using a background plane. |

#### `M_COMPLETION_TO_STAIRCASE`

Sets whether the visible face can be extruded to an edge of a staircase plane to complete the box, when only one box face (plane) is found. A staircase plane is any plane whose edge touches the extrusion of the face (if the box were extruded to this plane, the two would seem like a staircase).  *[Image: 3dmod_box_extrusion_staircase_plane.png]*  When enabled, if [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) was disabled, or the closest background plane was too far to complete the box, Aurora Imaging Library will attempt to extrude the face to the farthest plane (includes the floor plane, if defined using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md), and planes found in the scene) whose edge touches the projection of the face and falls inside the size range for the missing dimension (for example, for range-type models, [`M_SIZE_..._MIN`](../../Reference/3dmod/M3dmodControl.md) to [`M_SIZE_..._MAX`](../../Reference/3dmod/M3dmodControl.md)). If no plane meets these criteria, the face will be extruded to the completion size, if [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md) is enabled.  The completion methods are attempted in the following order:  1. [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md). 2. [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md). 3. [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md).  This control type is used only when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to complete the box using a staircase plane. |
| `M_ENABLE` *(default)* | Specifies to attempt to complete the box using a staircase plane. |

#### `M_COMPLETION_TO_USER_SIZE`

Sets whether the visible face can be extruded to the specified completion size to complete the box, when only one box face (plane) is found. When enabled, if no other completion method was successful, Aurora Imaging Library will extrude the face to [`M_COMPLETION_SIZE_...`](../../Reference/3dmod/M3dmodControl.md) for the missing dimension.  The completion methods are attempted in the following order:  1. [`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md). 2. [`M_COMPLETION_TO_STAIRCASE`](../../Reference/3dmod/M3dmodControl.md). 3. [`M_COMPLETION_TO_USER_SIZE`](../../Reference/3dmod/M3dmodControl.md).  Note that the completion sizes refer to the box model's dimensions and are not related to the axes of the working coordinate system.  This control type is used only when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to complete the box using the completion size. |
| `M_ENABLE` *(default)* | Specifies to complete the box using the completion size. |

#### `M_COVERAGE_MAX`

Sets the maximum expected model coverage. The model coverage is the percentage of the model's surface covered with inlier points found in the occurrence. An occurrence with a model coverage greater than or equal to the maximum expected model coverage has a score of 100%.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum expected model coverage, as a percentage. |

#### `M_ELONGATION_MAX`

Sets the maximum elongation of the box occurrence. The elongation is defined as the maximum side/minimum side. This is used to prevent lines from being incorrectly identified as boxes when a very large size range is used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the box occurrence has no maximum elongation. It can be infinitely long. |
| `Value > 1.0` | Specifies the box occurrence's maximum elongation, defined as maximum side / minimum side. |

#### `M_ELONGATION_MIN`

Sets the minimum elongation of the box occurrence. The elongation is defined as the maximum side/minimum side. This is used to prevent lines from being incorrectly identified as boxes when a very large size range is used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1.0` *(default)* | Specifies the box occurrence's minimum elongation, defined as maximum side / minimum side. |

#### `M_NORMAL_ANGLE_TOLERANCE`

Sets the angular tolerance to use for [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md), when searching for box occurrences. Aurora Imaging Library only returns a box occurrence as a match if the normal of one of its faces meets the specified [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md), +/- the specified angular tolerance, relative to the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 90.0` *(default)* | Specifies the normal angular tolerance, in degrees. |

#### `M_NORMAL_CONDITION`

Sets whether to find only box occurrences that satisfy the specified condition, relative to the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md), or to find any box occurrence. A box occurrence is only considered a match if the normal of one of its faces meets the specified parallel condition when compared to the vector specified with [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) (within the specified tolerance [`M_NORMAL_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodInquire.md)). For example, you can set [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) to the normal of the floor plane and then set [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md) to [`M_PARALLEL`](../../Reference/3dmod/M3dmodControl.md) to find box occurrences parallel to the floor. If you know that your scene only contains parallel boxes, you should use this control to force the search algorithm to ignore non-parallel candidates. This can help to avoid false matches, particularly when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1. It can also help to speed up the search, since candidates that do not meet the parallel condition will not be further examined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PARALLEL` | Specifies that the normal of one of the faces must be parallel to [`M_NORMAL_...`](../../Reference/3dmod/M3dmodInquire.md), within the specified angular tolerance. |
| `M_UNCONDITIONAL` *(default)* | Specifies that there is no constraint on the normal. |
| `M_VISIBLE_FACE_PARALLEL` | Specifies that the normal of one of the visible faces must be parallel to [`M_NORMAL_...`](../../Reference/3dmod/M3dmodInquire.md), within the specified angular tolerance. If the face whose normal is parallel is inferred, the box occurrence is not selected as a match. |

#### `M_NORMAL_X`

Sets the X-component of the vector against which to compare box occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-component of the vector. |

#### `M_NORMAL_Y`

Sets the Y-component of the vector against which to compare box occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-component of the vector. |

#### `M_NORMAL_Z`

Sets the Z-component of the vector against which to compare box occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-component of the vector. |

#### `M_NUMBER`

Sets the maximum number of occurrences for which to search. Once the required number of occurrences with scores greater than or equal to [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) have been found, the search will stop to avoid exhaustive checking of all possible candidates. If not all the required number of occurrences have been found, Aurora Imaging Library will continue to search for the required number of matches greater than or equal to the acceptance level and with the best score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to find all occurrences.  Note that this setting can increase the search time; always set [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md) to a specific number whenever possible. |
| `Value > 0` *(default)* | Specifies the number of occurrences for which to search. |

#### `M_NUMBER_OF_POINTS_MIN`

Sets the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimum number of points per occurrence found. |

#### `M_NUMBER_OF_VISIBLE_FACES_MAX`

Sets the maximum number of visible faces (planes) required for a box occurrence to be accepted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 6` *(default)* | Specifies the maximum number of visible faces (planes) required for a box occurrence to be accepted. |

#### `M_NUMBER_OF_VISIBLE_FACES_MIN`

Sets the minimum number of visible faces (planes) required for a box occurrence to be accepted. If set to 1, a box can be extruded from a single plane according to the completion constraints ([`M_COMPLETION_...`](../../Reference/3dmod/M3dmodControl.md)) and the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodControl.md)). The plane will be extruded if its normal is less than [`M_COMPLETION_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodInquire.md) away from the reference direction. If these constraints are not met, the plane is not considered a box occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 6` *(default)* | Specifies the maximum number of visible faces (planes) required for a box occurrence to be accepted. |

#### `M_PLANE_ACCEPTANCE`

Sets the acceptance level used for individual faces of the box occurrence. A plane will only be considered a visible face of the box occurrence if its score is greater than or equal to this level. You can set [`M_PLANE_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md) to a low value to detect very occluded faces, while maintaining a high acceptance level for the whole box occurrence using [`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level used for individual faces of the box occurrence. |

#### `M_PLANE_CERTAINTY`

Sets the certainty level for individual faces of the box occurrence. If the score is greater than or equal to the specified certainty level, the plane is considered a visible face of the box, without searching for better faces. You can set [`M_PLANE_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) to a low value to detect very occluded faces, while maintaining a high certainty level for the whole box occurrence using [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty value for individual faces of the box occurrence. |

#### `M_PLANE_MAX_COVERAGE`

Sets the maximum expected coverage for individual faces of the box occurrence. A plane with a coverage greater than or equal to the maximum expected coverage has a score of 100%. You can set [`M_PLANE_MAX_COVERAGE`](../../Reference/3dmod/M3dmodControl.md) to a low value to detect very occluded faces, while maintaining a high maximum expected coverage for the whole box occurrence using [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum coverage for individual faces of the box occurrence. |

#### `M_POLARITY`

Sets whether the normals should point inside or outside the box occurrence. A box occurrence is only considered a match if its normals satisfy the specified condition. For example, you can set [`M_POLARITY`](../../Reference/3dmod/M3dmodInquire.md) to [`M_INSIDE`](../../Reference/3dmod/M3dmodControl.md) to only accept box occurrences that the 3D sensor views from the inside.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies that the normals can point inside or outside. |
| `M_INSIDE` | Specifies that the normals must point inside. |
| `M_OUTSIDE` | Specifies that the normals must point outside. |
| `M_SAME` *(default)* | Specifies that the normals can point either inside or outside, but the direction must be consistent for all faces. |

#### `M_SIZE_X`

Sets the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the expected size along the X-axis. |

#### `M_SIZE_X_MAX`

Sets the maximum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined maximum size.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum size along the X-axis. |
| `Value > 0.0` | Specifies the maximum size along the X-axis. |

#### `M_SIZE_X_MIN`

Sets the minimum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined minimum size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum size along the X-axis. |

#### `M_SIZE_Y`

Sets the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the expected size along the Y-axis. |

#### `M_SIZE_Y_MAX`

Sets the maximum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined maximum size.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum size along the Y-axis. |
| `Value > 0.0` | Specifies the maximum size along the Y-axis. |

#### `M_SIZE_Y_MIN`

Sets the minimum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined minimum size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum size along the Y-axis. |

#### `M_SIZE_Z`

Sets the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis, changing its originally defined size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the expected size along the Z-axis. |

#### `M_SIZE_Z_MAX`

Sets the maximum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis, changing its originally defined maximum size.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum size along the Z-axis. |
| `Value > 0.0` | Specifies the maximum size along the Z-axis. |

#### `M_SIZE_Z_MIN`

Sets the minimum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis, changing its originally defined minimum size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum size along the Z-axis. |

#### `M_TOLERANCE_X`

Sets the tolerance for the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined tolerance. Occurrences must have a size, along their X-axis, within the specified nominal size ([`M_SIZE_X`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the size along the X-axis. |
| `Value >= 0.0` | Specifies the tolerance for the size along the X-axis. |

#### `M_TOLERANCE_Y`

Sets the tolerance for the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined tolerance. Occurrences must have a size, along their Y-axis, within the specified nominal size ([`M_SIZE_Y`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the size along the Y-axis. |
| `Value >= 0.0` | Specifies the tolerance for the size along the Y-axis. |

#### `M_TOLERANCE_Z`

Sets the tolerance for the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis, changing its originally defined tolerance. Occurrences must have a size, along their Z-axis, within the specified nominal size ([`M_SIZE_Z`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the size along the Z-axis. |
| `Value >= 0.0` | Specifies the tolerance for the size along the Z-axis. |

---

### `Find cylinder 3D model finder context ID with a cylinder model`

Specifies a find cylinder 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_CYLINDER_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a cylinder model.

#### `M_ACCEPTANCE`

Sets the acceptance level for the score. An occurrence will be returned only if the match score between the target and the model is greater than or equal to this level. The score is a measure, as a percentage, of the model's surface that is covered by inlier points at the location of the occurrence. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. Set the maximum expected coverage with[`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. A 100% score indicates that the total model coverage is equal to [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md). |

#### `M_BASES`

Sets whether the cylinder model includes bases. Setting this value reserves points corresponding to a found occurrence's bases.  > **Note:** Note that points found on an occurrence's bases will not affect the fit regardless of this control type.

| Value | Description |
| --- | --- |
| `M_WITH_BASES` | Specifies that the cylinder model includes bases. |
| `M_WITHOUT_BASES` | Specifies that the cylinder model does not include bases. |

#### `M_CERTAINTY`

Sets the certainty level for the score, as a percentage. If the score is greater than or equal to the specified certainty level, the occurrence is considered a match, without searching the rest of the target for better matches (provided the specified number of occurrences has been found). The certainty level is defined relative to the maximum expected model coverage, such that setting [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) to 100% indicates that certain matches must have a total model coverage equal to the maximum expected model coverage.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the score, as a percentage.  If you set the certainty level too high (close to 100.0%), you slow down the search because you force the search algorithm to check the whole point cloud for the best possible match(es). A good certainty level is slightly lower than the expected score, so that the search can finish as soon as a match is found. However, if you set the certainty level too low, false matches might be found. |

#### `M_COVERAGE_MAX`

Sets the maximum expected model coverage. The model coverage is the percentage of the model's surface covered with inlier points found in the occurrence. An occurrence with a model coverage greater than or equal to the maximum expected model coverage has a score of 100%.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum expected model coverage, as a percentage. |

#### `M_LENGTH`

Sets the length of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined length.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the length. |

#### `M_LENGTH_MAX`

Sets the maximum length of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined maximum length.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum length. |
| `Value > 0.0` | Specifies the maximum length. |

#### `M_LENGTH_MIN`

Sets the minimum length of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined minimum length.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum length. |

#### `M_MIN_SEPARATION_DISTANCE`

Sets the minimum gap distance along the length of a cylinder before considering the cylinder as two separate occurrences of the cylinder model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to split cylinders. |
| `Value > 0.0` | Specifies the minimum gap distance. |

#### `M_NUMBER`

Sets the maximum number of occurrences for which to search. Once the required number of occurrences with scores greater than or equal to [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) have been found, the search will stop to avoid exhaustive checking of all possible candidates. If not all the required number of occurrences have been found, Aurora Imaging Library will continue to search for the required number of matches greater than or equal to the acceptance level and with the best score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to find all occurrences.  Note that this setting can increase the search time; always set [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md) to a specific number whenever possible. |
| `Value > 0` *(default)* | Specifies the number of occurrences for which to search. |

#### `M_NUMBER_OF_POINTS_MIN`

Sets the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimum number of points per occurrence found. |

#### `M_RADIUS`

Sets the radius of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the radius of the model. |

#### `M_RADIUS_MAX`

Sets the maximum radius of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined maximum radius.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum radius. |
| `Value > 0.0` | Specifies the maximum radius. |

#### `M_RADIUS_MIN`

Sets the minimum radius of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined minimum radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum radius. |

#### `M_RESERVED_POINTS_DISTANCE`

Sets the reserved area around the occurrence, defined as a percentage of the radius. Points found in this area are not considered in the fit, and cannot be considered for other occurrences.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 100.0` *(default)* | Specifies the reserved points distance for the model, as a percentage of its radius. |

#### `M_TOLERANCE_LENGTH`

Sets the tolerance for the length of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined length tolerance. Occurrences must have a length within the specified nominal length ([`M_LENGTH`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the length. |
| `Value >= 0.0` | Specifies the tolerance for the length. |

#### `M_TOLERANCE_RADIUS`

Sets the tolerance for the radius of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined radius tolerance. Occurrences must have a radius within the specified nominal radius ([`M_RADIUS`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the radius. |
| `Value >= 0.0` | Specifies the tolerance for the radius. |

---

### `Find rectangular plane 3D model finder context ID with a rectangular plane model`

Specifies a find rectangular plane 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_RECTANGULAR_PLANE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a rectangular plane model.

#### `M_ACCEPTANCE`

Sets the acceptance level for the score. An occurrence will be returned only if the match score between the target and the model is greater than or equal to this level. The score is a measure, as a percentage, of the model's surface that is covered by inlier points at the location of the occurrence. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. Set the maximum expected coverage with[`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

#### `M_CERTAINTY`

Sets the certainty level for the score, as a percentage. If the score is greater than or equal to the specified certainty level, the occurrence is considered a match, without searching the rest of the target for better matches (provided the specified number of occurrences has been found). The certainty level is defined relative to the maximum expected model coverage, such that setting [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) to 100% indicates that certain matches must have a total model coverage equal to the maximum expected model coverage.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the score, as a percentage. |

#### `M_COVERAGE_MAX`

Sets the maximum expected model coverage. The model coverage is the percentage of the model's surface covered with inlier points found in the occurrence. An occurrence with a model coverage greater than or equal to the maximum expected model coverage has a score of 100%.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum expected model coverage, as a percentage. |

#### `M_ELONGATION_MAX`

Sets the maximum elongation of the rectangular plane occurrence. The elongation is defined as the maximum side/minimum side. This is used to prevent lines from being incorrectly identified as rectangular planes when a very large size range is used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the rectangular plane occurrence has no maximum elongation. It can be infinitely long. |
| `Value > 1.0` | Specifies the rectangular plane occurrence's maximum elongation, defined as maximum side / minimum side. |

#### `M_ELONGATION_MIN`

Sets the minimum elongation of the rectangular plane occurrence. The elongation is defined as the maximum side/minimum side. This is used to prevent lines from being incorrectly identified as rectangular planes when a very large size range is used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1.0` *(default)* | Specifies the rectangular plane occurrence's minimum elongation, defined as maximum side / minimum side. |

#### `M_NORMAL_ANGLE_TOLERANCE`

Sets the angular tolerance to use for [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md), when searching for rectangular plane occurrences. Aurora Imaging Library only returns the occurrence as a match if the normal of the rectangular plane meets the specified [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md), +/- the specified angular tolerance, relative to the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 90.0` *(default)* | Specifies the normal angular tolerance, in degrees. |

#### `M_NORMAL_CONDITION`

Sets whether to find only rectangular plane occurrences that satisfy the specified condition, relative to the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md), or to find any rectangular plane occurrence. A rectangular plane occurrence is only considered a match if its normal meets the specified condition when compared to the vector specified with [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) (within the specified tolerance [`M_NORMAL_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodInquire.md)). For example, you can set [`M_NORMAL_...`](../../Reference/3dmod/M3dmodControl.md) to the normal of the floor plane and then set [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md) to [`M_PARALLEL`](../../Reference/3dmod/M3dmodControl.md) to find rectangular plane occurrences parallel to the floor. If you know that your scene only contains parallel and/or perpendicular rectangular planes, you should use this control to force the search algorithm to ignore all other candidates. This can help to avoid false matches and speed up the search, since candidates that do not meet the specified condition will not be further examined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PARALLEL` | Specifies that the normal of the rectangular plane must be parallel to the vector specified using [`M_NORMAL_...`](../../Reference/3dmod/M3dmodInquire.md), within the specified angular tolerance. |
| `M_PARALLEL_OR_PERPENDICULAR` | Specifies that the normal of the rectangular plane must be either parallel or perpendicular to the vector specified using [`M_NORMAL_...`](../../Reference/3dmod/M3dmodInquire.md), within the specified angular tolerance. |
| `M_PERPENDICULAR` | Specifies that the normal of the rectangular plane must be perpendicular to the vector specified using [`M_NORMAL_...`](../../Reference/3dmod/M3dmodInquire.md), within the specified angular tolerance. |
| `M_UNCONDITIONAL` *(default)* | Specifies that there is no constraint on the normal of the rectangular plane. |

#### `M_NORMAL_X`

Sets the X-component of the vector against which to compare rectangular plane occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-component of the vector. |

#### `M_NORMAL_Y`

Sets the Y-component of the vector against which to compare rectangular plane occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-component of the vector. |

#### `M_NORMAL_Z`

Sets the Z-component of the vector against which to compare rectangular plane occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-component of the vector. |

#### `M_NUMBER`

Sets the maximum number of occurrences for which to search. Once the required number of occurrences with scores greater than or equal to [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) have been found, the search will stop to avoid exhaustive checking of all possible candidates. If not all the required number of occurrences have been found, Aurora Imaging Library will continue to search for the required number of matches greater than or equal to the acceptance level and with the best score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to find all occurrences.  Note that this setting can increase the search time; always set [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md) to a specific number whenever possible. |
| `Value > 0` *(default)* | Specifies the number of occurrences for which to search. |

#### `M_NUMBER_OF_POINTS_MIN`

Sets the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimum number of points per occurrence found. |

#### `M_SIZE_X`

Sets the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the expected size along the X-axis. |

#### `M_SIZE_X_MAX`

Sets the maximum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined maximum size.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum size along the X-axis. |
| `Value > 0.0` | Specifies the maximum size along the X-axis. |

#### `M_SIZE_X_MIN`

Sets the minimum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined minimum size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum size along the X-axis. |

#### `M_SIZE_Y`

Sets the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the expected size along the Y-axis. |

#### `M_SIZE_Y_MAX`

Sets the maximum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined maximum size.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum size along the Y-axis. |
| `Value > 0.0` | Specifies the maximum size along the Y-axis. |

#### `M_SIZE_Y_MIN`

Sets the minimum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined minimum size.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum size along the Y-axis. |

#### `M_TOLERANCE_X`

Sets the tolerance for the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis, changing its originally defined tolerance. Occurrences must have a size, along their X-axis, within the specified nominal size ([`M_SIZE_X`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the size along the X-axis. |
| `Value >= 0.0` | Specifies the tolerance for the size along the X-axis. |

#### `M_TOLERANCE_Y`

Sets the tolerance for the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined tolerance. Occurrences must have a size, along their Y-axis, within the specified nominal size ([`M_SIZE_Y`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the size along the Y-axis. |
| `Value >= 0.0` | Specifies the tolerance for the size along the Y-axis. |

---

### `Find sphere 3D model finder context ID with a sphere model`

Specifies a find sphere 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SPHERE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a sphere model.

#### `M_ACCEPTANCE`

Sets the acceptance level for the score. An occurrence will be returned only if the match score between the target and the model is greater than or equal to this level. The score is a measure, as a percentage, of the model's surface that is covered by inlier points at the location of the occurrence. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. Set the maximum expected coverage with[`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 100.0` *(default)* | Specifies an acceptable score, as a percentage. A 100% score indicates that the total model coverage is equal to [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md). |

#### `M_CERTAINTY`

Sets the certainty level for the score, as a percentage. If the score is greater than or equal to the specified certainty level, the occurrence is considered a match, without searching the rest of the target for better matches (provided the specified number of occurrences has been found). The certainty level is defined relative to the maximum expected model coverage, such that setting [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) to 100% indicates that certain matches must have a total model coverage equal to the maximum expected model coverage.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 100.0` *(default)* | Specifies the certainty level for the score, as a percentage.  If you set the certainty level too high (close to 100.0%), you slow down the search because you force the search algorithm to check the whole point cloud for the best possible match(es). A good certainty level is slightly lower than the expected score, so that the search can finish as soon as a match is found. However, if you set the certainty level too low, false matches might be found. |

#### `M_COVERAGE_MAX`

Sets the maximum expected model coverage. The model coverage is the percentage of the model's surface covered with inlier points found in the occurrence. An occurrence with a model coverage greater than or equal to the maximum expected model coverage has a score of 100%.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum expected model coverage, as a percentage. |

#### `M_NUMBER`

Sets the maximum number of occurrences for which to search. Once the required number of occurrences with scores greater than or equal to [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) have been found, the search will stop to avoid exhaustive checking of all possible candidates. If not all the required number of occurrences have been found, Aurora Imaging Library will continue to search for the required number of matches greater than or equal to the acceptance level and with the best score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to find all occurrences.  Note that this setting can increase the search time; always set [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md) to a specific number whenever possible. |
| `Value > 0` *(default)* | Specifies the number of occurrences for which to search. |

#### `M_NUMBER_OF_POINTS_MIN`

Sets the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimum number of points per occurrence found. |

#### `M_RADIUS`

Sets the radius of a nominal cylinder model ([`M_SPHERE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the expected radius. |

#### `M_RADIUS_MAX`

Sets the maximum radius of a range-type sphere model ([`M_SPHERE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined maximum radius.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the maximum radius. |
| `Value > 0.0` | Specifies the maximum radius. |

#### `M_RADIUS_MIN`

Sets the minimum radius for a range-type sphere model ([`M_SPHERE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined minimum radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum radius. |

#### `M_RESERVED_POINTS_DISTANCE`

Sets the reserved area around the occurrence, defined as a percentage of the radius. Points found in this area are not considered in the fit, and cannot be considered for other occurrences.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 100.0` *(default)* | Specifies the reserved points distance for the model, as a percentage of its radius. |

#### `M_TOLERANCE_RADIUS`

Sets the tolerance for the radius of a nominal sphere model ([`M_SPHERE`](../../Reference/3dmod/M3dmodInquire.md)), changing its originally defined radius tolerance. Occurrences must have a radius within the specified nominal radius ([`M_RADIUS`](../../Reference/3dmod/M3dmodControl.md)) +/- the specified tolerance.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no constraint on the radius. |
| `Value >= 0.0` | Specifies the tolerance for the radius. |

---

### `Find surface or planar surface 3D model finder context ID with a surface model`

Specifies a find surface or planar surface 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) or [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a surface model.

#### `M_ACCEPTANCE`

Sets the acceptance level for the score. An occurrence will be returned only if the match score between the target and the model is greater than or equal to this level. The score is a measure, as a percentage, of the model's surface that is covered by inlier points at the location of the occurrence. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. Set the maximum expected coverage with[`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 100.0` *(default)* | Specifies an acceptable score, as a percentage. A 100% score indicates that the total model coverage is equal to [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md). |

#### `M_ACCEPTANCE_COLOR`

Sets the acceptance level for the color score. An occurrence will be returned only if the color score between the target and the model is greater than or equal to this level. The color score is a measure, as a percentage, of the similarity between the color of the model and the color of the target.  Note that this control type has no effect unless [`M_USE_COLOR`](../../Reference/3dmod/M3dmodControl.md)is set to[`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level for the color score, as a percentage. |

#### `M_ACCEPTANCE_TARGET`

Sets the acceptance level for the target score. An occurrence will be returned only if the target score between the target and the model is greater than or equal to this level. The target score is a measure, as a percentage, of the total points found in the occurrence that are also found in the model. Points found in the occurrence that are not present in the model will reduce the target score. For example, a target score of 100% means that there are no points in the occurrence that do not fit the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 100.0` *(default)* | Specifies the acceptance level for the target score, as a percentage. |

#### `M_CERTAINTY`

Sets the certainty level for the score, as a percentage. If the score is greater than or equal to the specified certainty level, the occurrence is considered a match, without searching the rest of the target for better matches (provided the specified number of occurrences has been found). The certainty level is defined relative to the maximum expected model coverage, such that setting [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) to 100% indicates that certain matches must have a total model coverage equal to the maximum expected model coverage.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 100.0` *(default)* | Specifies the certainty level for the score, as a percentage.  If you set the certainty level too high (close to 100.0%), you slow down the search because you force the search algorithm to check the whole point cloud for the best possible match(es). A good certainty level is slightly lower than the expected score, so that the search can finish as soon as a match is found. However, if you set the certainty level too low, false matches might be found. |

#### `M_COVERAGE_MAX`

Sets the maximum expected model coverage. The model coverage is the percentage of the model's surface covered with inlier points found in the occurrence. An occurrence with a model coverage greater than or equal to the maximum expected model coverage has a score of 100%.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum expected model coverage, as a percentage. |

#### `M_FIT_SCORE_MIN`

Sets the minimum expected occurrence fit score. Note that this control type has no effect unless [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md)is set to[`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum expected fit score. |

#### `M_MODEL_PLANES_OCCLUSION_MODE`

Sets the occlusion level of the planar regions of the model in the scene. If less than 30% of a planar region's surface would be occluded at the position of an occurrence, set [`M_MODEL_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_LOW`](../../Reference/3dmod/M3dmodControl.md). If the occlusion level is between 30% and 50%, set [`M_MODEL_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_MEDIUM`](../../Reference/3dmod/M3dmodControl.md). If the occlusion level is between 50% and 80%, set [`M_MODEL_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_HIGH`](../../Reference/3dmod/M3dmodControl.md). If the occlusion level is greater than 80%, set [`M_MODEL_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_VERY_HIGH`](../../Reference/3dmod/M3dmodControl.md).  If different planar regions have differing occlusion levels, use the occlusion level for the highest occluded planar region.  Note that this control type is only available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies a high model plane occlusion. |
| `M_LOW` *(default)* | Specifies a low model plane occlusion. |
| `M_MEDIUM` | Specifies a medium model plane occlusion. |
| `M_VERY_HIGH` | Specifies a very high model plane occlusion. |

#### `M_NUMBER`

Sets the maximum number of occurrences for which to search. Once the required number of occurrences with scores greater than or equal to [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) have been found, the search will stop to avoid exhaustive checking of all possible candidates. If not all the required number of occurrences have been found, Aurora Imaging Library will continue to search for the required number of matches greater than or equal to the acceptance level and with the best score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to find all occurrences.  Note that this setting can increase the search time; always set [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md) to a specific number whenever possible. |
| `Value > 0` *(default)* | Specifies the number of occurrences for which to search. |

#### `M_NUMBER_OF_POINTS_MIN`

Sets the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimum number of points per occurrence found. |

#### `M_REMOVE_OUTLIERS`

Sets whether to perform outlier removal on the model, changing the model's originally defined outlier removal setting. When enabled, Aurora Imaging Library removes outliers such as clusters of points around the perimeter of the model. Enable outlier removal if the model contains stray points, especially if [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) is enabled. The default for this control type is set when adding the surface model to the context using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_SURFACE`](../../Reference/3dmod/M3dmodDefine.md).  Note that you should not enable outlier removal if your model was defined from a point cloud created from a CAD file (using [`MbufImport`](../../Reference/buf/MbufImport.md)/[`MbufRestore`](../../Reference/buf/MbufRestore.md)) since parts of the model can be removed, which can cause the search to fail/return false positives.  You can use [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) to draw the remaining points after preprocessing ([`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md) with [`M_DRAW_MODEL_PREPROCESSED`](../../Reference/3dmod/M3dmodControlDraw.md)).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform outlier removal on the model. |
| `M_ENABLE` | Specifies to perform outlier removal on the model. |

#### `M_REMOVE_OUTLIERS_SEARCH_MODE`

Sets the search mode for finding outliers when [`M_REMOVE_OUTLIERS`](../../Reference/3dmod/M3dmodControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically set the search mode based on the point cloud's organization; either [`M_ORGANIZED`](../../Reference/3dmod/M3dmodControl.md) if organized or [`M_TREE`](../../Reference/3dmod/M3dmodControl.md) if unorganized. |
| `M_ORGANIZED` | Specifies to use the point cloud's organizational structure to determine the outliers. This option is supported only for an organized point cloud. |
| `M_TREE` | Specifies to use a KD tree search mode to determine the outliers. |

#### `M_RESTING_PLANE_ANGLE_TOLERANCE`

Sets the angular tolerance for the resting plane constraint. The resting plane provides contextual information about how the occurrence lies within the scene. Aurora Imaging Library will only return occurrences that rest on this plane, +/- the specified angular tolerance. Note that this does not affect any of the scores, but it filters occurrences that do not meet this constraint. It also does not decrease the search time because occurrences are filtered near the end of the search.  You can copy the resting plane to or from the specified find surface or planar surface 3D model finder context using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_RESTING_PLANE`](../../Reference/3dmod/M3dmodCopy.md). Note that to use a resting plane, you must also copy a floor plane ([`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)) to the find surface or planar surface 3D model finder context.  A small angular tolerance is typically required since the angle found is not always exact.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 180.0` *(default)* | Specifies the angular tolerance, in degrees. |

#### `M_SEARCH_POINT_RESOLUTION`

Sets the resolution of the target point cloud (scene), where the resolution is expressed as the average distance between points. Very different resolutions between the scene and the model might cause missed occurrences. You should specify the resolution of the scene when it is incompatible with the resolution of the model. For example, if the distance between points in the scene is greater than that of the model, Aurora Imaging Library will subsample the model to match the resolution of the target. If the distance between points in the scene is lower than that of the model's, Aurora Imaging Library will upsample the model to match the resolution of the target, using its associated mesh. If there is no associated mesh in the find surface or planar surface 3D model finder context when Aurora Imaging Library attempts to upsample the model, an error is generated.  After an [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operation, you can check whether the resolutions of the scene and model were compatible, using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_STATUS_SCENE_RESOLUTION`](../../Reference/3dmod/M3dmodGetResult.md).  You can inquire the resolution of the defined model (before preprocessing) using [`M_MODEL_RESOLUTION`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAME_AS_MODEL` *(default)* | Specifies that the target point cloud (scene) has the same resolution as the model. |
| `Value > 0` | Specifies the expected resolution of the target point cloud (scene), where the resolution is expressed as the average distance between points. |

#### `M_SET_POSITION_X`

Changes the X-coordinate of the model's reference axis origin. A surface occurrence's found position is determined by the model's reference axis; position results return the coordinates of the model's reference axis origin transformed at the model occurrence.  Note that this does not transform the model, but moves the origin of the model coordinate system to this position.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate. |

#### `M_SET_POSITION_Y`

Changes the Y-coordinate of the model's reference axis origin. A surface occurrence's found position is determined by the model's reference axis; position results return the coordinates of the model's reference axis origin transformed at the model occurrence.  Note that this does not transform the model, but moves the origin of the model coordinate system to this position.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate. |

#### `M_SET_POSITION_Z`

Changes the Z-coordinate of the model's reference axis origin. A surface occurrence's found position is determined by the model's reference axis; position results return the coordinates of the model's reference axis origin transformed at the model occurrence.  Note that this does not transform the model, but moves the origin of the model coordinate system to this position.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate. |

#### `M_TARGET_PLANES_OCCLUSION_MODE`

Sets the target occlusion level when a planar region of the model is a portion of a planar region of an object in the scene. This sets how much of the object's planar region is not found in the model. If less than 30% of an object's planar region in the scene is not present in the model's surface, set [`M_TARGET_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_LOW`](../../Reference/3dmod/M3dmodControl.md). If the occlusion level is between 30% and 50%, set [`M_TARGET_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_MEDIUM`](../../Reference/3dmod/M3dmodControl.md). If the occlusion level is between 50% and 80%, set [`M_TARGET_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_HIGH`](../../Reference/3dmod/M3dmodControl.md). If the occlusion level is greater than 80%, set [`M_TARGET_PLANES_OCCLUSION_MODE`](../../Reference/3dmod/M3dmodControl.md) to [`M_VERY_HIGH`](../../Reference/3dmod/M3dmodControl.md).  Note that this control type is only available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Specifies a high target plane occlusion. |
| `M_LOW` *(default)* | Specifies a low target plane occlusion. |
| `M_MEDIUM` | Specifies a medium target plane occlusion. |
| `M_VERY_HIGH` | Specifies a very high target plane occlusion. |

#### `M_TRANSFORM_MODEL`

Transforms the model (rotation and translation) to a new position and orientation. When the transformation occurs, the orientation and position of the model prior to the transformation is lost.  Note that a surface occurrence's found position is determined by the model's reference axis; position results return the coordinates of the model's reference axis origin transformed at the model occurrence.

| Value | Description |
| --- | --- |
| `Transformation matrix object ID` | Specifies the identifier of a transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The transformation matrix must be of type [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md). |

### For a 3D model finder result buffer

The following [`ContextOrResult3dmodId`](../../Reference/3dmod/M3dmodControl.md), [`ControlType`](../../Reference/3dmod/M3dmodControl.md), and [`ControlValue`](../../Reference/3dmod/M3dmodControl.md) parameter settings can be specified for a 3D model finder result buffer when [`Index`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_GENERAL`](../../Reference/3dmod/M3dmodControl.md) or [`M_DEFAULT`](../../Reference/3dmod/M3dmodControl.md).

---

### `3D model finder result buffer ID`

Specifies a 3D model finder result buffer, allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md).

#### `M_RESET`

Removes all results from the 3D model finder result buffer. This does not delete the result buffer, unlike [`M3dmodFree`](../../Reference/3dmod/M3dmodFree.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default behavior. |

#### `M_STOP_FIND`

Stops the current execution of [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) from another thread. Any completed results from the 3D model finder operation will be available.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default behavior. |

Note that if you expect occurrences to have a significantly different size than the model, you should use a range-type model instead, since the algorithm used for this type of model is better suited.
