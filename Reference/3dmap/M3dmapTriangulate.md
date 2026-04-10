---
doctype: Reference
module: 3dmap
function: M3dmapTriangulate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapTriangulate"
---

# M3dmapTriangulate

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

> Perform stereo vision triangulation to find the 3D world coordinates of points from their pixel location in the image plane of two or more cameras.

## Syntax

```c
void M3dmapTriangulate(
    const AIL_ID *     ContextCalOrImageBufIdArrayPtr,  //in
    const AIL_DOUBLE * PixelCoordXArrayPtr,             //in
    const AIL_DOUBLE * PixelCoordYArrayPtr,             //in
    AIL_DOUBLE *       WorldCoordXArrayPtr,             //out
    AIL_DOUBLE *       WorldCoordYArrayPtr,             //out
    AIL_DOUBLE *       WorldCoordZArrayPtr,             //out
    AIL_DOUBLE *       RMSErrorArrayPtr,                //out
    AIL_INT            NumCalibrations,                 //in
    AIL_INT            NumPoints,                       //in
    AIL_INT64          CoordinateSystem,                //in
    AIL_INT64          ControlFlag                      //in
)
```

## Description

This function performs stereo vision triangulation (stereophotogrammetry) to calculate the X-, Y-, and Z-world coordinates of points (features) that can be seen by two or more cameras. You must identify the pixel location of the points in the image plane of the different cameras. Each of the camera setups must be calibrated in 3D mode using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) so that the position of the cameras in the world is known. To calculate each point's location in the world, the point's location in each camera's image plane and the camera's location in the world are used.

With a single call to [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md), you can calculate the world coordinates of multiple points, even if a point does not appear in the image plane of all cameras. Each point must be seen by at least two cameras. To indicate that a point is not seen by a camera, use **M_INVALID_POINT**. For example, to determine the world coordinates of the following two points on an object that is seen by three cameras, you would pass the following arguments to [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md):

*[Image: 3dmap_triangulation_pair_points.png]*

*[Image: 3dmap_triangulation_array.png]*

> **Note:** The camera calibration contexts of the camera setups must have been allocated in 3D mode using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) and they must share a common world coordinate system. For more information about camera calibration, see [Camera calibration](../../UserGuide/C28_Calibration/ChapterInformation.md).

Although results can be obtained with only two cameras, rounding and extraction errors can affect the accuracy of the calculations. Using more than two cameras will increase the robustness of the function.

For points that could not be computed, their corresponding entries in the coordinate output arrays will be set to **M_INVALID_POINT** and their corresponding entries in the [`RMSErrorArrayPtr`](../../Reference/3dmap/M3dmapTriangulate.md) array will be set to 0.0. This can occur when all projection lines from the image planes to the point are perfectly parallel and therefore do not intersect.

## Parameters

### `ContextCalOrImageBufIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of an array containing the identifiers of the camera calibration contexts for each of the camera setups. Alternatively, the array can contain the identifiers of calibrated or corrected images; the operation uses the camera calibration contexts associated with the images.

### `PixelCoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-pixel coordinates of the common points in the image plane of each camera. This array must have a size equal to ([`NumCalibrations`](../../Reference/3dmap/M3dmapTriangulate.md) x [`NumPoints`](../../Reference/3dmap/M3dmapTriangulate.md)).

### `PixelCoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-pixel coordinates of the common points in the image plane of each camera. This array must have a size equal to ([`NumCalibrations`](../../Reference/3dmap/M3dmapTriangulate.md) x [`NumPoints`](../../Reference/3dmap/M3dmapTriangulate.md)).

### `WorldCoordXArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of an array in which to write the calculated X-world coordinates. This array must have a size equal to [`NumPoints`](../../Reference/3dmap/M3dmapTriangulate.md).

### `WorldCoordYArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to write the calculated Y-world coordinates. This array must have a size equal to [`NumPoints`](../../Reference/3dmap/M3dmapTriangulate.md).

### `WorldCoordZArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to write the calculated Z-world coordinates. This array must have a size equal to [`NumPoints`](../../Reference/3dmap/M3dmapTriangulate.md).

### `RMSErrorArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to write the root mean square (RMS) error for the calculated world coordinates of each point. This array must have a size equal to [`NumPoints`](../../Reference/3dmap/M3dmapTriangulate.md).

### `NumCalibrations` *(in, AIL_INT)*

Specifies the number of camera calibration contexts. This should correspond to the number of cameras. This parameter must be greater or equal to 2.

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points to compute using stereo vision triangulation. This parameter must be greater or equal to 1.

### `CoordinateSystem` *(in, AIL_INT64)*

Specifies the world coordinate system that is common to all the specified camera calibration contexts.

*For specifying the common coordinate system*

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_COORDINATE_SYSTEM` | Specifies that the camera calibration contexts have the absolute coordinate system in common. |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies that the camera calibration contexts have the relative coordinate system in common. |
| `M_ROBOT_BASE_COORDINATE_SYSTEM` | Specifies that the camera calibration contexts have the robot base coordinate system in common. |
| `M_TOOL_COORDINATE_SYSTEM` | Specifies that the camera calibration contexts have the tool coordinate system in common. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
