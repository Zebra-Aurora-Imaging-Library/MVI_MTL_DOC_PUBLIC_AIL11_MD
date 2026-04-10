---
doctype: Reference
module: 3dmet
function: M3dmetFit
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetFit"
---

# M3dmetFit

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

> Fits a 3D geometry to a point cloud or depth map.

## Syntax

```c
void M3dmetFit(
    AIL_ID     Context3dmetId,            //in
    AIL_ID     SrcContainerOrImageBufId,  //in
    AIL_INT    GeometryType,              //in
    AIL_ID     Result3dmetOr3dgeoId,      //out
    AIL_DOUBLE OutlierDistance,           //in
    AIL_INT64  ControlFlag                //in
)
```

## Description

This function fits a 3D geometry to a point cloud or depth map using an iterative fit operation. The function starts from an initial fit estimate of the size and position of the 3D geometry. The function then internally iterates through better estimates of the correct size and position of the 3D geometry until a specified stop condition is reached. Upon each iteration, the function calculates the average root-mean-square (RMS) error per inlier, until a stop condition is met.

The initial fit estimate is calculated according to the mode specified using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_ESTIMATION_MODE`](../../Reference/3dmet/M3dmetControl.md). A set of inlier points is then defined according to [`OutlierDistance`](../../Reference/3dmet/M3dmetFit.md), and a new size and placement for the 3D geometry, which reduces the average RMS error per inlier, is calculated. This process then repeats until a stop condition is met.

If the initial fit estimate mode is set to [`M_FROM_GEOMETRY`](../../Reference/3dmet/M3dmetControl.md), the fit operation will use the specified geometry in the fit 3D metrology context. By default, the context contains an undefined geometry. If you try to initiate the fit operation and use the undefined geometry to determine an initial fit estimate, an error will occur. You can use [`M3dmetCopy`](../../Reference/3dmet/M3dmetCopy.md) to copy a defined 3D geometry object into the fit 3D metrology context.

If you perform a fit operation using a depth map image buffer, it is possible to specify a ROI in raster format, where only the values inside the ROI will be used in the fit. Note that missing data (or invalid points) in the depth map (or point cloud) are ignored when fitting.

When fitting, it is also possible to specify an outlier distance, beyond which no input data is considered for the fit operation.

After calling [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), you can inquire whether the operation was successful using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_STATUS_FIT`](../../Reference/3dmet/M3dmetGetResult.md), along with several other metrics. To retrieve, from a fit 3D metrology result buffer, the calculated best fit 3D geometry or the inliers, use [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md).

If statistics about the fit are not required, you can specify a 3D geometry object in which to directly copy the fit result. In this case, if the fit succeeds, [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) defines the 3D geometry with the specified type.

> **Note:** Note that 3D geometries of type [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md) and [`M_POINT`](../../Reference/3dgeo/M3dgeoInquire.md) are not supported for this function.

## Parameters

### `Context3dmetId` *(in, AIL_ID)*

Specifies the fit 3D metrology context.

*For specifying the 3D metrology fit context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to perform the fit operation using all the default settings specified for fit 3D metrology contexts in [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). |
| `Fit 3D metrology context identifier` | Specifies the identifier of a fit 3D metrology context. This context must have been previously allocated on the required system using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_FIT_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). |

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the point cloud or depth map to which to fit the 3D geometry.

*For specifying the point cloud or depth map*

| Value | Description |
| --- | --- |
| `Depth map container identifier` | Specifies the identifier of the container containing a 3D-processable depth map.

The container must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)). |
| `Depth map image buffer identifier` | Specifies the identifier of an image buffer that contains a fully corrected depth map. The image buffer must be a 1-band, 8-bit, 16-bit or 32-bit unsigned buffer and must be fully corrected (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

This image buffer can have a region of interest (ROI) associated with it, in raster format. Only values inside the ROI will be used in the fit operation. |
| `Point cloud container identifier` | Specifies the identifier of the container containing a 3D-processable point cloud.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). |

### `GeometryType` *(in, AIL_INT)*

Specifies the type of 3D geometry object to use in the fit operation. Note that 3D box and 3D point geometry objects are not supported.

*Specifies the type of geometry to fit onto the source container*

| Value | Description |
| --- | --- |
| `M_CYLINDER` | Specifies to fit a 3D cylinder geometry to the point cloud or depth map.

Fitting a 3D cylinder geometry requires at least 5 valid data points.

> **Note:** Note that the fit operation considers the 3D cylinder geometry's curved surface only. If the target cylinder in the point cloud or depth map has bases, then the operation might try to fit the 3D cylinder geometry to the data points on the bases, giving inaccurate results. You can set an [`OutlierDistance`](../../Reference/3dmet/M3dmetFit.md) to include only data points that are found within the specified distance from the 3D cylinder geometry's curved surface, preventing an erroneous fit to data points that make up a cylinder's base in the point cloud or depth map. |
| `M_LINE` | Specifies to fit a 3D line geometry to the point cloud or depth map.

Fitting a 3D line geometry requires at least 2 valid data points. |
| `M_PLANE` | Specifies to fit a 3D plane geometry to the point cloud or depth map.

Fitting a 3D plane geometry requires at least 3 valid data points. |
| `M_SPHERE` | Specifies to fit a 3D sphere geometry to the point cloud or depth map.

Fitting a 3D sphere geometry requires at least 4 valid data points. |

### `Result3dmetOr3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the fit 3D metrology result buffer in which to store the results of the fit operation, or specifies the 3D geometry object in which to copy a successful fit result.

### `OutlierDistance` *(in, AIL_DOUBLE)*

Specifies the distance from the 3D geometry, beyond which points are considered outliers and will be ignored during the fit operation.

*For specifying the distance beyond which points become outliers*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_VALUE` | Specifies to automatically compute the outlier distance. You can retrieve the calculated value using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_OUTLIER_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md).

When specified, [`M_AUTO_VALUE`](../../Reference/3dmet/M3dmetFit.md) uses the expected percentage of outliers ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_EXPECTED_OUTLIER_PERCENTAGE`](../../Reference/3dmet/M3dmetControl.md)) in the outlier distance computation. |
| `M_INFINITE` *(default)* | Specifies that no points in the point cloud are outliers, and all points will be considered inliers in every iteration of the fit operation. |
| `Value > 0.0` | Specifies the distance from the 3D geometry, beyond which points are considered outliers and will be ignored during the fit operation. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
