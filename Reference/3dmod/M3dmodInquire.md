---
doctype: Reference
module: 3dmod
function: M3dmodInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodInquire"
---

# M3dmodInquire

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

> Inquire about a setting of a find 3D model finder context.

## Syntax

```c
AIL_INT64 M3dmodInquire(
    AIL_ID    Context3dmodId,  //in
    AIL_INT64 Index,           //in
    AIL_INT64 InquireType,     //in
    void *    UserVarPtr       //out
)
```

## Description

This function inquires about a specified setting of a find 3D model finder context.

To inquire about draw 3D model finder context settings, use [`M3dmodInquireDraw`](../../Reference/3dmod/M3dmodInquireDraw.md) instead.

## Parameters

### `Context3dmodId` *(in, AIL_ID)*

Specifies the identifier of the find 3D model finder context about which to inquire information. The find 3D model finder context must have been previously allocated on the required system using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_..._CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md).

### `Index` *(in, AIL_INT64)*

Specifies what to inquire. Set this parameter to one of the following values.

*For specifying a general context or individual model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CONTEXT` *(default)* | Specifies to inquire information about the find 3D model finder context. |
| `0` | Specifies to inquire the 3D model defined in the context. Note that the model must have been added to the context, using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), prior to calling [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with this value. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring a find box, cylinder, rectangular plane, sphere, surface, or planar surface 3D model finder context

The following [`Context3dmodId`](../../Reference/3dmod/M3dmodInquire.md) and [`InquireType`](../../Reference/3dmod/M3dmodInquire.md) parameter settings can be specified for a 3D model finder context when [`Index`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_CONTEXT`](../../Reference/3dmod/M3dmodInquire.md) or [`M_DEFAULT`](../../Reference/3dmod/M3dmodInquire.md).

---

### `Find box 3D model finder context ID`

Specifies a find box 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_BOX_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_DIRECTION_MODE`

Inquires how to interpret the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodInquire.md)).  For a find box 3D model finder context, this is only used when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_DIRECTION_REFERENCE_X`

Inquires the X-coordinate of the position, or the X-component of the vector, that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodInquire.md).  For a find box 3D model finder context, this is only used when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_DIRECTION_REFERENCE_Y`

Inquires the Y-coordinate of the position, or the Y-component of the vector, that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodInquire.md).  For a find box 3D model finder context, this is only used when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_DIRECTION_REFERENCE_Z`

Inquires the Z-coordinate of the position, or the Z-component of the vector, that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodInquire.md).  For a find box 3D model finder context, this is only used when [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_Z`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_DISTANCE`

Inquires the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_DISTANCE_MODE`

Inquires how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FLOOR_DEFINED`

Inquires whether a floor plane has been defined using[`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that a floor plane has not been defined. |
| `M_TRUE` | Specifies that a floor plane has been defined. |

#### `M_NUMBER_MODELS`

Inquires the number of models in the context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of models. |

#### `M_PREPROCESSED`

Inquires whether the find 3D model finder context is preprocessed. The context must be preprocessed (using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md)) before calling [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). After certain settings of the context are changed using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md), a model is added or removed using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), or a floor plane is copied to the context using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

#### `M_SORT`

Inquires the sorting key for result retrieval.

| Value | Description |
| --- | --- |
| *(see [`M_SORT`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SORT_DIRECTION`

Inquires whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_DIRECTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TIMEOUT`

Inquires the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find cylinder 3D model finder context ID`

Specifies a find cylinder 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_CYLINDER_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_FIT_DISTANCE`

Inquires the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_DISTANCE_MODE`

Inquires how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_NORMALS_DISTANCE`

Inquires the acceptable deviation between a given point's normal vector and the normal vector of the model at the same point, to decide which points are used when fitting.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_NORMALS_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_MODELS`

Inquires the number of models in the context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of models. |

#### `M_PERSEVERANCE`

Inquires the algorithm's search perseverance when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_PERSEVERANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_PREPROCESSED`

Inquires whether the find 3D model finder context is preprocessed. The context must be preprocessed (using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md)) before calling [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). After certain settings of the context are changed using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md), or a model is added or removed using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

