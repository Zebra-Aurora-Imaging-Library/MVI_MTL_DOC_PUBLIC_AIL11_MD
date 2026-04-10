---
doctype: Reference
module: 3dmod
function: M3dmodCopy
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmod / M3dmodCopy"
---

# M3dmodCopy

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

> Copy data to or from a 3D model finder context.

## Syntax

```c
void M3dmodCopy(
    AIL_ID    SrcContext3dmodId,  //in
    AIL_INT64 SrcIndex,           //in
    AIL_ID    DstAilObjectId,     //out
    AIL_INT64 DstIndex,           //in
    AIL_INT64 CopyType,           //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function copies data to or from a 3D model finder context. For example, you can copy the floor plane ([`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)) or the resting plane ([`M_RESTING_PLANE`](../../Reference/3dmod/M3dmodCopy.md)) to the 3D model finder context.

## Parameters

### `SrcContext3dmodId` *(in, AIL_ID)*

Specifies the identifier of the source find 3D model finder context, 3D geometry object, or image buffer, depending on the type of copy operation.

### `SrcIndex` *(in, AIL_INT64)*

Specifies the index of the model in the source find 3D model finder context, depending on the type of copy operation.

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the destination find 3D model finder context, 3D registration context, 3D geometry object, container, or image buffer, depending on the type of copy operation.

### `DstIndex` *(in, AIL_INT64)*

Specifies the index of the model in the destination find 3D model finder context, depending on the type of copy operation.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to M_DEFAULT.

## Parameter Associations

### For specifying the copy type

---

### `M_FLOOR`

Specifies to copy the plane that defines the floor of the scene to or from the specified find box, surface, or planar surface 3D model finder context. The floor plane provides contextual information about the position and orientation of objects within the scene.  For a find surface or planar surface 3D model finder context, you must copy a floor plane to the find surface or planar surface 3D model finder context if you want to use a resting plane. The resting plane uses the floor plane as a reference when establishing the tilt angle of the occurrence.  For a find box 3D model finder context, the floor plane is considered another possible background plane ([`M_COMPLETION_TO_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md)), so a single-faced box could be extruded to the floor plane in some situations. Refer to [Completion size when a single box face is found](../../UserGuide/C37_3D_Model_finder/S07_Search_settings_specific_to_geometric_models.md) for more information.  Note that [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) does not use the specified plane to establish the background points to remove. You can remove floor points, using [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md).

#### `M_NULL`

Specifies to remove the plane that defines the floor of the scene from the specified find box, surface, or planar surface 3D model finder context.

#### `3D plane geometry object ID from which to copy`

Specifies the identifier of a 3D plane geometry object from which to copy the floor plane. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a plane.

#### `Find box 3D model finder context ID from which to copy`

Specifies the identifier of a find box 3D model finder context from which to copy the floor plane.

#### `Find surface or planar surface 3D model finder context ID from which to copy`

Specifies the identifier of a find surface or planar surface 3D model finder context from which to copy the floor plane.

---

### `M_GRIP_LABEL_IMAGE`

Specifies to copy the grip label image to or from the specified model in the specified find surface 3D model finder context. In a grip label image, the gray value of each pixel is the grip label of the corresponding point in the model. Pixels with a value of zero represent unlabeled points. Note that the location of each pixel in the grip label image corresponds to the point at the same location in the range component of the model point cloud container.  When an occurrence of a model with a grip label image is found, a score is calculated for each labeled surface. This is useful if you want to determine the best surface of the occurrence for gripping. You can retrieve the grip scores using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_SCORE_GRIP()`](../../Reference/3dmod/M3dmodGetResult.md).

#### `M_NULL`

Specifies to remove the grip label image from the specified find surface 3D model finder context.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model. |

#### `Find surface 3D model finder context ID from which to copy`

Specifies the identifier of a find surface 3D model finder context from which to copy the grip label image.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model. |

#### `Grip label image buffer ID from which to copy`

Specifies the identifier of an image buffer from which to copy the grip label image.  The image buffer must be a 1-band, 8-bit unsigned buffer. The image buffer size must equal the size of the container (range component) used to define the model, in both X and Y. To retrieve the required size, use [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_MODEL_RANGE_SIZE_X`](../../Reference/3dmod/M3dmodInquire.md) and [`M_MODEL_RANGE_SIZE_Y`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model. |

---

### `M_MODEL`

Specifies to copy the point cloud of the model, as it was extracted before preprocessing, from the specified find surface or planar surface 3D model finder context to the specified container. The model must have been previously defined with [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md).

#### `Find surface or planar surface 3D model finder context ID from which to copy`

Specifies the identifier of the find surface or planar surface 3D model finder context.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model to copy. |

---

### `M_MODEL_PREPROCESSED`

Specifies to copy the point cloud of the preprocessed version of the model, from the specified find surface or planar surface 3D model finder context to the specified container. The model must have been previously defined using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md), and must have been called before this call.

#### `Find surface or planar surface 3D model finder context ID from which to copy`

Specifies the identifier of the find surface or planar surface 3D model finder context.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model to copy. |

---

### `M_REFINE_REGISTRATION`

Specifies to copy the refine registration settings available for use by the specified find surface or planar surface 3D model finder context.

#### `M_FIND_SURFACE_CONTEXT_REFINEMENT_FAST`

Specifies a predefined 3D registration context with settings that prioritize performance when fine-tuning results ([`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) with [`M_FIND_SURFACE_REFINEMENT_FAST`](../../Reference/3dmod/M3dmodControl.md)).

