---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: A_code_example
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / A code example"
---

# A code example

The code example _MCode.cpp_decodes a Code 39 (1D) code and a Data Matrix (2D) code.

*[Image: CodeExampleImage.png]*

Specifically, _MCode.cpp_ reads two different types of codes, using regions to limit the area in which they are found, using the Code functions in conjunction with other Aurora Imaging Library functions. It then outlines the codes in the image, identifies the code types and decodes their strings, printing the results to the display.

> **Code example:** [MCode.cpp](MCode.cpp)

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.
