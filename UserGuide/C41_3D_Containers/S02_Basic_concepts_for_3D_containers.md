---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Containers
section: Basic_concepts_for_3D_containers
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Containers / Basic concepts for 3D containers"
---

# Basic concepts for 3D containers

The basic concepts and vocabulary conventions when dealing with 3D containers are:

- **Container**. An Aurora Imaging Library object that contains one or more buffers, called its components.
- **3D Container**. A container that stores 3D data in a format supported by Aurora Imaging Library.
- **Component**. A buffer that has been allocated in a container.
- **Child container**. A container that references a subset of the components of a parent container. The contents of a child container are determined dynamically when the parent container changes by applying a filter to its components based on a list of criteria, specified when the child container is allocated.
- **3D settings**. The component settings that affect how 3D data in a range or disparity component is interpreted during conversion to a 3D-processable or 3D-displayable container. These settings are listed in [`Component3DSettings`](../../Reference/buf/MbufInquireContainer.md).
- **Component type**. The component setting which indicates what type of information the component stores.
- **Point cloud container **. A container that stores a point cloud in a format supported by Aurora Imaging Library (either for 3D-processing, 3D-display, or conversion).
- **3D-processable point cloud container **. A point cloud container that can be passed as a source and/or destination to Aurora Imaging Library 3D processing functions. You can inquire if a point cloud container is 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md).
- **3D-processable depth map container **. A depth map container that can be passed as a source and/or destination to Aurora Imaging Library 3D processing functions. You can inquire if a depth map container is 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md).
- **3D-displayable container **. A 3D container that can be selected to an Aurora Imaging Library 3D display. You can inquire if a container is 3D-displayable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with [`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md).
- **Range Component**. A component with the component type [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md). A range component stores the coordinates of the points of a point cloud, or the depths of the pixels of a depth map.
- **Disparity Component**. A component with the component type [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufAllocComponent.md). A disparity component stores the apparent distance (typically measured in pixel units) between where an object appears in the left and right images captured by a stereoscopic camera.
- **Intensity Component**. A component with the component type [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md). An intensity component stores a standard image.
- **Reflectance Component**. A component with the component type [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md). A reflectance component stores the intensity of reflected light for each point or pixel of the range or disparity component.