| Value | Description |
| --- | --- |
| `3D registration context identifier` | Specifies an Aurora Imaging Library 3D registration context, allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md). |
| `Internal 3D registration context ID` | Specifies the internal 3D registration context used to perform the refine registration when [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md). Retrieve this identifier by calling [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_USER_DEFINED_REGISTRATION_CONTEXT_ID`](../../Reference/3dmod/M3dmodInquire.md). |

#### `M_FIND_SURFACE_CONTEXT_REFINEMENT_PRECISE`

Specifies a predefined 3D registration context with settings that prioritize accuracy when fine-tuning results ([`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) with [`M_FIND_SURFACE_REFINEMENT_PRECISE`](../../Reference/3dmod/M3dmodControl.md)).

| Value | Description |
| --- | --- |
| `3D registration context identifier` | Specifies an Aurora Imaging Library 3D registration context, allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md). |
| `Internal 3D registration context ID` | Specifies the internal 3D registration context used to perform the refine registration when [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md). Retrieve this identifier by calling [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_USER_DEFINED_REGISTRATION_CONTEXT_ID`](../../Reference/3dmod/M3dmodInquire.md). |

#### `Find surface or planar surface 3D model finder context ID from which to copy`

Specifies the identifier of the find surface or planar surface 3D model finder context from which to copy the current refine registration settings.

| Value | Description |
| --- | --- |
| `3D registration context identifier` | Specifies an Aurora Imaging Library 3D registration context, allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md). |
| `Internal 3D registration context ID` | Specifies the internal 3D registration context used to perform the refine registration when [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) is set to [`M_USER_DEFINED_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md). Retrieve this identifier by calling [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_USER_DEFINED_REGISTRATION_CONTEXT_ID`](../../Reference/3dmod/M3dmodInquire.md). |

---

### `M_RESTING_PLANE`

Specifies to copy the resting plane to or from the specified find surface or planar surface 3D model finder context. The resting plane is used to provide contextual information about how the model occurrence lies within the scene. When matching, Aurora Imaging Library considers all poses of the occurrence unless a resting plane is specified. You can use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_RESTING_PLANE_ANGLE_TOLERANCE`](../../Reference/3dmod/M3dmodControl.md) to limit the acceptable angle of the model occurrence relative to the resting plane. This does not affect any of the scores, but it filters occurrences that do not sit on the resting plane, +/- the specified tolerance. Note that defining a resting plane does not decrease the search time because occurrences are filtered near the end of the search.  Note that to use a resting plane, you must also copy a floor plane ([`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)) to the find surface or planar surface 3D model finder context; the floor plane denotes the base of the scene. The resting plane uses the floor plane as a reference when establishing the tilt angle of the occurrence.

#### `M_NULL`

Specifies to remove the resting plane from the specified find surface or planar surface 3D model finder context.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model. |

#### `3D plane geometry object ID from which to copy`

Specifies the identifier of a 3D plane geometry object from which to copy the resting plane. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a plane.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index in which to copy the plane. |

#### `Find surface or planar surface 3D model finder context ID from which to copy`

Specifies the identifier of a find surface or planar surface 3D model finder context from which to copy the resting plane.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model to copy. |

---

### `M_WEIGHT_IMAGE`

Specifies to copy the weight image to or from the specified model in the specified find surface 3D model finder context. In a weight image, the gray value of each pixel is the weight of the corresponding point in the model. The weight image must consist of only non-zero pixels. Pixels with a value of zero will result in an error. Note that the location of each pixel in the weight image corresponds to the point at the same location in the range component of the model point cloud container.  [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) gives more importance to higher weighted points of the model when searching for occurrences. Model points that have a higher weight will also have a greater impact on the model score.

#### `M_NULL`

Specifies to remove the weight image from the specified find surface 3D model finder context.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model. |

#### `Find surface 3D model finder context ID from which to copy`

Specifies the identifier of a find surface 3D model finder context from which to copy the weight image.

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model. |

#### `Weight image buffer ID from which to copy`

Specifies the identifier of an image buffer from which to copy the weight image.  The image buffer must be a 1-band, 8-bit unsigned buffer. The image buffer size must equal the size of the container (range component) used to define the model, in both X and Y. To retrieve the required size, use [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_MODEL_RANGE_SIZE_X`](../../Reference/3dmod/M3dmodInquire.md) and [`M_MODEL_RANGE_SIZE_Y`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| `0 <= Value <M_NUMBER_MODELS` | Specifies the index of the model. |
