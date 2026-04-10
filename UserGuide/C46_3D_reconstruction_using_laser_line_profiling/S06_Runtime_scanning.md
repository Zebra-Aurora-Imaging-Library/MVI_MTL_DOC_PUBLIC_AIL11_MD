---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: Runtime_scanning
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / Runtime scanning"
---

# Runtime scanning settings

After you have calibrated your 3D reconstruction setup and before starting runtime scanning, you must specify the conveyor speed, using[`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_SCAN_SPEED`](../../Reference/3dmap/M3dmapControl.md). The value you give to [`M_SCAN_SPEED`](../../Reference/3dmap/M3dmapControl.md) is the conveyor displacement, in world-units, between two successive camera images.

To have the most accurate coordinates in your point cloud, this value should properly reflect both the speed at which the object is moving and the rate at which the camera captures a frame. If the conveyor speed changes while grabbing new images, or the rate at which you are grabbing images changes, you must specify a new value for [`M_SCAN_SPEED`](../../Reference/3dmap/M3dmapControl.md).

As demonstrated in the image below, if the conveyor is moving in the direction of the Y-axis of the laser line coordinate system, you must specify a positive value for the scan speed. If the conveyor is moving in the opposite direction, you must specify a negative value for the scan speed. For information about the laser line coordinate system, see [3D coordinate systems and the coordinates of a point cloud](S07_3D_coordinate_systems.md).

*[Image: 3dmap_scan_speed.png]*