#### `M_SORT`

Inquires the sorting key for result retrieval.

| Value | Description |
| --- | --- |
| *(see [`M_SORT`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SORT_DIRECTION`

Inquires whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_DIRECTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TIMEOUT`

Inquires the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find rectangular plane 3D model finder context ID`

Specifies a find rectangular plane 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_RECTANGULAR_PLANE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_FIT_DISTANCE`

Inquires the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_DISTANCE_MODE`

Inquires how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_MODELS`

Inquires the number of models in the context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of models. |

#### `M_PREPROCESSED`

Inquires whether the find 3D model finder context is preprocessed. The context must be preprocessed (using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md)) before calling [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). After certain settings of the context are changed using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md), or a model is added or removed using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

#### `M_SORT`

Inquires the sorting key for result retrieval.

| Value | Description |
| --- | --- |
| *(see [`M_SORT`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SORT_DIRECTION`

Inquires whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_DIRECTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TIMEOUT`

Inquires the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find sphere 3D model finder context ID`

Specifies a find sphere 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SPHERE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_FIT_DISTANCE`

Inquires the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_DISTANCE_MODE`

Inquires how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_ITERATIONS_MAX`

Inquires the maximum number of fit iterations to perform, when finding a sphere occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_ITERATIONS_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_NORMALS_DISTANCE`

Inquires the acceptable deviation between a given point's normal vector and the normal vector of the model at the same point, to decide which points are used when fitting.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_NORMALS_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_MODELS`

Inquires the number of models in the context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of models. |

#### `M_PERSEVERANCE`

Inquires the algorithm's search perseverance when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_PERSEVERANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_PREPROCESSED`

Inquires whether the find 3D model finder context is preprocessed. The context must be preprocessed (using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md)) before calling [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). After certain settings of the context are changed using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md), or a model is added or removed using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

#### `M_SORT`

Inquires the sorting key for result retrieval.

| Value | Description |
| --- | --- |
| *(see [`M_SORT`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SORT_DIRECTION`

Inquires whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_DIRECTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TIMEOUT`

Inquires the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find surface or planar surface 3D model finder context ID`

Specifies a find surface or planar surface 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) or [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations.

#### `M_CONVERSION_GAMMA`

Inquires whether to remove gamma correction before the match.

| Value | Description |
| --- | --- |
| *(see [`M_CONVERSION_GAMMA`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_DIRECTION_MODE`

Inquires how to interpret the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_DIRECTION_REFERENCE_X`

Inquires the X-component of the vector that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_DIRECTION_REFERENCE_Y`

Inquires the Y-component of the vector that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_DIRECTION_REFERENCE_Z`

Inquires the Z-component of the vector that determines the reference direction, depending on [`M_DIRECTION_MODE`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_Z`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_EXHAUSTIVE_SEARCH`

Inquires whether to perform very robust but slow exhaustive searching.  Note that this inquire type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| *(see [`M_EXHAUSTIVE_SEARCH`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_DISTANCE`

Inquires the fit distance when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodInquire.md).  Note that for a find surface or planar surface 3D model finder context, this is only used when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dmod/M3dmodInquire.md) and [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodInquire.md) is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_DISTANCE_MODE`

Inquires how Aurora Imaging Library establishes the fit distance, which is used when determining whether points belong to a certain occurrence.  > **Note:** Note that for a find surface or planar surface 3D model finder context, this is only used when [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodInquire.md) is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_DISTANCE_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FLOOR_DEFINED`

