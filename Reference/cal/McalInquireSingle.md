---
doctype: Reference
module: cal
function: McalInquireSingle
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalInquireSingle"
---

# McalInquireSingle

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

> Inquire about a single pose in a sequence of poses taken during camera calibration.

## Syntax

```c
AIL_INT McalInquireSingle(
    AIL_ID    ContextCalOrCalibratedAilObjectId,  //in
    AIL_INT   Index,                              //in
    AIL_INT64 InquireType,                        //in
    void *    UserVarPtr                          //out
)
```

## Description

This function allows you to inquire about the settings of a single pose in the sequence of poses performed to calibrate [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration contexts. Every call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) corresponds to a pose used to obtain information about the camera setup during the camera calibration process.

## Parameters

### `ContextCalOrCalibratedAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context.

### `Index` *(in, AIL_INT)*

Specifies the index of the pose of an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context from which settings should be inquired. The first call corresponds to the index value 0 and the last one is the return value of [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_NUMBER_OF_CALIBRATION_POSES`](../../Reference/cal/McalInquire.md) minus one.

### `InquireType` *(in, AIL_INT64)*

Specifies the setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to return the value of the inquired setting. Since the [`McalInquireSingle`](../../Reference/cal/McalInquireSingle.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For a camera calibration context being calibrated using McalGrid()

The [`InquireType`](../../Reference/cal/McalInquireSingle.md) parameter can be set to one of the following values only after calibrating with [`McalGrid`](../../Reference/cal/McalGrid.md).

---

### `M_COLUMN_NUMBER`

Inquires the number of columns in the camera calibration grid.

| Value | Description |
| --- | --- |
| `Value >= 2` | Specifies the number of columns. |

---

### `M_COLUMN_SPACING`

Inquires the number of world units between columns.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the spacing between columns. |

---

### `M_GRID_ORIGIN_X`

Inquires the X-position of the top-left circle of a circle grid, or the top-left point connecting four squares in a chessboard grid, in the camera calibration plane coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-position, in the camera calibration plane coordinate system. |

---

### `M_GRID_ORIGIN_Y`

Inquires the Y-position of the top-left circle of a circle grid, or the top-left point connecting four squares in a chessboard grid, in the camera calibration plane coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-position, in the camera calibration plane coordinate system. |

---

### `M_GRID_ORIGIN_Z`

Inquires the Z-position of the top-left circle of a circle grid, or the top-left point connecting four squares in a chessboard grid, in the camera calibration plane coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-position, in the camera calibration plane coordinate system. |

---

### `M_GRID_TYPE`

Inquires the type of grid used to perform the camera calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CHESSBOARD_GRID` | Specifies a chessboard grid. |
| `M_CIRCLE_GRID` *(default)* | Specifies a grid of circles. |
| `M_Y_AXIS_CLOCKWISE` *(default)* | Orients the positive Y-axis 90° clockwise from the X-axis. |
| `M_Y_AXIS_COUNTER_CLOCKWISE` | Orients the positive Y-axis 90° counter-clockwise from the X-axis. |
| `M_FAST` | Speeds up the time it takes to perform the camera calibration by using a faster camera calibration algorithm. |

---

### `M_GRID_UNIT_SHORT_NAME`

Inquires the units returned by [`M_GRID_UNITS`](../../Reference/cal/McalInquireSingle.md). You can find the particular abbreviation for each unit in the description of [`M_GRID_UNITS`](../../Reference/cal/McalInquireSingle.md).

---

### `M_GRID_UNITS`

Inquires the units of measurement encoded in a chessboard grid with a fiducial.  > **Note:** Note that this information is not used by the calibration module. No unit consistency is enforced and no unit conversion is performed.

| Value | Description |
| --- | --- |
| `M_CENTIMETERS` | Specifies that the grid units are measured in centimeters. |
| `M_FEET` | Specifies that the grid units are measured in feet. |
| `M_INCHES` | Specifies that the grid units are measured in inches. |
| `M_KILOMETERS` | Specifies that the grid units are measured in kilometers. |
| `M_METERS` | Specifies that the grid units are measured in meters. |
| `M_MICROMETERS` | Specifies that the grid units are measured in micrometers. |
| `M_MILES` | Specifies that the grid units are measured in miles. |
| `M_MILLIMETERS` | Specifies that the grid units are measured in millimeters. |
| `M_MILS` | Specifies that the grid units are measured in mils. |
| `M_UNKNOWN` | Specifies that grid units are measured in an unknown unit. |

---

### `M_ROW_NUMBER`

Inquires the number of rows in the camera calibration grid.

| Value | Description |
| --- | --- |
| `Value >= 2` | Specifies the number of rows. |

---

### `M_ROW_SPACING`

Inquires the spacing between rows in the camera calibration grid.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the spacing between rows, in world units. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### For a camera calibration context being calibrated using McalGrid() or McalList()

The [`InquireType`](../../Reference/cal/McalInquireSingle.md) parameter can be set to one of the following values after a call to either [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md).

---

### `M_AVERAGE_PIXEL_ERROR`

Inquires the average camera calibration error in the pixel coordinate system.  This is the average distance in the pixel coordinate system between the initial calibration points and their projected points in an image.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the average camera calibration error, in pixels. |

---

### `M_AVERAGE_WORLD_ERROR`

Inquires the average camera calibration error in the absolute coordinate system.  This is the average distance in the absolute coordinate system between the initial calibration points and their projected points in an image.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the average camera calibration error, in world units. |

---

### `M_CALIBRATION_IMAGE_POINTS_X`

Inquires the X-pixel coordinate of the calibration points.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points' pixel coordinates are determined by the pixel positions of the centers of the circles in a circle grid or the intersections of four squares/rectangles in a chessboard grid.  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' pixel coordinates with the [`PixCoordXArrayPtr`](../../Reference/cal/McalList.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in pixels. |

---

### `M_CALIBRATION_IMAGE_POINTS_Y`

Inquires the Y-pixel coordinate of the calibration points.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points' pixel coordinates are determined by the pixel positions of the centers of the circles in a circle grid or the intersections of four squares/rectangles in a chessboard grid.  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' pixel coordinates with the [`PixCoordYArrayPtr`](../../Reference/cal/McalList.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in pixels. |

---

### `M_CALIBRATION_INPUT_DATA`

Specifies the type of data that was used to perform the camera calibration.

| Value | Description |
| --- | --- |
| `M_GRID` | Specifies that the camera calibration was performed using a camera calibration grid ([`McalGrid`](../../Reference/cal/McalGrid.md)). |
| `M_LIST` | Specifies that the camera calibration was performed by explicitly specifying the correspondence between some pixels and their real-world coordinates ([`McalList`](../../Reference/cal/McalList.md)). |

---

### `M_CALIBRATION_WORLD_POINTS_X`

Inquires the X-world coordinate of the calibration points that are computed based on explicitly specified values. The coordinate is expressed in world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)).  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' world coordinates with the [`WorldCoordXArrayPtr`](../../Reference/cal/McalList.md) parameter.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points are computed from the parameters [`GridOffsetX`](../../Reference/cal/McalGrid.md), [`ColumnNumber`](../../Reference/cal/McalGrid.md), and [`ColumnSpacing`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in real-world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). |

---

### `M_CALIBRATION_WORLD_POINTS_Y`

Inquires the Y-world coordinate of the calibration points that are computed based on explicitly specified values. The coordinate is expressed in world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)).  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' world coordinates with the [`WorldCoordYArrayPtr`](../../Reference/cal/McalList.md) parameter.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points are computed from the parameters [`GridOffsetY`](../../Reference/cal/McalGrid.md), [`RowNumber`](../../Reference/cal/McalGrid.md), and [`RowSpacing`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in real-world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). |

