---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Basic_concepts
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Camera Calibration module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Camera Calibration module are:

- **Axis**. The axes of a coordinate system are used to specify the location of objects within a coordinate system. A coordinate system typically has as many axes as there are dimensions in the space it describes; these axes are typically perpendicular to each other.
- **Camera calibration plane**. Camera calibration plane is the world plane used to perform the initial camera calibration with a grid or a list of points.
- **Calibrated image**. A calibrated image is an image that has a camera calibration context associated with it.
- **Calibrated digitizer**. A digitizer with an associated camera calibration context is known as a calibrated digitizer. Images grabbed with a calibrated digitizer are automatically associated with the camera calibration context and become calibrated images as a result.
- **Calibration points**. Calibration points are points in an image with known real-world coordinates and used to establish the real-world coordinates of all other points in the image based on the selected camera calibration mode.
- **Coordinates**. Coordinates are used to define an object's location in space. Coordinates are typically expressed as a set of numbers. Each number in the set represents the object's distance from the origin along a corresponding axis.
- **Coordinate system**. Coordinate systems are used to position objects. Coordinate systems are characterized by their origin and their axes.
- **Constant pixel size**. Constant pixel size refers to uniform pixel dimensions in X and Y. With constant pixel size, transformations of results between pixel and world units remain consistent across the image, regardless of position.
- **Distortion**. Distortion is an optical effect which causes objects in an image to appear deformed.
- **Field of view**. Field-of-view refers to the largest world region visible in an image at a given resolution. It is the extent of the observable world that is seen at any moment.
- **Image plane**. The plane defined in world units on which an object's image is projected. It is located in front of the camera's effective pinhole and is perpendicular to the camera's optical axis (Z-axis).
- **Origin**. The origin of a coordinate system is its reference point. At the origin, the position along all the axes of the coordinate system is zero; positions in the coordinate system are typically returned with respect to the origin.
- **Pixel units**. Pixel units are the most basic unit in an image. Pixel units are used to measure sizes, distances, and positions in an uncalibrated image.
- **Pose**. A pose is the position and orientation of an object in space.
- **Real-world units**. Real-world units are used to measure sizes, distances, and positions as they exist in the real world.
- **Robot encoders**. Robot encoders are the parts of a robot that measure the movement of the robot's arm and return it in human-readable units.
- **Transformation**. Transformations are used to move, rotate, or scale objects within a coordinate system. Typically, these transformations are represented using matrices. Transformations can also be used to convert measurements from pixel units to real-world units or vice versa.
- **Working area**. Working area is the area of the image from which you want real-world results.
