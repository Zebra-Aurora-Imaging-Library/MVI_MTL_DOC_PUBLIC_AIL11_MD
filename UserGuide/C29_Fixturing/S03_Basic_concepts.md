---
doctype: UserGuide
part: "2D related information"
chapter: Fixturing
section: Basic_concepts
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / fixturing / Basic concepts"
---

# Basic concepts for fixturing

The basic concepts and vocabulary conventions for fixturing are:

- **Absolute coordinate system (also known as absolute world coordinate system)**. The coordinate system in the world from which all other world coordinate systems are defined and to which the pixel coordinate system maps. The absolute coordinate system is implicitly defined from the calibration points specified when calibrating the camera setup. If the default uniform camera calibration context is used instead of calibration points, the absolute coordinate system is set, by default, to have the same origin and orientation as the pixel coordinate system and to have world units equal to 1 pixel. The absolute coordinate system is unmovable. Its unit of measure is user-defined (for example, mm, cm, or inches).
- **Calibrated image**. An image associated with a camera calibration context. Upon association, the image receives its own copy of the world coordinate systems and extrinsic camera attributes; it receives a reference to the camera calibration context for all other settings, including the intrinsic camera attributes. If the coordinate systems of the camera calibration context are changed after the association, the change will not affect the image. The image only refers back to the camera calibration context for non-uniform pixel-to-world mapping information and for information about the original camera calibration setup.
- **Camera calibration context**. An Aurora Imaging Library object that stores the mapping between the pixel coordinate system and the world coordinate system, as well as information about the camera calibration setup, and the initial locations of all the coordinate systems.
- **Camera calibration**. Relates the pixel coordinate system to the absolute coordinate system.
- **Fixturing offset object**. An Aurora Imaging Library object that stores the learned positional and angular offset required between the relative coordinate system and the calculated reference location of an object.
- **Location**. A position and angle, expressed in a specified coordinate system.
- **Pixel units**. The most basic unit in an image. Pixel units are used to measure sizes, distances, and positions in an uncalibrated image.
- **World units (also known as real-world units)**. Units used to measure sizes, distances, and positions as they exist in the real world.
- **Relative coordinate system (also known as relative world coordinate system)**. Coordinate system defined in the absolute coordinate system and used to set spatial information and retrieve spatial results in real-world units. The relative coordinate system can be moved anywhere in the absolute coordinate system to set spatial information and retrieve spatial results relative to a different location in the world. Before modification, the relative coordinate system is equivalent to the absolute coordinate system.
- **Reference location**. A location that represents the location of an object (for example, the center of an object) and is at a fixed offset from the object.
- **Training image**. A typical image used to set up processing, analysis, or a fixturing offset object, at the beginning of your application (outside of any time critical loop).
