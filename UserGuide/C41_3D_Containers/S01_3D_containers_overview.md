---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Containers
section: 3D_containers_overview
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Containers / 3D containers overview"
---

# 3D containers overview

A **container** is an Aurora Imaging Library object that can contain buffers, called its **components**. A 3D container is a container that holds information for a single 3D scene, whereby each component stores a different piece of information of that 3D scene. For example, a 3D-processable point cloud or depth map container contains a range component (which stores coordinate information of the points), a confidence component (which stores the validity of each point and is optional for a depth map container), and might also contain an intensity component (which stores the intensity or color of each point). Containers should be used as a destination when grabbing from 3D sensors or importing 3D data, and containers with the appropriate components can be displayed and processed using Aurora Imaging Library 3D functions.

*[Image: buf_point_cloud_container.png]*

*[Image: buf_depth_map_container.png]*

You can allocate a container using[`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md). Typically, you will not need to manually allocate or free components for a container; they will be allocated and freed automatically when the container is used as a destination for a grabbing or 3D processing function. If necessary, you can allocate a component using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md), copy a buffer to an existing or new (automatically allocated) component using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md), or create a component mapped onto previously allocated memory using[`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md).

When allocating a container, you must specify the Aurora Imaging Library system on which to allocate it and the attributes it should have (such as [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md)). All image buffer components of the container will be allocated with the same attributes.

When a container is no longer required, you should release it using [`MbufFree`](../../Reference/buf/MbufFree.md).

If a container has components that prevent it from being used as a source for a 3D processing function (for example, because a 3D sensor transmitted multiple range and confidence components during a grab into the container), you can create a child container with only the required components using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md). The components in a child container are in the same memory as those in its parent container, so any changes made to those components will appear in both containers.

> **Note:** You cannot directly allocate or free components of a child container. A child container therefore cannot be passed as a destination for a grab, copy, or processing function; it can, however, be passed as a source to relevant functions.
