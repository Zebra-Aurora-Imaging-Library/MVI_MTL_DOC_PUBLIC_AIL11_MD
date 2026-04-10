---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: 3D_Reconstruction_example
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / 3D Reconstruction example"
---

# Aurora Imaging Library 3D reconstruction example

The 3D Reconstruction using laser line profiling example _m3dmap.cpp_ creates a partially corrected depth map of a piece of wood's surface and a fully corrected depth map of a cookie.

*[Image: 3dmap_example.png]*

The partially corrected depth map of the wood's surface is then used by the Blob Analysis module to find any surface defects. The fully corrected depth map of the cookie is used to compute its volume.

> **Code example:** [m3dmap.cpp](m3dmap.cpp)
