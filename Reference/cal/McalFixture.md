---
doctype: Reference
module: cal
function: McalFixture
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalFixture"
---

# McalFixture

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

> Move the relative coordinate system or learn the fixturing offset.

## Syntax

```c
void McalFixture(
    AIL_ID     DstCalibratedAilObjectId,  //out
    AIL_ID     FixturingOffsetCalId,      //in
    AIL_INT64  Operation,                 //in
    AIL_INT64  LocationType,              //in
    AIL_ID     CalOrLocSourceId,          //in
    AIL_DOUBLE Param1,                    //in
    AIL_DOUBLE Param2,                    //in
    AIL_DOUBLE Param3,                    //in
    AIL_DOUBLE Param4                     //in
)
```

## Description

This function allows you to move the relative coordinate system to a location (position and angle) that is at a fixed offset from an object that you want to analyze. Alternatively, this function can write the values required for the move to the specified 3D transformation matrix object. To specify the location, you must pass the calculated reference location for the object and, optionally, a preestablished offset to apply to this reference location. To pass the preestablished offset, you must pass a fixturing offset object that contains the information. Use a separate call to this function to set up the fixturing offset object.

When moving the relative coordinate system ([`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md)), you can specify the reference location of the object in one of two ways, based upon some previous analysis. You can have [`McalFixture`](../../Reference/cal/McalFixture.md) use, for example, the position and angle of a model occurrence from a specified Aurora Imaging Library Pattern Matching or Model Finder result buffer. Alternatively, you can explicitly specify a location with respect to the relative coordinate system of the Aurora Imaging Library object passed to [`CalOrLocSourceId`](../../Reference/cal/McalFixture.md); internally, [`McalFixture`](../../Reference/cal/McalFixture.md) converts this location so that it is with respect to the absolute coordinate system, and then uses the calculated location to place the new relative coordinate system.

> **Note:** Note that if you know the new position and angle for the relative coordinate system in the absolute coordinate system, use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) to move it explicitly. Alternatively, you can specify the default uniform camera calibration context ([`M_DEFAULT_UNIFORM_CALIBRATION`](../../Reference/cal/McalInquire.md)) as the coordinate system in which the location is expressed ([`CalOrLocSourceId`](../../Reference/cal/McalFixture.md)). This camera calibration context's relative coordinate system is always aligned with its absolute coordinate system (no angle, no offset, no scale). Therefore, the specified location is consequently interpreted as being relative to the absolute coordinate system.

When setting up the fixturing offset object ([`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md)), [`McalFixture`](../../Reference/cal/McalFixture.md) learns the offset between a specified location and the relative coordinate system. You can specify the location using the same techniques as those available when moving the relative coordinate system. In addition, you can set it to the reference location of a specified model in its original model source image. The function then saves the offset between the location and its relative coordinate system, in the specified fixturing offset object. Typically, you have obtained the location from a training image, whereby the location is a typical reference location and the relative coordinate system is at a required offset from this location.

[`McalFixture`](../../Reference/cal/McalFixture.md) assumes that the reference location and preestablished offset use the same absolute coordinate system as the image/calibrated context in which to save the new relative coordinate system, even if their mapping to the pixel coordinate system is different.

For more information on fixturing, see [Fixturing in Aurora Imaging Library](../../UserGuide/C29_Fixturing/ChapterInformation.md).

Note that only 1- and 3-band image buffers are supported.

If you adjust the relative coordinate system of a calibrated image associated with an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, the raster information will be discarded, causing the ROI to become an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI. See [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) for more information.

## Parameters

### `DstCalibratedAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the destination camera calibration context, 3D transformation matrix object, the 3D reconstruction result buffer, or the image for which to displace the relative coordinate system. If the [`Operation`](../../Reference/cal/McalFixture.md) parameter is set to [`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md), this parameter should be set to `M_NULL`.

### `FixturingOffsetCalId` *(in, AIL_ID)*

Specifies the identifier of the fixturing offset object to set up ([`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md)) or to apply to the reference location when moving the relative coordinate system ([`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md)). The fixturing offset object must have been previously allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md). If you don't need to apply a fixturing offset when moving the relative coordinate system, set this parameter to `M_NULL`.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

### `LocationType` *(in, AIL_INT64)*

Specifies how the location used for the operation is specified.

### `CalOrLocSourceId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library object containing the location for the operation, or containing the coordinate system in which the location is expressed.

### `Param1` *(in, AIL_DOUBLE)*

Specifies information about the location used for the operation. Its definition is dependent on the specified operation and the location type.

### `Param2` *(in, AIL_DOUBLE)*

Specifies information about the location used for the operation. Its definition is dependent on the specified operation and the location type.

### `Param3` *(in, AIL_DOUBLE)*

Specifies information about the location used for the operation. Its definition is dependent on the specified operation and the location type.

### `Param4` *(in, AIL_DOUBLE)*

Specifies information about the location used for the operation. Its definition is dependent on the specified operation and the location type.

## Parameter Associations

### For specifying the location used for the operation

---

### `M_LEARN_OFFSET`

Learns the offset between the specified location and the coordinate system in which it is expressed, and saves this offset in the specified fixturing offset object.

#### `M_MODEL_MOD`

