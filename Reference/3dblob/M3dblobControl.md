---
doctype: Reference
module: 3dblob
function: M3dblobControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobControl"
---

# M3dblobControl

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

> Control a 3D blob analysis context or result buffer setting.

## Syntax

```c
void M3dblobControl(
    AIL_ID     ContextOrResult3dblobId,  //out
    AIL_INT64  LabelOrIndex,             //in
    AIL_INT64  ControlType,              //in
    AIL_DOUBLE ControlValue              //in
)
```

## Description

This function controls the settings of a 3D blob analysis context or result buffer.

You can inquire most of these settings using [`M3dblobInquire`](../../Reference/3dblob/M3dblobInquire.md).

To control draw 3D blob analysis settings for [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md), use [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md) instead.

## Parameters

### `ContextOrResult3dblobId` *(out, AIL_ID)*

Specifies the identifier of the 3D blob analysis context or result buffer to control. The 3D blob analysis context or result buffer must have been previously allocated on the system using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) or [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md), respectively.

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of the blob to control, or specifies to control all blobs. This parameter must be set to one of the following values:

*For specifying the blob label or index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If a 3D blob analysis context is specified, same as [`M_CONTEXT`](../../Reference/3dblob/M3dblobControl.md).

If a 3D blob analysis result buffer is specified, same as [`M_GENERAL`](../../Reference/3dblob/M3dblobControl.md). |
| `M_BLOB_INDEX` | Specifies the index of the blob. |
| `M_BLOB_LABEL` | Specifies the label of the blob. |
| `M_ALL_BLOBS` | Specifies to apply the control setting to all blobs. |
| `M_CONTEXT` | Specifies to control a general setting of a 3D blob analysis context. |
| `M_GENERAL` | Specifies to control a general setting of a 3D blob analysis result buffer. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For specifying the control type and control value for a 3D blob analysis context or result buffer

The following [`ContextOrResult3dblobId`](../../Reference/3dblob/M3dblobControl.md), [`ControlType`](../../Reference/3dblob/M3dblobControl.md), and [`ControlValue`](../../Reference/3dblob/M3dblobControl.md) parameter settings can be specified for different types of 3D blob analysis contexts or result buffers.

---

### `Calculate 3D blob analysis context ID`

Specifies a calculate 3D blob analysis context. allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_CALCULATE_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md), and used in [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) operations.

#### `M_ALL_FEATURES`

Sets whether to calculate all blob features. You can then selectively enable or disable specific features for calculation.  It is not recommended to use [`M_ALL_FEATURES`](../../Reference/3dblob/M3dblobControl.md) in your final application. This is because programs using [`M_ALL_FEATURES`](../../Reference/3dblob/M3dblobControl.md) set to [`M_ENABLE`](../../Reference/3dblob/M3dblobControl.md) might become slower when new Aurora Imaging Library versions are released, if new features have been added.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies to disable the calculation of all features. |
| `M_ENABLE` | Specifies to enable the calculation of all features. |

#### `M_BOUNDING_BOX`

Sets whether to calculate the axis-aligned bounding box and its related Ferets. You can retrieve results for these features using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_BOX_CENTER_...`](../../Reference/3dblob/M3dblobGetResult.md), [`M_FERET_X`](../../Reference/3dblob/M3dblobGetResult.md), [`M_FERET_Y`](../../Reference/3dblob/M3dblobGetResult.md), [`M_FERET_Z`](../../Reference/3dblob/M3dblobGetResult.md), [`M_MAX_...`](../../Reference/3dblob/M3dblobGetResult.md), [`M_MIN_...`](../../Reference/3dblob/M3dblobGetResult.md), and [`M_SIZE_...`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the axis-aligned bounding box and its related Ferets. |
| `M_ENABLE` | Specifies to calculate the axis-aligned bounding box and its related Ferets. |

#### `M_CENTROID`

Sets whether to calculate the centroid. This is equivalent to enabling [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) with an order of at least 1. You can retrieve results for this feature using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_CENTROID_...`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the centroid. |
| `M_ENABLE` | Specifies to calculate the centroid. |

