---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Working_with_noncompliant_cameras
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Working with noncompliant cameras"
---

# Working with non-compliant cameras

If your 3D sensor transmits 3D data in a format defined by an industry standard (such as GigE Vision or GenICam), typically the data will already be correctly formatted for you to use it with Aurora Imaging Library. Otherwise, your 3D sensor is non-compliant; the data it transmits is not natively compatible with Aurora Imaging Library 3D functions, and you must refer to your 3D sensor manual to determine how 3D information is stored in the transmitted data. You will need to manually prepare this data for use with Aurora Imaging Library (possibly using a third-party SDK provided by the device manufacturer). You will also need to set the correct component type and 3D settings for the grabbed data.

## Grabbing data from non-compliant 3D sensors

If your non-compliant 3D sensor transmits 3D data using a protocol that Aurora Imaging Library can grab (for example, as a standard image stream over GigE Vision), and each frame of data contains only a single type of information (such as point cloud coordinates or a disparity map), you should grab the data directly into a component of a container. You can do this by passing the Aurora Imaging Library identifier of the component (previously allocated using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md)) to [`MdigGrab`](../../Reference/dig/MdigGrab.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md). Note that the component must be in a container with the [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md)attribute.

If your non-compliant 3D sensor transmits 3D data using a protocol that Aurora Imaging Library can grab, and each frame of data contains multiple types of information (such as coordinates, confidence, and intensity), you should grab the data into a standard image buffer. Refer to your camera manual to determine where, and in what format, each type of 3D data is stored in the image. Then, use [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md)to create components for a container with the correct attributes and component type and map them on to these areas in memory.

If your 3D sensor requires you to grab data using a third-party SDK, you need put the grabbed data in a component. Typically, you should do this by copying the values grabbed using the third-party SDK into one or more arrays. You can then pass these arrays, and the Aurora Imaging Library identifier of an image buffer component that has the appropriate component type (previously allocated using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md)) to [`MbufPut`](../../Reference/buf/MbufPut.md) or [`MbufPutColor`](../../Reference/buf/MbufPutColor.md). If your 3D sensor transmits more than one type of data, you can use this procedure with multiple components to put all of that data in the same container.

If the third-party SDK provides the ability to export 3D data in either the PLY or STL file format, you can load the 3D data from that file into a container using [`MbufLoad`](../../Reference/buf/MbufLoad.md), [`MbufRestore`](../../Reference/buf/MbufRestore.md), or[`MbufImport`](../../Reference/buf/MbufImport.md). This is useful when performance is not critical (for example, early in application development). Note that point cloud data loaded from a PLY or STL file is typically unorganized. For information on point cloud organization, see [Organized and unorganized point clouds](../C35_3D_Image_processing/S12_Organized_and_unorganized_point_clouds.md).

> **Note:** When you are working with data grabbed using a third-party SDK, if you require maximum performance and know the exact layout of the data in memory, you can map a component onto that memory using [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md)(if the layout of the data is compatible with Aurora Imaging Library). To do this, you must determine the Host address, format, and pitch of the grabbed data. If the third-party SDK continues to manipulate that space in memory after grabbing (for example, by clearing it), you should instead create a buffer on the memory using an [`MbufCreate...`](../../Reference/buf/MbufCreateColor.md)function, and then copy that buffer to a component using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md).

## Formatting grabbed data for use with MbufConvert3d

Typically, if you need to manually prepare the data transmitted by your 3D sensor to a format supported by Aurora Imaging Library, you should first determine which [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md)most closely resembles the structure of the transmitted data. You must then set the [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md) setting of the range or disparity component to this value, and perform any additional operations required for your data to fully match this setting. To learn the exact layout of data required by Aurora Imaging Library for each 3D representation, see [Layout of data in a component](S05_Working_with_noncompliant_cameras.md). You must also include confidence information, either stored in a confidence component or in the range or disparity component itself (using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)to set the [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufControlContainer.md)and [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufControlContainer.md)settings).

