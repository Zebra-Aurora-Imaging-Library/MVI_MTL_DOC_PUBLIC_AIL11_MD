---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Basic_concepts_for_grabbing_from_3D_sensors
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Basic concepts for grabbing from 3D sensors"
---

# Basic concepts for grabbing from 3D sensors

The basic concepts and vocabulary conventions when dealing with 3D sensors are:

- **3D sensor**. A device that transmits 3D information.
- **Compliant 3D sensor**. A 3D sensor that transmits 3D data in a format compliant with an industry standard (such as GigE Vision or GenICam), suitable for grabbing into a container.
- **Non-compliant 3D sensor**. A 3D sensor that transmits 3D data in a non-standard format. Some of these devices use an image streaming protocol supported by Aurora Imaging Library (such as Camera Link) to transmit 3D data; the format is typically described in the device's manual. Others transmit 3D data that must be grabbed using a third-party SDK supplied by the device manufacturer.
- **Range component**. A component with the component type [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md). A range component stores 3D coordinates or a depth map.
- **Disparity component**. A component with the component type [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufInquireContainer.md). A disparity component store a disparity map, typically grabbed from a stereoscopic camera.
- **Depth map**. An image where the gray value of a pixel represents its depth in the world.
- **Disparity map**. A monochrome 2D image that stores the difference (measured in pixels) between the apparent positions of objects, as seen by each image sensor of a stereoscopic camera. This information can be used to determine the distance of each object from the camera, and therefore constitutes 3D information.
- **Confidence information**. Information about the accuracy of 3D data. Aurora Imaging Library treats any point of a point cloud or pixel of a depth map or disparity map that is associated with 0 percent confidence as invalid data.
- **Laser profiler**. A 3D sensor that generates 3D data by measuring the displacement of a laser line projected onto an object of interest. Typically, the object is on a conveyor belt and the profiler takes multiple slices before compiling them into a single point cloud.
- **Stereoscopic camera**. A 3D sensor that generates 3D data by capturing images from 2 image sensors simultaneously and comparing the positions of objects in the two images. This is the same principle that human vision uses to obtain 3D information.
- **Structured light camera**. A 3D sensor that generates 3D data by analyzing images taken while projecting several calibrated light patterns.
- **Time-of-flight sensor**. A 3D sensor that generates 3D data by emitting light with a specific wavelength and measuring (for each pixel) the time taken for that light to bounce off a surface and hit the image sensor.