#### `M_FERET_CONTACT_POINTS`

Sets whether to save the contact points at each Feret; the contact points are the coordinates of the Feret diameter's start and end points. You can retrieve results for these features using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_FERET_...`](../../Reference/3dblob/M3dblobGetResult.md) and [`M_FERET_CONTACT_POINTS_...`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to save the contact points at each Feret. |
| `M_ENABLE` | Specifies to save the contact points at each Feret. |

#### `M_FERET_GENERAL`

Sets whether to calculate the Feret along a custom direction specified with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) + [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobControl.md). You can retrieve results for this feature using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the Feret along a custom direction. |
| `M_ENABLE` | Specifies to calculate the Feret along a custom direction.  > **Note:** Note that you must specify the direction vector with [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobControl.md). |

#### `M_FERET_MAX_DIAMETER`

Sets whether to calculate the longest Feret. Note that the Feret's direction is accurate up to the angle specified with [`M_FERET_PRECISION`](../../Reference/3dblob/M3dblobControl.md). You can retrieve results for this feature using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_FERET_MAX_DIAMETER`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the longest Feret. |
| `M_ENABLE` | Specifies to calculate the longest Feret. |

#### `M_FERET_MIN_DIAMETER`

Sets whether to calculate the shortest Feret. Note that the Feret's direction is accurate up to the angle specified with [`M_FERET_PRECISION`](../../Reference/3dblob/M3dblobControl.md). You can retrieve results for this feature using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_FERET_MIN_DIAMETER`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the shortest Feret. |
| `M_ENABLE` | Specifies to calculate the shortest Feret. |

#### `M_FERET_PRECISION`

Sets the accuracy when calculating the minimum and maximum Feret directions. The smaller the specified angle, the longer the calculation will take.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 90.0` *(default)* | Specifies the precision (in degrees). |

#### `M_LINEARITY`

Sets whether to calculate the degree to which the blobs are linear. This is equivalent to enabling [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) with an order of at least 2. You can retrieve results for this feature using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_LINEARITY`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the linearity. |
| `M_ENABLE` | Specifies to calculate the linearity. |

#### `M_MOMENT_ORDER`