Specifies to use the location of a Model Finder model's reference axis (that is, the model's reference location) in the model source image. Note that this location is with respect to the model source image's top-left corner (the top-left corner of the original image from which the model was defined), and not the model image itself.  > **Note:** To set the reference location of the model, use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_X`](../../Reference/mod/MmodControl.md), [`M_REFERENCE_Y`](../../Reference/mod/MmodControl.md), and [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md).

#### `M_MODEL_PAT`

Specifies to use the reference location of a Pattern Matching model in its source image.  > **Note:** To set the reference location of the model, use [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_REFERENCE...`](../../Reference/pat/MpatControl.md).

#### `M_POINT_AND_ANGLE`

Specifies to use the specified point and angle as the location.

| Value | Description |
| --- | --- |
| `M_DEFAULT_UNIFORM_CALIBRATION` | Specifies to use the relative coordinate system of a uniform camera calibration where the absolute and relative coordinate systems are equal to the pixel coordinate system. |
| `Calibrated image ID` | Specifies the identifier of the calibrated image. |
| `Calibration context ID` | Specifies the identifier of the camera calibration context which defines the relative coordinate system in which the point and angle are expressed. |

#### `M_POINT_AND_DIRECTION_POINT`

Specifies to use two points to define the reference location. The first point defines the position, and the angle of the vector going from the first point to the second point defines the angle.  If there is no fixturing offset object, the origin of the new relative coordinate system is placed at the first point and the angle of the X-axis of the new relative coordinate system is set to the angle of the vector going from the first point to the second point. The Y-axis is 90° from the X-axis.

| Value | Description |
| --- | --- |
| `M_DEFAULT_UNIFORM_CALIBRATION` | Specifies to use the relative coordinate system of a uniform camera calibration where the absolute and relative coordinate systems are equal to the pixel coordinate system. |
| `Calibrated image ID` | Specifies the identifier of the calibrated image. |
| `Calibration context ID` | Specifies the identifier of the camera calibration context which defines the relative coordinate system in which the point and angle are expressed. |

#### `M_RESULT_MET`

Specifies to use the resulting position and angle of a metrology reference frame.

#### `M_RESULT_MOD`

Specifies to use the position and angle of a Model Finder model occurrence.

#### `M_RESULT_PAT`

Specifies to use the position and angle of a Pattern Matching model occurrence.

---

### `M_MOVE_RELATIVE`

Moves the relative coordinate system to a location that is at a specified offset from the specified reference location. If no fixturing offset object is specified, the relative coordinate system is placed at the specified reference location.

#### `M_LASER_3DMAP`

Specifies to use the laser line coordinate system, defined by the 3D reconstruction context, to define the location. Note that the context must have been allocated using [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) and have been calibrated using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) or [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md).

#### `M_POINT_AND_ANGLE`

Specifies to use the specified point and angle as the reference location.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the current relative coordinate system of the target calibrated image or target camera calibration context ([`DstCalibratedAilObjectId`](../../Reference/cal/McalFixture.md)). |
| `M_DEFAULT_UNIFORM_CALIBRATION` | Specifies to use the relative coordinate system of a uniform camera calibration where the absolute and relative coordinate systems are equal to the pixel coordinate system. |
| `Calibrated image ID` | Specifies the identifier of the calibrated image. |
| `Calibration context ID` | Specifies the identifier of the camera calibration context which defines the relative coordinate system in which the point and angle are expressed. |

#### `M_POINT_AND_DIRECTION_POINT`

Specifies to use two points to define the reference location. The first point defines the position, and the angle of the vector going from the first point to the second point defines the angle.  If there is no fixturing offset object, the origin of the new relative coordinate system is placed at the first point and the angle of the X-axis of the new relative coordinate system is set to the angle of the vector going from the first point to the second point. The Y-axis is 90° from the X-axis.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the current relative coordinate system of the target calibrated image or target camera calibration context ([`DstCalibratedAilObjectId`](../../Reference/cal/McalFixture.md)). |
| `M_DEFAULT_UNIFORM_CALIBRATION` | Specifies to use the relative coordinate system of a uniform camera calibration where the absolute and relative coordinate systems are equal to the pixel coordinate system. |
| `Calibrated image ID` | Specifies the identifier of the calibrated image. |
| `Calibration context ID` | Specifies the identifier of the camera calibration context which defines the relative coordinate system in which the point and angle are expressed. |

#### `M_RESULT_MET`

Specifies to use the resulting position and angle of a metrology reference frame as the reference location.

#### `M_RESULT_MOD`

Specifies to use the position and angle of a Model Finder model occurrence as the reference location.

#### `M_RESULT_PAT`

Specifies to use the position and angle of a Pattern Matching model occurrence as the reference location.

#### `M_RESULT_POINT_CLOUD_3DMAP`

Specifies to use the relative coordinate system of the 3D reconstruction result buffer to define the location.

#### `M_SAME_AS_SOURCE`

Specifies that the relative coordinate system of[`CalOrLocSourceId`](../../Reference/cal/McalFixture.md) is copied into [`DstCalibratedAilObjectId`](../../Reference/cal/McalFixture.md) and takes the fixturing offset into account (if any).

> **Note:** If an image is passed but is not associated with a camera calibration context, it will be treated as if associated with a uniform camera calibration context where there is no scaling, rotation, or offset present. Therefore, each pixel in the image will be considered to represent the same area in real-world units and it will be assumed that no non-linear distortions are present within the image.