If your 3D sensor is supplied with a third-party SDK, you might find it easier to use that SDK to partially, or completely, format your 3D data before putting it into a component. In other cases, you should put the data in a component immediately and manipulate it using Aurora Imaging Library functions (such as [`MimArith`](../../Reference/im/MimArith.md)). The best strategy will depend upon the format in which the data is transmitted, and the functionality provided by the third-party SDK.

Once the grabbed 3D data is stored in a container and correctly formatted for use by Aurora Imaging Library, you should convert the container to a 3D-processable point cloud, 3D-processable depth map, or 3D-displayable container using[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

## Layout of data in a component

Aurora Imaging Library expects the data in a component to be laid out in a specific way depending on its component type. Many component types are also related to one another. For example, in a point cloud container, the same position in the range component and confidence component should describe information about the same point; therefore, the data in these components must be aligned. For this reason, all 3D components in a 3D container must have the same [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)(aside from the mesh component, which you should typically not access directly).

In a 3D-processable point cloud or depth map container, no one component stores the points; for example, the range component stores coordinate information of the points, the confidence component stores the validity of each point, and it might contain an intensity component, which stores the intensity or color of each point.

*[Image: buf_point_cloud_container.png]*

*[Image: buf_depth_map_container.png]*

Aurora Imaging Library can only process 3D information stored in a container that is [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) or [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), even if the data in your components is arranged correctly. For more information, see[Preparing a container for display or processing](../C41_3D_Containers/S04_Preparing_a_container_for_display_or_processing.md).

The following sections describes how data must be arranged in components you might need to manipulate directly.

### Range

An [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md) component is an image buffer that stores the coordinates of points, or a depth map. There are many different ways that this data can be arranged; the exact layout is identified by the component's coordinate system type and 3D representation (set using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)with [`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufInquireContainer.md) and[`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md) respectively).

Aurora Imaging Library only supports range components with[`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_CARTESIAN`](../../Reference/buf/MbufInquireContainer.md). For this coordinate system type, each pixel-position in the image buffer stores, at most, XYZ-coordinates for a single point. The range component's 3D representation identifies how many bands it must have, and how the XYZ-coordinates are stored in those bands.

All 3D representations, with the exception of [`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufControlContainer.md), are for organized 3D data. This means it is assumed that points close to each other in 3D space are also close to each other in the pixel coordinate system. Typically, disparity components and organized range components should have [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)greater than 1. If your 3D sensor transmits range or disparity data for multiples lines in a single row (for example, multiple profiles from a laser profiler), you can make it organized by splitting that data at a known interval to put it in multiple rows.

> **Note:** In most cases, using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)with a container that has a range component with an organized 3D representation, but only single row of data, will not produce a correct result. Similarly, specifying an organized 3D representation for 3D data that is unorganized can lead to invalid results when the converted container is passed as a source to Aurora Imaging Library processing functions.

The following table describes the required layout for each 3D representation type:

| 3D representation | Bands | Description |
| --- | --- | --- |
| [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufControlContainer.md) | 3 | X-coordinates are stored in band 0, Y-coordinates are stored in band 1, and Z-coordinates are stored in band 2. |
| [`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufControlContainer.md) | 3 | X-coordinates are stored in band 0, Y-coordinates are stored in band 1, and Z-coordinates are stored in band 2. The data is unorganized, meaning that it is stored in a single row, with no implied association between pixel-position and 3D position. Since the coordinates are unorganized,[`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md) must be 1. |
| [`M_CALIBRATED_XZ_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | 3 | X-coordinates are stored in band 0 and Z-coordinates are stored in band 2 (band 1 is not used). The Y-coordinate for each pixel row is stored in a separate 1-dimensional Aurora Imaging Library array buffer that stores one value per row in the component. This [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer should not be part of the container, and there is no component type for this buffer. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). |
| [`M_CALIBRATED_XZ_UNIFORM_Y`](../../Reference/buf/MbufControlContainer.md) | 3 | X-coordinates are stored in band 0 and Z-coordinates are stored in band 2 (band 1 is not used). Y-coordinates are stored implicitly by the point's row position in the pixel coordinate system (multiplied by [`M_3D_SCALE_Y`](../../Reference/buf/MbufInquireContainer.md)). |
| [`M_CALIBRATED_Z`](../../Reference/buf/MbufControlContainer.md) | 1 | Z-coordinates are stored explicitly in the buffer. There are no XY-coordinates in the 3D data stored with this setting. |
| [`M_CALIBRATED_Z_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | 1 | Z-coordinates are stored explicitly in the buffer. The Y-coordinate for each pixel row is stored in a separate 1-dimensional Aurora Imaging Library array buffer that stores one value per row in the component. This [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer should not be part of the container, and there is no component type for this buffer. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). There are no X-coordinates in the 3D data stored with this setting. |
| [`M_CALIBRATED_Z_UNIFORM_X_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | 1 | Z-coordinates are stored explicitly in the buffer. X-coordinates are stored implicitly by the point's column position in the pixel coordinate system (multiplied by [`M_3D_SCALE_X`](../../Reference/buf/MbufInquireContainer.md)). The Y-coordinate for each pixel row is stored in a separate 1-dimensional Aurora Imaging Library array buffer that stores one value per row in the component. This [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer should not be part of the container, and there is no component type for this buffer. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). |
| [`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md) | 1 | Z-coordinates are stored explicitly in the buffer. XY-coordinates are stored implicitly by the point's column and row position in the pixel coordinate system respectively (multiplied by [`M_3D_SCALE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_SCALE_Y`](../../Reference/buf/MbufInquireContainer.md)). This is also sometimes referred to as a depth map. |
| [`M_PROJECTED_Z`](../../Reference/buf/MbufControlContainer.md) | 1 | Z-coordinates are stored explicitly in the buffer. XY-coordinates are stored implicitly using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and either the point's column and row position in the pixel coordinate system or the A and B coordinate maps provided by the camera, respectively. This is also sometimes referred to as a projective map. |
| [`M_PROJECTED_Z_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | 1 | Z-coordinates are stored explicitly in the buffer. X-coordinates are stored implicitly using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and either the point's column position in the pixel coordinate system or the A coordinate map provided by the camera. The Y-coordinate for each pixel row is stored in a separate 1-dimensional Aurora Imaging Library array buffer that stores one value per row in the component. This [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer should not be part of the container, and there is no component type for this buffer. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). This is also sometimes referred to as a projective map. |
| [`M_UNCALIBRATED_Z`](../../Reference/buf/MbufControlContainer.md) | 1 | Z-coordinates are stored explicitly in the buffer. These coordinates are not natively calibrated. There are no XY-coordinates in the 3D data stored with this setting. |

### Disparity

An [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufInquireContainer.md) component is a 1-band image buffer that stores a disparity map. A disparity map stores the difference in the apparent position of objects in the 2 images produced by a stereoscopic camera. Pixels with higher values indicate that the object shown is further apart in the two images, meaning that the object is closer to the camera.

There are many different ways that this data can be arranged; the exact layout is identified by the component's 3D representation (set using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)with[`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md)).

A disparity map does not explicitly store coordinates, but you can use [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) to convert a container with a disparity map to a point cloud container. Depending on the 3D representation of the disparity component, the Y-coordinates in the resulting range component will be calculated differently.

> **Note:** To correctly convert a container with a disparity component to a point cloud container, you must correctly set the disparity-specific 3D settings (using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md) with control type settings that start with [`M_3D_DISPARITY_...`](../../Reference/buf/MbufControlContainer.md)) for the disparity component. The correct values should be provided by your camera manufacturer.

The following table describes the required layout for each 3D representation type:

| 3D representation | Description |
| --- | --- |
| [`M_DISPARITY`](../../Reference/buf/MbufControlContainer.md) | Y-coordinates are calculated by applying a perspective projection to the image. Typically, this representation type should be used with disparity components that have been generated using areascan cameras. |
| [`M_DISPARITY_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | The Y-coordinate for each pixel row is stored in a separate 1-dimensional Aurora Imaging Library array buffer that stores one value per row in the component. This [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer should not be part of the container, and there is no component type for this buffer. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). Typically, this representation type should only be used with disparity components that have been generated using linescan cameras. |
| [`M_DISPARITY_UNIFORM_Y`](../../Reference/buf/MbufControlContainer.md) | Y-coordinates for each pixel are stored implicitly by row index in the pixel coordinate system (multiplied by [`M_3D_SCALE_Y`](../../Reference/buf/MbufInquireContainer.md)). Typically, this representation type should only be used with disparity components that have been generated using linescan cameras. |

### Confidence

An [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufInquireContainer.md) component is a 1-band image buffer that stores confidence information. When a confidence component is part of a 3D container, it stores the probability that each point (or each pixel of a disparity component) is accurate. In Aurora Imaging Library, 3D information associated with a confidence value of 0 is considered invalid and is not used by 3D image processing or analysis functions.

The confidence component must have the same [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md)and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range or disparity component.

> **Note:** Confidence information can also be indicated within the range or disparity component itself. If an invalid data value is enabled and set (using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md) with [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufInquireContainer.md) respectively) for the range or disparity component, any point with the Z-coordinate value (or disparity map pixel value) equivalent to the invalid data value will be treated as invalid data when the container is passed to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

### Intensity

An [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufInquireContainer.md)component is an image buffer that stores a standard 2D image. When an intensity component is part of a 3D container, it stores color information for each point (or each pixel of a disparity component).

The intensity component must have the same [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md)and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range or disparity component.

> **Note:** Unlike the other component types, an intensity component is often used to store a photographic image that is not related to the range or disparity component (for example, the image from which the laser line was extracted if the 3D data was acquired using a laser profiler). In this case, the intensity component should not be used as part of a 3D container, because it does not store values for the points in the container.

### Reflectance

An [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufInquireContainer.md) component is an image buffer that stores a reflectance map. The exact usage of this component varies depending on the 3D sensor used to generate the data. When a reflectance component is part of a 3D container, it stores intensity information for each point (for example, the intensity of the laser line at each point if the 3D data was acquired using a laser profiler). Unlike an intensity component, a reflectance map is typically only used to store values for points, rather than a photographic image.

The reflectance component must have the same [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md)and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range or disparity component.

### Normals

An [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md) component is a 3-band floating-point image buffer that stores a unit vector indicating a direction for each point. Typically, normals indicate what direction is perpendicular to the surface of the object at each point (or, in rare cases, each pixel of a disparity component).

The XYZ-components of the normal vector for each point are stored in band 0,1, and 2 of the buffer, respectively. Each normal vector must be normalized to 1, or be set to 0 in all bands (indicating that there is no normal vector stored for that point). You can use [`M3dimFix`](../../Reference/3dim/M3dimFix.md) with [`M_NORMALS_NORMALIZED`](../../Reference/3dim/M3dimFix.md) to normalize the normal vectors to 1 if this condition is not met.

The normals component must have the same [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md)and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range or disparity component.

### Region

An [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufInquireContainer.md) component is a mask image buffer that defines a region of interest (ROI) in a depth map container. When a region component is part of a depth map container, non-zero values represent points in the range component that are part of the ROI. In Aurora Imaging Library, 3D image processing functions only operate on points inside the ROI.

The region component must have the same [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md)and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range component.

## Examples

The following code snippet shows how to create components on an image buffer containing 3D data, grabbed from a non-compliant 3D sensor:

> **Code example:** [userguide.grabbing_from_3d_sensors.creating_components_on_3d_data_in_a_standard_image_stream](userguide.grabbing_from_3d_sensors.creating_components_on_3d_data_in_a_standard_image_stream)

The following code snippet shows how to put 3D data (grabbed using a third-party SDK) in the components of a container:

> **Code example:** [userguide.grabbing_from_3d_sensors.grabbing_with_a_third_party_SDK_and_using_MbufPut](userguide.grabbing_from_3d_sensors.grabbing_with_a_third_party_SDK_and_using_MbufPut)