Inquires whether a floor plane has been defined using[`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that a floor plane has not been defined. |
| `M_TRUE` | Specifies that a floor plane has been defined. |

#### `M_MODEL_NORMAL_SEARCH_MODE`

Inquires the search mode for calculating model normals when the defined model has no normals component ([`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md)).

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_NORMAL_SEARCH_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_MODELS`

Inquires the number of models in the context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of models. |

#### `M_PERSEVERANCE`

Inquires the algorithm's search perseverance when searching for occurrences.  Note that this inquire type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| *(see [`M_PERSEVERANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_PREPROCESSED`

Inquires whether the find 3D model finder context is preprocessed. The context must be preprocessed (using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md)) before calling [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). After certain settings of the context are changed using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md), a model is added or removed using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), or a floor plane or resting plane is copied to the context using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

#### `M_REFINE_REGISTRATION`

Inquires whether and how to perform refine registration of each occurrence found.

| Value | Description |
| --- | --- |
| *(see [`M_REFINE_REGISTRATION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REMOVE_BACKGROUND`

Inquires whether to remove background points from the target point cloud before performing the match.

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_BACKGROUND`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REMOVE_BACKGROUND_SEARCH_MODE`

Inquires the search mode for finding the background points when [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodInquire.md) is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_BACKGROUND_SEARCH_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REMOVE_FLOOR`

Inquires whether to remove floor points from the target point cloud before performing the match.

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_FLOOR`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REMOVE_FLOOR_DIRECTION`

Inquires the direction, relative to the defined floor plane, in which to remove points when [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodInquire.md) is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_FLOOR_DIRECTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REMOVE_FLOOR_EXPECTED_PERCENTAGE`

Inquires the percentage of total points in the target point cloud that are expected to be floor points when [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodInquire.md) is enabled and [`M_REMOVE_FLOOR_OFFSET`](../../Reference/3dmod/M3dmodInquire.md) is set to [`M_AUTO_VALUE`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_FLOOR_EXPECTED_PERCENTAGE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REMOVE_FLOOR_OFFSET`

Inquires the offset, relative to the defined floor plane, within which to remove points when [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodInquire.md) is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_FLOOR_OFFSET`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REUSE_RESULT`

Inquires whether to reuse the result of the previous search at the beginning of the current search.

