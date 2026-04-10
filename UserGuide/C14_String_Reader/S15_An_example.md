---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: An_example
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / An example"
---

# String Reader example

The String Reader example _MStr.cpp_ illustrates how to define a font from an image with a mosaic of sample Quebec license plates.

*[Image: QcPlates.png]*

Two string models are then defined and their controls are set to read two valid types of Quebec license plates: three letters followed by three numbers, or three numbers followed by three letters. A valid license plate reading is then performed in a target image containing a car on the road.

*[Image: LicPlate.png]*

The example also displays the score of the string that was read, and the duration of the read operation.

> **Code example:** [MStr.cpp](MStr.cpp)

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.
