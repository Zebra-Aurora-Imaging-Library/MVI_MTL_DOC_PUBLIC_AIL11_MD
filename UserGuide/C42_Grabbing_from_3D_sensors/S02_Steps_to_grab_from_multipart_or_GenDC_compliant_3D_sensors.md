---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Steps_to_grab_from_multipart_or_GenDC_compliant_3D_sensors
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Steps to grab from multipart or GenDC compliant 3D sensors"
---

# Steps to grab from compliant 3D sensors

The following steps provide a typical methodology to grab into a container from 3D sensors that transmit 3D data in a format defined by an industry standard (such as GigE Vision or GenICam). You can determine whether your 3D sensor transmits data in a format suitable for grabbing directly into a container using [`MdigInquire`](../../Reference/dig/MdigInquire.md)with [`M_TARGET_BUFFER_OBJECT`](../../Reference/dig/MdigInquire.md).

1. Configure your 3D sensor to transmit the range or disparity component and associated confidence information (either transmitted in a confidence component or in the range/disparity component using an invalid data value). Aurora Imaging Library provides the following methods to perform this configuration:
   - Use Aurora Imaging Capture Works to configure your 3D sensor at runtime.
   - Use the Feature Browser to configure your 3D sensor at runtime. You can launch the Feature Browser using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md) (after allocating your digitizer in step 2).
   - Use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) to configure your 3D sensor from within your application (after allocating your digitizer in step 2). Optionally, you can preconfigure a GenICam User Set on your device and use this function to select that User Set (with the feature name "UserSetSelector") and load it (with the feature name "UserSetLoad").
   - Use Aurora Imaging Intellicam to create a DCF for your device.
2. Allocate a digitizer for your 3D sensor using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md)(specifying the appropriate DCF file if you created one).
3. Allocate a container with the [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md)attribute using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md). To display the grabbed data directly using this container, you must also specify the [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)attribute.
4. Grab into the container using[`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigProcess`](../../Reference/dig/MdigProcess.md), or [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md). All components of the container are freed and components appropriate to store the grabbed data are allocated automatically. These components will be reused for subsequent grabs, unless the 3D sensor transmits components that cannot be stored in the automatically allocated components (for example, because the components have a different size for each grab).
5. If your 3D sensor transmitted multiple sets of components (for example, 2 range components and 2 associated confidence components), allocate a child container to access a single set of components (for example, only the components associated with group 1) using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md).
6. If required, convert the container to a format that is [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), or [`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md) using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).
7. Free all your allocated objects using [`MdigFree`](../../Reference/dig/MdigFree.md), unless [`M_UNIQUE_ID`](../../Reference/dig/MdigAlloc.md) was specified during allocation.