| Value | Description |
| --- | --- |
| *(see [`M_REUSE_RESULT`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SAVE_FIT_INFO`

Inquires whether to enable the calculation of the root-mean square (RMS) error and the fit score.

| Value | Description |
| --- | --- |
| *(see [`M_SAVE_FIT_INFO`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SCENE_COMPLEXITY`

Inquires the complexity of the scene.  Note that this inquire type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| *(see [`M_SCENE_COMPLEXITY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SCENE_PROJECTION`

Inquires whether to apply occlusion handling to the model, which discards points of the model that are not visible to the 3D sensor at the position of the occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_SCENE_PROJECTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SCORE_MODE`

Inquires the mode with which to associate model and target points for score calculations.

| Value | Description |
| --- | --- |
| *(see [`M_SCORE_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SORT`

Inquires the sorting key for result retrieval.

| Value | Description |
| --- | --- |
| *(see [`M_SORT`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SORT_DIRECTION`

Inquires whether results are sorted in ascending or descending order.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_DIRECTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TIMEOUT`

Inquires the maximum amount of time for [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to complete the 3D model finder operation before generating a time-out error.

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_USE_COLOR`

Inquires whether to use the color information of the model and scene when searching for surface occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_USE_COLOR`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_USER_DEFINED_REGISTRATION_CONTEXT_ID`

Inquires the identifier of the internal registration context to use for fine tuning when [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md)is set to[`M_USER_DEFINED_REGISTRATION`](../../Reference/3dmod/M3dmodInquire.md).

### For inquiring the type of model in a find 3D model finder context

The following [`Context3dmodId`](../../Reference/3dmod/M3dmodInquire.md) and [`InquireType`](../../Reference/3dmod/M3dmodInquire.md) parameter settings can be specified for a model in a 3D model finder context when [`Index`](../../Reference/3dmod/M3dmodInquire.md) is set to [`0`](../../Reference/3dmod/M3dmodInquire.md).

---

### `Find 3D model finder context ID with a model`

Specifies a find box, cylinder, rectangular plane, sphere, surface, or planar surface 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_..._CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a model.

#### `M_MODEL_TYPE`

Inquires the type of model in the find 3D model finder context.

| Value | Description |
| --- | --- |
| `M_BOX` | Specifies that the model is a nominal box model. |
| `M_BOX_RANGE` | Specifies that the model is a range-type box model. |
| `M_CYLINDER` | Specifies that the model is a nominal cylinder model. |
| `M_CYLINDER_RANGE` | Specifies that the model is a range-type cylinder model. |
| `M_RECTANGLE` | Specifies that the model is a nominal rectangular plane model. |
| `M_RECTANGLE_RANGE` | Specifies that the model is a range-type rectangular-plane model. |
| `M_SPHERE` | Specifies that the model is a nominal sphere model. |
| `M_SPHERE_RANGE` | Specifies that the model is a range-type sphere model. |
| `M_SURFACE` | Specifies that the model is a surface model, defined by a point cloud. |

### For inquiring a box, cylinder, rectangular plane, sphere, or surface 3D model

The following [`Context3dmodId`](../../Reference/3dmod/M3dmodInquire.md) and [`InquireType`](../../Reference/3dmod/M3dmodInquire.md) parameter settings can be specified for a box, cylinder, rectangular plane, sphere, or surface 3D model when [`Index`](../../Reference/3dmod/M3dmodInquire.md) is set to [`0`](../../Reference/3dmod/M3dmodInquire.md).

---

### `Find box 3D model finder context ID with a box model`

Specifies a find box 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_BOX_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a box model.

#### `M_ACCEPTANCE`

Inquires the acceptance level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_BOX_FACE_PARALLELISM_THRESHOLD`

Inquires the maximum shear angle allowed between adjacent rectangles for them to count as the same box.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_FACE_PARALLELISM_THRESHOLD`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_BOX_FACE_PERPENDICULARITY_THRESHOLD`

Inquires the maximum deviation from 90 degrees allowed between adjacent rectangles for them to count as the same box.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_FACE_PERPENDICULARITY_THRESHOLD`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_CERTAINTY`

Inquires the certainty level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_CERTAINTY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COMPLETION_ANGLE_TOLERANCE`

Inquires the completion angular tolerance, when only one box face (plane) is found.  Note that the corresponding control type has no effect unless [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_COMPLETION_ANGLE_TOLERANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COMPLETION_SIZE_X`

Inquires the length along X to use to establish the missing dimension, when only one box face (plane) is found.  Note that the corresponding control type has no effect unless [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_COMPLETION_SIZE_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COMPLETION_SIZE_Y`

Inquires the length along Y to use to establish the missing dimension, when only one box face (plane) is found.  Note that the corresponding control type has no effect unless [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_COMPLETION_SIZE_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COMPLETION_SIZE_Z`

Inquires the length along Z to use to establish the missing dimension, when only one box face (plane) is found.  Note that the corresponding control type has no effect unless [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_COMPLETION_SIZE_Z`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COMPLETION_TO_BACKGROUND`

Inquires whether the visible face could be extruded to a background plane to complete the box, when only one box face (plane) is found.  Note that the corresponding control type has no effect unless [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_COMPLETION_TO_BACKGROUND`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COMPLETION_TO_STAIRCASE`

Inquires whether the visible face could be extruded to an edge of a staircase plane to complete the box, when only one box face (plane) is found.  Note that the corresponding control type has no effect unless [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_COMPLETION_TO_STAIRCASE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COMPLETION_TO_USER_SIZE`

Inquires whether the visible face could be extruded to the specified completion size to complete the box, when only one box face (plane) is found.  Note that the corresponding control type has no effect unless [`M_NUMBER_OF_VISIBLE_FACES_MIN`](../../Reference/3dmod/M3dmodInquire.md) is set to 1.

| Value | Description |
| --- | --- |
| *(see [`M_COMPLETION_TO_USER_SIZE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COVERAGE_MAX`

Inquires the maximum expected model coverage.

| Value | Description |
| --- | --- |
| *(see [`M_COVERAGE_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_ELONGATION_MAX`

Inquires the maximum elongation of the box occurrence. The elongation is defined as the maximum side/minimum side.

| Value | Description |
| --- | --- |
| *(see [`M_ELONGATION_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_ELONGATION_MIN`

Inquires the minimum elongation of the box occurrence. The elongation is defined as the maximum side/minimum side.

| Value | Description |
| --- | --- |
| *(see [`M_ELONGATION_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_ANGLE_TOLERANCE`

Inquires the angular tolerance to use for [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md), when searching for box occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_ANGLE_TOLERANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_CONDITION`

Inquires whether to find only box occurrences that satisfy the specified condition, relative to the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodInquire.md), or to find any box occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_CONDITION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_X`

Inquires the X-component of the vector against which to compare box occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_Y`

Inquires the Y-component of the vector against which to compare box occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_Z`

Inquires the Z-component of the vector against which to compare box occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_Z`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER`

Inquires the maximum number of occurrences for which to search.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_OF_POINTS_MIN`

Inquires the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_OF_VISIBLE_FACES_MAX`

Inquires the maximum number of visible faces (planes) required for a box occurrence to be accepted.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_VISIBLE_FACES_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_OF_VISIBLE_FACES_MIN`

Inquires the minimum number of visible faces (planes) required for a box occurrence to be accepted. If set to 1, a box can be extruded from a single plane according to the completion constraints ([`M_COMPLETION_...`](../../Reference/3dmod/M3dmodInquire.md)) and the reference direction ([`M_DIRECTION_REFERENCE_...`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_VISIBLE_FACES_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_PLANE_ACCEPTANCE`

Inquires the acceptance level used for individual faces of the box occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_PLANE_ACCEPTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_PLANE_CERTAINTY`

Inquires the certainty level for individual faces of the box occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_PLANE_CERTAINTY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_PLANE_MAX_COVERAGE`

Inquires the maximum expected coverage for individual faces of the box occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_PLANE_MAX_COVERAGE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_POLARITY`

Inquires whether the normals should point inside or outside the box occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_POLARITY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_X`

Inquires the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_X_MAX`

Inquires the maximum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_X_MIN`

Inquires the minimum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Y`

Inquires the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis, changing its originally defined size.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Y_MAX`

Inquires the maximum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Y_MIN`

Inquires the minimum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Z`

Inquires the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis, changing its originally defined size.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Z`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Z_MAX`

Inquires the maximum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Z_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Z_MIN`

Inquires the minimum size of a range-type box model ([`M_BOX_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Z_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_X`

Inquires the tolerance for the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_Y`

Inquires the tolerance for the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_Z`

Inquires the tolerance for the size of a nominal box model ([`M_BOX`](../../Reference/3dmod/M3dmodInquire.md)) along its Z-axis.

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_Z`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find cylinder 3D model finder context ID with a cylinder model`

Specifies a find cylinder 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_CYLINDER_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a cylinder model.

#### `M_ACCEPTANCE`

Inquires the acceptance level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_BASES`

Inquires whether the cylinder model includes bases.

| Value | Description |
| --- | --- |
| *(see [`M_BASES`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_CERTAINTY`

Inquires the certainty level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_CERTAINTY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COVERAGE_MAX`

Inquires the maximum expected model coverage.

| Value | Description |
| --- | --- |
| *(see [`M_COVERAGE_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_LENGTH`

Inquires the length of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_LENGTH_MAX`

Inquires the maximum length of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_LENGTH_MIN`

Inquires the minimum length of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_MIN_SEPARATION_DISTANCE`

Inquires the minimum gap distance along the length of a cylinder before considering the cylinder as two separate occurrences of the cylinder model.

| Value | Description |
| --- | --- |
| *(see [`M_MIN_SEPARATION_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER`

Inquires the maximum number of occurrences for which to search.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_OF_POINTS_MIN`

Inquires the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RADIUS`

Inquires the radius of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RADIUS_MAX`

Inquires the maximum radius of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RADIUS_MIN`

Inquires the minimum radius of a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RESERVED_POINTS_DISTANCE`

Inquires the reserved distance around the occurrence, defined as a percentage of the radius. Points found in this area are not considered in the fit, and cannot be considered for other occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_RESERVED_POINTS_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_LENGTH`

Inquires the tolerance for the length of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_LENGTH`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_RADIUS`

Inquires the tolerance for the radius of a nominal cylinder model ([`M_CYLINDER`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_RADIUS`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find rectangular plane 3D model finder context ID with a rectangular plane model`

Specifies a find rectangular plane 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_RECTANGULAR_PLANE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a rectangular plane model.

#### `M_ACCEPTANCE`

Inquires the acceptance level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_CERTAINTY`

Inquires the certainty level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_CERTAINTY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COVERAGE_MAX`

Inquires the maximum expected model coverage.

| Value | Description |
| --- | --- |
| *(see [`M_COVERAGE_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_ELONGATION_MAX`

Inquires the maximum elongation of the rectangular plane occurrence. The elongation is defined as the maximum side/minimum side.

| Value | Description |
| --- | --- |
| *(see [`M_ELONGATION_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_ELONGATION_MIN`

Inquires the minimum elongation of the rectangular plane occurrence. The elongation is defined as the maximum side/minimum side.

| Value | Description |
| --- | --- |
| *(see [`M_ELONGATION_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_ANGLE_TOLERANCE`

Inquires the angular tolerance to use for [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md), when searching for rectangular plane occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_ANGLE_TOLERANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_CONDITION`

Inquires whether to find only rectangular plane occurrences that satisfy the specified condition, relative to the vector [`M_NORMAL_...`](../../Reference/3dmod/M3dmodInquire.md), or to find any rectangular plane occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_CONDITION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_X`

Inquires the X-component of the vector against which to compare rectangular plane occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_Y`

Inquires the Y-component of the vector against which to compare rectangular plane occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NORMAL_Z`

Inquires the Z-component of the vector against which to compare rectangular plane occurrences, as specified using [`M_NORMAL_CONDITION`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_Z`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER`

Inquires the maximum number of occurrences for which to search.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_OF_POINTS_MIN`

Inquires the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_X`

Inquires the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_X_MAX`

Inquires the maximum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_X_MIN`

Inquires the minimum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Y`

Inquires the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Y_MAX`

Inquires the maximum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SIZE_Y_MIN`

Inquires the minimum size of a range-type rectangular plane model ([`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_X`

Inquires the tolerance for the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_X`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_Y`

Inquires the tolerance for the size of a nominal rectangular plane model ([`M_RECTANGLE`](../../Reference/3dmod/M3dmodInquire.md)) along its Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_Y`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find sphere 3D model finder context ID with a sphere model`

Specifies a find sphere 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SPHERE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a sphere model.

#### `M_ACCEPTANCE`

Inquires the acceptance level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_CERTAINTY`

Inquires the certainty level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_CERTAINTY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COVERAGE_MAX`

Inquires the maximum expected model coverage.

| Value | Description |
| --- | --- |
| *(see [`M_COVERAGE_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER`

Inquires the maximum number of occurrences for which to search.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_OF_POINTS_MIN`

Inquires the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RADIUS`

Inquires the radius of a nominal sphere model ([`M_SPHERE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RADIUS_MAX`

Inquires the maximum radius of a range-type sphere model ([`M_SPHERE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RADIUS_MIN`

Inquires the minimum radius of a range-type sphere model ([`M_SPHERE_RANGE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RESERVED_POINTS_DISTANCE`

Inquires the reserved distance around the occurrence, defined as a percentage of the radius. Points found in this area are not considered in the fit, and cannot be considered for other occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_RESERVED_POINTS_DISTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TOLERANCE_RADIUS`

Inquires the tolerance for the radius of a nominal sphere model ([`M_SPHERE`](../../Reference/3dmod/M3dmodInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_TOLERANCE_RADIUS`](Reference/3dmod/M3dmodControl.md))* |  |

---

### `Find surface or planar surface 3D model finder context ID with a surface model`

Specifies a find surface or planar surface 3D model finder context, allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) or [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), and used in [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operations. It must contain a surface model.

#### `M_ACCEPTANCE`

Inquires the acceptance level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_ACCEPTANCE_COLOR`

Inquires the acceptance level for the color score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_COLOR`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_ACCEPTANCE_TARGET`

Inquires the acceptance level for the target score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_TARGET`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_CERTAINTY`

Inquires the certainty level for the score.

| Value | Description |
| --- | --- |
| *(see [`M_CERTAINTY`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_COVERAGE_MAX`

Inquires the maximum expected model coverage.

| Value | Description |
| --- | --- |
| *(see [`M_COVERAGE_MAX`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_FIT_SCORE_MIN`

Inquires the minimum expected occurrence fit score. Note that the corresponding control type has no effect unless [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodInquire.md)is set to[`M_ENABLE`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIT_SCORE_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_MODEL_PLANES_OCCLUSION_MODE`

Inquires the occlusion level of the planar regions of the model in the scene.  Note that this inquire type is only available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_PLANES_OCCLUSION_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_MODEL_RANGE_SIZE_X`

Inquires the size in X of the range component of the model point cloud.  Note that you can use this value to allocate an image buffer of an appropriate size to pass to [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_GRIP_LABEL_IMAGE`](../../Reference/3dmod/M3dmodCopy.md) or [`M_WEIGHT_IMAGE`](../../Reference/3dmod/M3dmodCopy.md).  Note that this inquire type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the X-size, in pixels. |

#### `M_MODEL_RANGE_SIZE_Y`

Inquires the size in Y of the range component of the model point cloud.  Note that you can use this value to allocate an image buffer of an appropriate size to pass to [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_GRIP_LABEL_IMAGE`](../../Reference/3dmod/M3dmodCopy.md) or [`M_WEIGHT_IMAGE`](../../Reference/3dmod/M3dmodCopy.md).  Note that this inquire type is not available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the Y-size, in pixels. |

#### `M_MODEL_RESOLUTION`

Inquires the resolution of the model when it was originally defined (before preprocessing); the resolution is expressed as the average distance between points.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the resolution of the defined model, where the resolution is expressed as the average distance between points. |

#### `M_NUMBER`

Inquires the maximum number of occurrences for which to search.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_OF_POINTS_MIN`

Inquires the minimum number of points per occurrence found.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS_MIN`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_NUMBER_RESTING_PLANE`

Inquires the number of resting planes defined using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_RESTING_PLANE`](../../Reference/3dmod/M3dmodCopy.md).

| Value | Description |
| --- | --- |
| `0 <= Value <= 1` | Specifies the number of resting planes. |

#### `M_REMOVE_OUTLIERS`

Inquires whether to perform outlier removal on the model.

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_OUTLIERS`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_REMOVE_OUTLIERS_SEARCH_MODE`

Inquires the search mode for finding outliers when [`M_REMOVE_OUTLIERS`](../../Reference/3dmod/M3dmodInquire.md) is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_REMOVE_OUTLIERS_SEARCH_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_RESTING_PLANE_ANGLE_TOLERANCE`

Inquires the angular tolerance for the resting plane constraint.

| Value | Description |
| --- | --- |
| *(see [`M_RESTING_PLANE_ANGLE_TOLERANCE`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_SEARCH_POINT_RESOLUTION`

Inquires the resolution of the target point cloud (scene), where the resolution is expressed as the average distance between points.

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_POINT_RESOLUTION`](Reference/3dmod/M3dmodControl.md))* |  |

#### `M_TARGET_PLANES_OCCLUSION_MODE`

Inquires the target occlusion level when a planar region of the model is a portion of a planar region of an object in the scene.  Note that this inquire type is only available for find planar surface 3D model finder contexts.

| Value | Description |
| --- | --- |
| *(see [`M_TARGET_PLANES_OCCLUSION_MODE`](Reference/3dmod/M3dmodControl.md))* |  |

### Combination Constants — For inquiring about the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get information about the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

#### `M_IS_SET_TO_DEFAULT`

Inquires whether the specified inquire type is set to its default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not set to its default value. |
| `M_TRUE` | Specifies that the inquire type is set to its default value. |

### Combination Constants — For inquiring whether an inquire type has a default value or whether it is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type has a default value or whether it is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
