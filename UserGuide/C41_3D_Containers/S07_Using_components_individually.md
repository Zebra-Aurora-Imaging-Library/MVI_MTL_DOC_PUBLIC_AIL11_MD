---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Containers
section: Using_components_individually
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Containers / Using components individually"
---

# Using components individually

Typically, you will not need to access the components of a 3D-processable point cloud or depth map container directly. Most 3D processing and analysis functionality that your application will require can be achieved using Aurora Imaging Library 3D processing functions to perform operations on the container as a whole. There are advanced use-cases for accessing components directly, such as altering the confidence component to limit a processing operation to a particular set of points, or displaying grabbed 3D data as an image in a 2D display.

Because a component is an Aurora Imaging Library buffer, you can inquire its Aurora Imaging Library identifier (using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_ID`](../../Reference/buf/MbufInquireContainer.md)). You can then use the component with any Aurora Imaging Library function that takes a buffer. This feature can be a powerful tool, for example allowing you to process 3D data using 2D processing functions (such as thresholding the Z-coordinates stored in the range component using [`MimClip`](../../Reference/im/MimClip.md)), or analyze 3D data using 2D analysis functions (such as performing blob analysis on the Z-coordinates stored in the range component using [`MblobCalculate`](../../Reference/blob/MblobCalculate.md)). Manipulating components directly also means taking on additional responsibility in your application, because Aurora Imaging Library can no longer guarantee that 3D-processable point cloud or depth map containers will remain 3D-processable.

> **Note:** To learn the required layout of data in a buffer depending on its component type, see [Working with non-compliant cameras](../C42_Grabbing_from_3D_sensors/S05_Working_with_noncompliant_cameras.md).

## Safe practices for working with components directly

Aurora Imaging Library functions that work with a container might free and reallocate components. You should therefore not depend on the Aurora Imaging Library identifier of a component remaining static in your application. Typically, you should re-inquire a component's Aurora Imaging Library identifier after calling any Aurora Imaging Library function that uses the container as a destination. You should also assume that any changes you have made to the buffer (such as controlling settings or assigning an ROI) will not be retained.

The following will not be kept if a component is freed and reallocated:

- Calibration.
- Hooked functions.
- LUT associations.
- Regions of interest.
- Settings (including 3D settings).
- Memory address.

> **Note:** If a component that has been selected to a 2D display is freed and reallocated, by default Aurora Imaging Library will automatically select the newly allocated component to the 2D display. You can disable this behavior using [`MappControl`](../../Reference/app/MappControl.md)with [`M_COMPONENT_AUTO_RESELECT`](../../Reference/app/MappControl.md) and [`M_COMPONENT_AUTO_RESELECT_DISABLE`](../../Reference/app/MappControl.md).

### Child buffers

Typically, you should not create child buffers of components. Doing so can lead to undefined behavior. If you need to create a child buffer of a component, you should free the child buffer before calling any Aurora Imaging Library function that might free or reallocate components in the container.

### Created components

If you created a component mapped to user-allocated memory using [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md), and that component is reallocated because the container is used as a destination for an Aurora Imaging Library function, the newly allocated component is no longer mapped to the specified memory. You are responsible for freeing the underlying memory after the mapped component is freed or reallocated by Aurora Imaging Library.

## Ensuring that components are not reallocated

In some cases, you might need to ensure that a component of a container is not reallocated. For example, when grabbing images from a non-compliant 3D sensor that transmits 3D data in a standard image stream, you might grab directly into a component (as opposed to grabbing into its container), or grab into a non-component image buffer and use [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md)to create components mapped onto that buffer. In these cases, typically you need the components to keep the same Aurora Imaging Library identifier, attributes, and other settings.

Aurora Imaging Library will only automatically free or reallocate components when required to store the output of an Aurora Imaging Library function. Therefore, the best way to ensure that components are not freed or reallocated is to avoid using the container as a destination for any Aurora Imaging Library function. If you need to use the container as a destination (for example, to perform in-place processing), you should copy the components into another container using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md). If you need to convert the data to be 3D-processable, you can perform both operations at the same time using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

> **Note:** When grabbing into a container (as opposed to grabbing into an individual component), Aurora Imaging Library grab functions (such as [`MdigGrab`](../../Reference/dig/MdigGrab.md)) free all components of the container, even if those components are the correct size and type to hold the transmitted components. In some cases, components automatically allocated by an Aurora Imaging Library grab function will be reused for subsequent grabs.