---

### `M_CALIBRATION_WORLD_POINTS_Z`

Inquires the Z-world coordinate of the calibration points that are based on explicitly specified values. The computed calibration points are expressed in world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). If this value is inquired for 2D-based camera calibration contexts, the specified array will be filled with 0.0 values.  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' pixel coordinates with the [`WorldCoordZArrayPtr`](../../Reference/cal/McalList.md) parameter.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, you explicitly set the Z-coordinate of all the calibration points with the [`GridOffsetZ`](../../Reference/cal/McalGrid.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate, in real-world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). |

---

### `M_MAXIMUM_PIXEL_ERROR`

Inquires the maximum camera calibration error, in pixels.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum camera calibration error, in pixels. |

---

### `M_MAXIMUM_WORLD_ERROR`

Inquires the maximum camera calibration error, in world units.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum camera calibration error, in world units. |

---

### `M_NUMBER_OF_CALIBRATION_POINTS`

Inquires the number of calibration points found by [`McalGrid`](../../Reference/cal/McalGrid.md) or passed to [`McalList`](../../Reference/cal/McalList.md).  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you can explicitly set the number of calibration points with the [`NumPoint`](../../Reference/cal/McalList.md) parameter.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the number of calibration points is determined by the number of columns and rows in your grid.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of calibration points. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
