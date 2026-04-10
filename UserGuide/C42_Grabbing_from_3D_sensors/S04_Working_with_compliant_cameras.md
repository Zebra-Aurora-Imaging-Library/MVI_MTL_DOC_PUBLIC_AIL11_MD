---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Working_with_compliant_cameras
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Working with compliant cameras"
---

# Working with compliant cameras

If your 3D sensor transmits 3D data in a format defined by an industry standard (such as GigE Vision or GenICam), grab this data into a container with the [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md)attribute (previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md)). Typically, you can convert the 3D data to a 3D-processable point cloud or depth map container and/or 3D-displayable container using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

> **Note:** You can determine whether your 3D sensor transmits data in a format suitable for grabbing into a container using [`MdigInquire`](../../Reference/dig/MdigInquire.md)with [`M_TARGET_BUFFER_OBJECT`](../../Reference/dig/MdigInquire.md).

You can perform a single grab into a container using [`MdigGrab`](../../Reference/dig/MdigGrab.md), or you can continuously grab and process the grabbed data, without dropping frames, using [`MdigProcess`](../../Reference/dig/MdigProcess.md). You can also use[`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) to continuously acquire frames of data for use with a 2D or 3D display.

## Displaying grabbed 3D data

If you grabbed into a container allocated with the [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)attribute, you can display 3D data immediately after it is grabbed by selecting the container to a 3D display (using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md)). If the container is not 3D-displayable, but could be converted to a 3D-displayable container using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), Aurora Imaging Library will internally convert the 3D data. Typically, you will need to convert the grabbed data to a 3D-processable point cloud or depth map container using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md); in this case, it is more efficient to convert the data to a container that is also 3D-displayable, and select that container to the 3D display instead of the grab container.

> **Note:** Note that you can select an empty container to a 3D display. When you grab 3D data into the empty container, it will be shown in the 3D display immediately.

## Processing grabbed 3D data

Typically, you must convert the grabbed data for processing using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) before you can use it with Aurora Imaging Library 3D-processing functions. For more information, see[Preparing a container for display or processing](../C41_3D_Containers/S04_Preparing_a_container_for_display_or_processing.md).

Grabbed 3D data can be used with Aurora Imaging Library 3D-processing functions immediately if the following conditions are all met:

- The 3D sensor is configured to transmit data in a format that is 3D-processable.
- The grab container was allocated with both the [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md) and [`M_PROC`](../../Reference/buf/MbufAllocContainer.md) attributes.
- The 3D settings (such as shear, offset, and scale) of the range component are set to their default values (except for [`M_3D_DISTANCE_UNIT`](../../Reference/buf/MbufControlContainer.md)). You can find these settings in the table [`Component3DSettings`](../../Reference/buf/MbufControlContainer.md).
  Typically, transmitted components have non-default values for 3D settings; Aurora Imaging Library applies these settings to the underlying data when the container is passed as a source to[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). For example if [`M_3D_SCALE_X`](../../Reference/buf/MbufControlContainer.md)is set to 1.5, [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) scales the coordinates by 1.5 in the destination and sets [`M_3D_SCALE_X`](../../Reference/buf/MbufControlContainer.md) to the default value of 1 in the destination container.

You can inquire whether a point cloud or depth map container is 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) or [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), respectively.

> **Note:** You can still apply 3D settings (such as shear, offset, or scale) to a 3D-processable point cloud or depth map container by setting the 3D settings for the range component (using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)) and passing the container as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).
