---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Calibrating_using_calibration_points_from_a_list
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Calibrating using calibration points from a list"
---

# Calibrating using calibration points from a list

[`McalList`](../../Reference/cal/McalList.md) allows you to explicitly specify the calibration points as a list of pixel coordinates and their associated real-world coordinates. The more coordinates you specify, the more accurate the mapping. [`McalList`](../../Reference/cal/McalList.md) can be used when you explicitly know the real-world coordinates for a given set of pixel coordinates. The specified pixel coordinates should cover the area of the image from which you want real-world coordinates (the working area).

Based on your camera calibration mode, [`McalList`](../../Reference/cal/McalList.md) requires a certain minimum number of points to calibrate correctly. The following table shows the minimum number of points that [`McalList`](../../Reference/cal/McalList.md) requires to perform a full calibration given a specific camera calibration mode.

|   |   |
| --- | --- |
| Camera calibration mode | Minimum number of points required for a full calibration |
| Uniform | 3 |
| Piecewise linear interpolation | 3 |
| Perspective transformation | 4 |
| Tsai-based (with no z-coordinate) | 5 |
| Tsai-based (with z-coordinate) | 7 |
| Zhang-based | 7 |
| Tsai-based robotics | 5 |
| Zhang-based robotics | 7 |

In the case of perspective distortion, knowing the world coordinates of 4 points in the image gives sufficient information to create a mapping function. To create a good mapping for radial distortion, the Camera Calibration module requires a larger number of coordinates (for example, more than 30) distributed over the image.

Unlike when using [`McalGrid`](../../Reference/cal/McalGrid.md), [`McalList`](../../Reference/cal/McalList.md) does not use a default orientation for the Y-axis of the absolute coordinate system. It establishes the orientation based on the specified calibration points. It arbitrarily selects three of the specified points (for example, point A, B, and C). It then establishes the direction that a line between point A and B would rotate to meet a line between point A and C while using the smallest angle of rotation. It finally selects the Y-axis orientation for the absolute coordinate system so that the rotation direction in the pixel coordinate system and in the absolute coordinate system are the same. To inquire the orientation of the Y-axis, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_Y_AXIS_DIRECTION`](../../Reference/cal/McalInquire.md).

*[Image: cal_points_list_Y_axis.png]*
