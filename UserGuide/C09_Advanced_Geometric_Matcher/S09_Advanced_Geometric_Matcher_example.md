---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Advanced_Geometric_Matcher_example
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Advanced Geometric Matcher example"
---

# Advanced Geometric Matcher examples

The following example demonstrates how to define a model and find model occurrences in target images.

> **Code example:** [Magm.cpp](Magm.cpp)

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.

The example, _Magm.cpp_ does the following:

- Extracts a single-definition model from a source image, then finds occurrences of it on a circuit board.
  *[Image: AGM_circuit_board_occurrences.png]*
- Constructs a composite-definition model through training, then finds occurrences of it with slight variations in appearance in different target images.
  *[Image: AGM_chess_occurrences.png]*
