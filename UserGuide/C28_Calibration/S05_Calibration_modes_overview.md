---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Calibration_modes_overview
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Calibration modes overview"
---

# Camera calibration modes

When you use [`McalAlloc`](../../Reference/cal/McalAlloc.md) to allocate the camera calibration context, you have to specify the camera calibration mode. Aurora Imaging Library supports the following camera calibration modes:

- [Uniform mode:](S06_2D_Calibration_modes.md) The most basic camera calibration mode available.
- 2D modes:
  - [Piecewise linear interpolation mode:](S06_2D_Calibration_modes.md) Compensates for any kind of distortion and is very accurate for points located inside the working area.
  - [Perspective transformation mode:](S06_2D_Calibration_modes.md) Compensates for rotation, translation, scale, and perspective distortions. The perspective transformation mode is accurate for points inside and outside the working area.
- 3D modes:
  - [Tsai-based mode:](S07_3D_Calibration_modes.md) A true 3D camera calibration based on a technique developed by Roger Y. Tsai. This mode determines the distance and orientation between the modeled camera and the image plane.
  - [Zhang-based mode:](S07_3D_Calibration_modes.md) A true 3D camera calibration based on a technique developed by Z. Zhang. This mode is similar to the Tsai-based mode with less restrictions on the optical axis and FOV coverage but requires an accumulation phase of grid images or calibration points.
  - [Robotics mode with a Tsai-based camera model:](S07_3D_Calibration_modes.md) Estimates the position and orientation of the robot base coordinate system with respect to the absolute coordinate system for a moving or stationary camera setup using a Tsai-based camera model. This mode allows for the retrieval of the position and orientation of the robot base coordinate system.
  - [Robotics mode with a Zhang-based camera model:](S07_3D_Calibration_modes.md) This mode is similar to the robotics mode with a Tsai-based camera model, but the camera's intrinsic parameters are determined separately from the position and orientation estimations of the robot base coordinate system.

> **Note:** Note that during development and at runtime, 2D and 3D calibration support requires the presence of different Aurora Imaging Library licenses that grant access to either the 2D or 3D calibration package. Access to both is granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to the required package separately.

The following table provides a side-by-side feature comparison of the different camera calibration modes supported by Aurora Imaging Library:

| Feature | Uniform | Linear interpolation | Perspective transformation | Tsai-based | Zhang-based | Robotics (Tsai-based and Zhang-based) |
| --- | --- | --- | --- | --- | --- | --- |
| Supports camera movement parallel to the image plane | X | X | X | X | X | X |
| Supports full 3-dimensional camera movement | X | X | X |
| Supports working with coordinates that lie outside the calibration grid | X | (less accurate) | X | X | X | X | X |
| Calculates the position of the robot base | X | X |
| Calibrates with a single image only | (optional) | X | X | X |
| Allows calibrating with the camera perpendicular to the camera calibration plane (0º angle of incidence) | X | X | X | X | X | X |
| Supports telecentric lenses | X | X | X |
| Supports one-dimensional distortions obtained from line-scan cameras | X | X | X |
| Corrects rotation, scale, and perspective distortion | X | X | X | X | X | X |
| Corrects lens distortion | X | X | X | X | X |
| Corrects all types of distortion | X |
| Corrects lens distortion only and keeps other distortions uncorrected | X | X | X | X |
| Models the camera explicitly | X | X | X | X |
| Estimates the position of the camera and other known objects using known world points | X | X | X | X |
| Allows retrieval of the position and orientation of the robot's base coordinate system | X | X |
| Allows proper calibration of cameras used for 3D reconstruction by [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) and [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md) functions | X | X | X |