Sets the order up to which moments are calculated. For example, setting this to 3 will calculate third order moments, but also second and first order moments.  Note that there are `(_N_+1)(_N_+2)/2` moments for order _N_. For example, if [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md) is set to 3, the number of computed moments are 10, 6, and 3 for third, second, and first order moments, respectively.  See [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) for the moments feature results that are available, depending on the order specified with [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the moment order. |

#### `M_MOMENTS`

Sets whether to calculate all moments up to [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md). You can retrieve results for these features using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_MOMENT_CENTRAL_XYZ()`](../../Reference/3dblob/M3dblobGetResult.md), and [`M_MOMENT_XYZ()`](../../Reference/3dblob/M3dblobGetResult.md). If the moment order is set to at least 1, you can also retrieve [`M_CENTROID_...`](../../Reference/3dblob/M3dblobGetResult.md) results; if the moment order is set to at least 2, you can also retrieve [`M_COVARIANCE_MATRIX`](../../Reference/3dblob/M3dblobGetResult.md), [`M_EIGENVALUE_...`](../../Reference/3dblob/M3dblobGetResult.md), [`M_LINEARITY`](../../Reference/3dblob/M3dblobGetResult.md), [`M_PLANARITY`](../../Reference/3dblob/M3dblobGetResult.md), and [`M_PRINCIPAL_AXIS_...`](../../Reference/3dblob/M3dblobGetResult.md) results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the moments. |
| `M_ENABLE` | Specifies to calculate the moments. |

#### `M_NEAREST_BLOB`

Sets whether to calculate features related to the nearest blob. You can retrieve results for these features using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobGetResult.md), [`M_NEAREST_BLOB_DISTANCE`](../../Reference/3dblob/M3dblobGetResult.md), and [`M_NEAREST_POINT_...`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate features related to the nearest blob. |
| `M_ENABLE` | Specifies to calculate features related to the nearest blob. |

#### `M_PCA`

Sets whether to calculate the principal components, which are used in a principal component analysis (PCA) or when calculating [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md). This is equivalent to enabling [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) with an order of at least 2. You can retrieve results for these features using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_PRINCIPAL_AXIS_...`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the principal components. |
| `M_ENABLE` | Specifies to calculate the principal components. |

#### `M_PCA_BOX`

Sets whether to calculate the PCA-aligned bounding box and its related Ferets. You can retrieve results for these features using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_BOX_CENTER_...`](../../Reference/3dblob/M3dblobGetResult.md), [`M_FERET_AT_PRINCIPAL_AXIS_...`](../../Reference/3dblob/M3dblobGetResult.md), and [`M_SIZE_...`](../../Reference/3dblob/M3dblobGetResult.md).  The PCA-aligned bounding box is typically a tighter fit than other boxes (for example, [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md)) for highly planar or linear objects.  > **Note:** Note that enabling [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md) automatically enables [`M_PCA`](../../Reference/3dblob/M3dblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the PCA-aligned bounding box and its related Ferets. |
| `M_ENABLE` | Specifies to calculate the PCA-aligned bounding box and its related Ferets. |

#### `M_PLANARITY`

Sets whether to calculate the degree to which the blobs are planar. This is equivalent to enabling [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) with an order of at least 2. You can retrieve results for this feature using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_PLANARITY`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the planarity. |
| `M_ENABLE` | Specifies to calculate the planarity. |

#### `M_SEMI_ORIENTED_BOX`

Sets whether to calculate the semi-oriented bounding box and its related Ferets. You can retrieve results for these features using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_BOX_CENTER_...`](../../Reference/3dblob/M3dblobGetResult.md), [`M_FERET_SEMI_ORIENTED_HEIGHT`](../../Reference/3dblob/M3dblobGetResult.md), [`M_FERET_SEMI_ORIENTED_WIDTH`](../../Reference/3dblob/M3dblobGetResult.md), [`M_FERET_Z`](../../Reference/3dblob/M3dblobGetResult.md), [`M_MAX_Z`](../../Reference/3dblob/M3dblobGetResult.md), and [`M_MIN_Z`](../../Reference/3dblob/M3dblobGetResult.md).  The semi-oriented bounding box is the minimum volume box that is axis-aligned in Z but not in X and Y. This box is useful when the objects lay flat in 1 dimension (for example, on a conveyor).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the semi-oriented bounding box and its related Ferets. |
| `M_ENABLE` | Specifies to calculate the semi-oriented bounding box and its related Ferets. |

---

### `Segmentation 3D blob analysis context ID`

Specifies a segmentation 3D blob analysis context. allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_SEGMENTATION_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md), and used in [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operations.

#### `M_COLOR_DISTANCE_MAX`

Sets the maximum difference between the color of two points for them to be considered neighbors. Set how to calculate the distance with [`M_COLOR_DISTANCE_MODE`](../../Reference/3dblob/M3dblobControl.md). This control type is available when the component [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) or [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md) exists in the source container. If you set a non-infinite value, and neither a reflectance nor intensity component exists, am Aurora Imaging Library error is reported.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies not to consider the color. |
| `Value > 0.0` | Specifies the maximum color difference. |

#### `M_COLOR_DISTANCE_MODE`

Sets how to calculate the difference in color between two points. If the intensity or reflectance component is monochrome, both modes calculate the same color difference.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EUCLIDEAN` *(default)* | Specifies to use the Euclidean distance to calculate the color difference.  A Euclidean distance is the square root of the sum of the squared differences between the color of each point's corresponding element in its intensity or reflectance component's 2D grid organization. |
| `M_MANHATTAN` | Specifies to use the Manhattan distance to calculate the color difference.  A Manhattan distance is the sum of the absolute value of the differences between the color of each point's corresponding element in its intensity or reflectance component's 2D grid organization. |

#### `M_CUSTOM_DISTANCE_COMPONENT`

Sets to use an extra component during the segmentation. The custom component must be 1-band and have the same size as the range component. Points are considered neighbors if their respective values in the custom component differ by less than [`M_CUSTOM_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md).

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/3dblob/M3dblobControl.md)or [`M_COMPONENT_DISPARITY`](../../Reference/3dblob/M3dblobControl.md)component of the container. |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 254. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/3dblob/M3dblobControl.md) component of the container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component stores information of an unknown type. |
| `M_NULL` *(default)* | Specifies that there is no custom component. |
| `M_COMPONENT_BY_ID` | Specifies to use the component, in the container, that has the specified buffer identifier. An error will be generated if the container does not have a component with this buffer identifier. |
| `M_COMPONENT_BY_INDEX` | Specifies to use the component, in the container, at the specified index. An error will be generated if the container does not have a component at this index. |

#### `M_CUSTOM_DISTANCE_MAX`

Sets the maximum difference within which two points are considered neighbors in the custom component. This control type is available only when you have specified a custom component with [`M_CUSTOM_DISTANCE_COMPONENT`](../../Reference/3dblob/M3dblobControl.md). If you set a non-infinite value, and the source container does not have the specified custom component, an Aurora Imaging Library error is reported.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no custom component. |
| `Value > 0.0` | Specifies the maximum difference between points in the custom component. |

#### `M_GLOBAL_NORMAL_DISTANCE_MAX`

Sets the maximum angle between the normal of a point and a blob's average normal, for the point to be added to the blob. This control type can be used to segment a point cloud into planar blobs to distinguish between relatively flat regions.  You can specify how to calculate the angle with [`M_NORMAL_DISTANCE_MODE`](../../Reference/3dblob/M3dblobControl.md). Setting a smaller maximum angle forces the blobs into more planar (flatter) regions, allowing for less variation in each blob. Since this control is solely based upon normal vectors, parallel but distinct blobs can still merge together, unless [`M_GLOBAL_PLANE_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) is specified as well.  This control type is available when an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container. If the normals component is not present, the segmentation will fail and the operation's status returns [`M_MISSING_COMPONENT_NORMALS_AIL`](../../Reference/3dblob/M3dblobGetResult.md), when requested using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_STATUS`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies not to consider the angle. |
| `0.0 < Value <= 180.0` | Specifies the maximum angle, in degrees. |

#### `M_GLOBAL_PLANE_DISTANCE_MAX`

Sets the maximum distance between a point and a blob's average plane, for the point to be added to the blob. The blob's average plane is defined by the average normal and passes through the centroid.  This control type requires that [`M_GLOBAL_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) has been set, and is used to prevent parallel planar blobs from merging together.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies not to consider the distance. |
| `Value > 0.0` | Specifies the maximum distance. |

#### `M_MAX_DISTANCE`

Sets the maximum distance within which two points are considered neighbors.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no maximum distance. Instead, the function will use [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dblob/M3dblobControl.md) to determine a point's neighborhood. |
| `Value > 0.0` | Specifies the maximum distance. |

#### `M_MAX_DISTANCE_MODE`

Sets whether to use, for the maximum distance between points, the value set with [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobControl.md) or an automatic threshold. This is the maximum distance within which two points are considered neighbors.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to automatically calculate the maximum distance. |
| `M_USER_DEFINED` *(default)* | Specifies to use the value set with [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobControl.md). |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Sets the maximum number of neighbors to consider at each point. If there are more neighbors than the specified maximum, only the closest points are kept.

| Value | Description |
| --- | --- |
| *(see [`M_MAXIMUM_NUMBER_NEIGHBORS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBOR_SEARCH_MODE`

Sets the search mode for finding the neighbors of a point.  > **Note:** Note that [`M_ORGANIZED`](../../Reference/3dblob/M3dblobControl.md) is usually faster, but is less robust to noise. [`M_TREE`](../../Reference/3dblob/M3dblobControl.md) is typically the most precise because it chooses points that are closest in 3D space, using the Euclidean distance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MESH` | Specifies to use the point cloud's mesh component to determine the neighbors of a point. A mesh's triangular faces are composed of connected neighboring points.  > **Note:** This option is available only when the source container has an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component. If the mesh component is not present, the segmentation will fail and the operation's status returns [`M_MISSING_COMPONENT_MESH_AIL`](../../Reference/3dblob/M3dblobGetResult.md), when requested using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_STATUS`](../../Reference/3dblob/M3dblobGetResult.md). |
| `M_ORGANIZED` | Specifies to use the point cloud's organizational structure to determine the neighbors of a point. This option is supported only for an organized point cloud.  When [`M_ORGANIZED`](../../Reference/3dblob/M3dblobControl.md) is specified, you must set either [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dblob/M3dblobControl.md) or [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dblob/M3dblobControl.md) to determine the extent of the neighborhood.  > **Note:** If the point cloud is unorganized, the segmentation will fail and the operation's status returns [`M_CONTAINER_NOT_ORGANIZED`](../../Reference/3dblob/M3dblobGetResult.md), when requested using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_STATUS`](../../Reference/3dblob/M3dblobGetResult.md). |
| `M_TREE` *(default)* | Specifies to use a KD tree search mode to determine a point's neighbors.  When [`M_TREE`](../../Reference/3dblob/M3dblobControl.md) is specified, you must also set [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dblob/M3dblobControl.md). |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Sets the neighborhood size, which must be an odd number, when finding the neighbors of a point in an organized point cloud. The size determines the number of elements along a row or column of the 2D buffer of the range and confidence components. For example, if you set [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dblob/M3dblobControl.md) to 7, the neighborhood is a 7-element by 7-element region in the source container's 2D grid organization (specifically, in the organized point cloud's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components).  Calculations for points close to the edge of the 2D buffer use a clipped neighborhood.  This control type is supported only when using an organized point cloud and only if [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dblob/M3dblobControl.md) is set to [`M_ORGANIZED`](../../Reference/3dblob/M3dblobControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NORMAL_DISTANCE_MAX`

Sets the maximum angle between normal vectors within which two points are considered neighbors. Specify how to calculate the angle with [`M_NORMAL_DISTANCE_MODE`](../../Reference/3dblob/M3dblobControl.md).  This control type is available when an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container. If the normals component is not present, the segmentation will fail and the operation's status returns [`M_MISSING_COMPONENT_NORMALS_AIL`](../../Reference/3dblob/M3dblobGetResult.md), when requested using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_STATUS`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies not to consider the normals. |
| `0.0 < Value <= 180.0` | Specifies the maximum angle, in degrees. |

#### `M_NORMAL_DISTANCE_MAX_MODE`

Sets whether to use, for the maximum angle between normal vectors, the value set with [`M_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) or an automatic threshold. This is the maximum angle between the normal vectors of two neighboring points for the points to be considered neighbors.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to automatically calculate the maximum angle. |
| `M_USER_DEFINED` *(default)* | Specifies to use the value set with [`M_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md). |

#### `M_NORMAL_DISTANCE_MODE`

Sets how to calculate the angle difference between the normal vectors of two points. If required, set [`M_NORMAL_DISTANCE_MODE`](../../Reference/3dblob/M3dblobControl.md) to [`M_ORIENTATION`](../../Reference/3dblob/M3dblobControl.md) to ensure that the segmentation is invariant to the normal viewpoint.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DIRECTION` *(default)* | Specifies to calculate the angle from 0 to 180 degrees. |
| `M_ORIENTATION` | Specifies to calculate the angle from 0 to 90 degrees; when specified, this control value considers flipped normals to be the same. |

#### `M_NUMBER_OF_POINTS_MAX`

Sets the maximum number of points for a blob to be considered valid. Blobs with more points than the specified value will be discarded and their points assigned the label 0. You can use [`M_NUMBER_OF_POINTS_MAX`](../../Reference/3dblob/M3dblobControl.md) to remove the background, which is often the largest blob.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no maximum number of points. |
| `Value > 0` | Specifies the maximum number of points. |

#### `M_NUMBER_OF_POINTS_MIN`

Sets the minimum number of points for a blob to be considered valid. Blobs with fewer points than the specified value will be discarded and their points will be assigned the label 0. You can use [`M_NUMBER_OF_POINTS_MIN`](../../Reference/3dblob/M3dblobControl.md) to remove blobs corresponding to a few isolated points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimum number of points. |

#### `M_RELABEL_CONSECUTIVE`

Sets whether to force the labels to be consecutive from 1 to [`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md). When calling [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with a label image, this will recompute the labels instead of using those from the label image.  [`M_RELABEL_CONSECUTIVE`](../../Reference/3dblob/M3dblobControl.md) sets each blob label to the blob's index + 1.  Note that this operation will change the value of [`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to force the labels to be consecutive. |
| `M_ENABLE` | Specifies to force the labels to be consecutive. |

#### `M_SEGMENTATION_MODE`

Sets the type of segmentation to perform.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EUCLIDEAN` *(default)* | Specifies to segment the point cloud based on the (Euclidean) distance between points. Use the other segmentation context control settings to modify the segmentation criteria. |
| `M_LABEL_IMAGE` | Specifies to copy a label image into the specified result buffer ([`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md)). You can use [`M_LABEL_IMAGE`](../../Reference/3dblob/M3dblobControl.md) to copy an external segmentation (for example, a label image created using the 2D Blob Analysis module). All segmentation control types except [`M_NUMBER_OF_POINTS_MAX`](../../Reference/3dblob/M3dblobControl.md), [`M_NUMBER_OF_POINTS_MIN`](../../Reference/3dblob/M3dblobControl.md), and [`M_RELABEL_CONSECUTIVE`](../../Reference/3dblob/M3dblobControl.md) are ignored in this mode.  > **Note:** Note that you can use [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with the predefined context [`M_SEGMENTATION_CONTEXT_LABEL_IMAGE`](../../Reference/3dblob/M3dblobSegment.md). In this case, all segmentation context control types are set to their default, except for [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) which is set to [`M_LABEL_IMAGE`](../../Reference/3dblob/M3dblobControl.md). |
| `M_WHOLE_IMAGE` | Specifies to group all valid points into a single blob, which allows you to perform calculations on the entire point cloud. All segmentation control types except [`M_NUMBER_OF_POINTS_MAX`](../../Reference/3dblob/M3dblobControl.md) and [`M_NUMBER_OF_POINTS_MIN`](../../Reference/3dblob/M3dblobControl.md) are ignored in this mode.  > **Note:** Note that you can use [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with the predefined context [`M_SEGMENTATION_CONTEXT_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobSegment.md). In this case, all segmentation context control types are set to their default, except for [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) which is set to [`M_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobControl.md). |

#### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) to complete the blob segmentation operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

---

### `Segmentation 3D blob analysis result ID, for a single blob or all blobs`

Specifies a segmentation 3D blob analysis result buffer, allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md), and used to store [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) results.

#### `?`

| Value | Description |
| --- | --- |
| `ControlValue` | Specifies the custom feature's computed value. |

#### `M_MERGE`

Merges the blob specified using the [`ControlValue`](../../Reference/3dblob/M3dblobControl.md) parameter with the one specified using the [`LabelOrIndex`](../../Reference/3dblob/M3dblobControl.md) parameter. The resulting blob retains the label of the [`LabelOrIndex`](../../Reference/3dblob/M3dblobControl.md) blob. Since this operation changes the segmentation (unlike when using [`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md), [`M3dblobSort`](../../Reference/3dblob/M3dblobSort.md), and [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md)), this result buffer becomes incompatible with sibling result buffers (for example, you cannot combine the set of blobs in this result buffer with those in sibling result buffers).  > **Note:** Note that, since the merge changes the number of blobs, the index of any blob might change.  If the two blobs contain calculated features, the merged blob will contain all features that can be trivially merged (for example, [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) and [`M_PCA`](../../Reference/3dblob/M3dblobControl.md)), but not features that would need to be recomputed from scratch (for example, minimum/maximum diameter and nearest blob). The merged blob will also never contain any custom features.  For a successful merge, both blobs must be singular; that is, [`M_ALL_BLOBS`](../../Reference/3dblob/M3dblobControl.md) must not be specified. If you want to treat all blobs as one, use [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with either the predefined context [`M_SEGMENTATION_CONTEXT_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobSegment.md) or a previously allocated [`M_SEGMENTATION_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md) context for which you have set [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) to [`M_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobControl.md).

| Value | Description |
| --- | --- |
| `M_BLOB_INDEX` | Specifies the index of the blob. |
| `M_BLOB_LABEL` | Specifies the label of the blob. |

#### `M_REMOVE_FEATURE`

Sets the custom feature(s) to remove from the blob. If a feature is not available, no error is reported.

| Value | Description |
| --- | --- |
| `M_CUSTOM_FEATURE` | Removes a custom feature from the blob(s). |
| `M_ALL` | Specifies to remove all custom features. |

---

### `Segmentation 3D blob analysis result ID, for general results`

Specifies a segmentation 3D blob analysis result buffer, allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md), and used to store [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) results.

#### `M_INDEX_MODE`

Sets how to interpret the [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter in all functions (such as [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md)) that use the 3D blob analysis result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INDEX` | Specifies that the index will be passed directly to the function's [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter, without any macros. |
| `M_LABEL` | Specifies that the label will be passed directly to the function's [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter, without any macros. |
| `M_USE_MACRO` *(default)* | Specifies that the label or index must be passed using the macro **M_BLOB_LABEL()** or **M_BLOB_INDEX()**. |

#### `M_RELABEL_CONSECUTIVE`

Relabels the blobs in consecutive order, from 1 to [`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md).  [`M_RELABEL_CONSECUTIVE`](../../Reference/3dblob/M3dblobControl.md) sets each blob label to the blob's index + 1.  Note that this operation will change the value of [`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

#### `M_RESET`

Removes all results from the segmentation 3D blob analysis result buffer. This does not delete the result buffer, as is the case with [`M3dblobFree`](../../Reference/3dblob/M3dblobFree.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

#### `M_STOP_SEGMENT`

Stops the current execution of [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md). This can only be done from another thread of higher priority. Results already available in the result buffer become invalid and will be discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

### Combination Constants — For specifying the custom Feret direction

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the direction along which to calculate the custom Feret.

#### `M_FERET_DIRECTION_X`

Sets the X-component of the direction vector.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the X-component of the direction vector. |

#### `M_FERET_DIRECTION_Y`

Sets the Y-component of the direction vector.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the Y-component of the direction vector. |

#### `M_FERET_DIRECTION_Z`

Sets the Z-component of the direction vector.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the Z-component of the direction vector. |

### Combination Constants — For specifying the maximum distance to the nearest blob

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the maximum distance to the nearest blob.

#### `M_MAX_DISTANCE`

Sets the maximum distance within which Aurora Imaging Library considers a nearby blob to be the closest.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no distance constraint. |
| `Value > 0.0` | Specifies the maximum distance. If the nearest blob is further than the specified value, the returned label will be 0. |

In this case, you must set [`LabelOrIndex`](../../Reference/3dblob/M3dblobControl.md) to [`M_CONTEXT`](../../Reference/3dblob/M3dblobControl.md) or [`M_DEFAULT`](../../Reference/3dblob/M3dblobControl.md).
