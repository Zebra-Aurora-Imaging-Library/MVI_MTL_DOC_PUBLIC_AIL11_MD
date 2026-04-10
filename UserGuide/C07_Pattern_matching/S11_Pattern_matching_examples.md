---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Pattern_matching_examples
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Pattern matching examples"
---

# Pattern matching examples

The following example demonstrates how to configure a find operation to locate a model within a target image.

> **Code example:** [mpat.cpp](mpat.cpp)

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.

The example _mpat.cpp_ does the following:

- Finds a CPU chip on a board, using a pattern matching context containing a model defined using [`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md)that has its accuracy and speed set to high (using [`MpatControl`](../../Reference/pat/MpatControl.md) with[`M_ACCURACY`](../../Reference/pat/MpatControl.md)and [`M_SPEED`](../../Reference/pat/MpatControl.md) both set to [`M_HIGH`](../../Reference/pat/MpatControl.md)).
  *[Image: mpatcircuitsboard.png]*
- Finds the same chip within a range of angles, using a pattern matching context containing a model defined using[`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md)+ [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md).
  *[Image: mpatcircuitsboardrot.png]*
- Finds a rotated wafer model occurrence within a rotated target image, using a pattern matching context containing a model defined using [`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md). In addition, the example uses[`MimRotate`](../../Reference/im/MimRotate.md) to rotate both the source and target image.
  *[Image: mpatwafer.png]*
- Automatically defines a model within a target image, using a pattern matching context containing a model defined using [`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md).
  *[Image: mpatwafer02.png]*
