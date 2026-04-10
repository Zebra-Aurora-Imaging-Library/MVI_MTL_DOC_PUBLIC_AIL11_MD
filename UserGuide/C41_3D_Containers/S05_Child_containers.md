---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Containers
section: Child_containers
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Containers / Child containers"
---

# Child containers

A child container is an Aurora Imaging Library object that behaves similarly to a normal container, but refers to components in another container. Child containers are useful when you need a container that has a subset of another container's components (for example, when your 3D sensor transmits multiple sets of 3D components in a single grab). A child container is not allocated in its own memory space; its components remain part of the parent container. Any modifications to data or settings of the components in the child container affect the components in the parent and vice versa. A child container inherits its attributes (such as [`M_PROC`](../../Reference/buf/MbufAllocContainer.md)) from the parent container.

> **Note:** Depending on your application, instead of using a child container you can free the components you don't need from a container using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md), or copy the components you do need to another container using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md).

When you allocate a child container (using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md)), you must specify one or more criteria to determine the child container's contents. The contents of the child container are dynamically updated any time the parent container is modified, using the specified criteria as a filter. For example, if you allocate a child container with the criteria [`M_COMPONENT_BY_GROUP_ID()`](../../Reference/buf/MbufChildContainer.md) of a parent container that has no components, the child container will also have no components. If you subsequently grab into the parent container, Aurora Imaging Library will include any grabbed components with the specified group ID in the child container.

When a child container is no longer required, it can be released using [`MbufFree`](../../Reference/buf/MbufFree.md). Before a container can be freed, all of its child containers must also be freed.

A container can have any number of child containers. Additionally, a child container can itself have child containers. The latter might be useful for filtering the parent container multiple times. For example, you can first allocate a child container that contains all components (of the ultimate parent container) in group 2, then allocate a child of that container which contains only the range and intensity components. Because the first child container only contains the components from group 2, the second child container.

You can determine a child container's direct parent using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with [`M_PARENT_ID`](../../Reference/buf/MbufInquireContainer.md), and its ultimate parent using [`M_ANCESTOR_ID`](../../Reference/buf/MbufInquireContainer.md).

> **Note:** You cannot directly allocate or free components of a child container. A child container therefore cannot be passed as a destination for a grab, copy, or processing function; it can, however, be passed as a source to relevant functions. For example, you can pass a child container as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), but you cannot pass it as a destination (because [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)might allocate or free components in the destination container to make it 3D-processable).
