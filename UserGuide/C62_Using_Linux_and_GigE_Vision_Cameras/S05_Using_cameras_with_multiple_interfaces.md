---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_Linux_and_GigE_Vision_Cameras
section: Using_cameras_with_multiple_interfaces
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / using-linux-and-gigE-vision-cameras / Using cameras with multiple interfaces"
---

# Using cameras with multiple interfaces

Some cameras use multiple interfaces to augment the available bandwidth, allowing you to stream very large images at a higher frame rate. If you use such a camera without any special configuration, you will still be able to control the camera but half of the data you receive will be missing. This is because the network stack will consider all packets received on the second interface to belong to someone else and drop them. The solution is to put both interfaces in the same bridge, bond, or team as described in [Connecting multiple cameras](S04_Connecting_multiple_cameras.md). If you have multiple multi-interface cameras directly connected to your computer, you do not need to create multiple bonds. A single bond containing all interfaces connected to the cameras should suffice.
