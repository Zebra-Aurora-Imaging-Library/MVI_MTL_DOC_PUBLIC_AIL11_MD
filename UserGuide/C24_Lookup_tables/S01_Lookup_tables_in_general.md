---
doctype: UserGuide
part: "2D related information"
chapter: Lookup_tables
section: Lookup_tables_in_general
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / lut / Lookup tables in general"
---

# Lookup tables in general

Lookup tables (LUTs) are collections of memory locations that are used to map data to precalculated values. They can easily reduce a multi-step or complex operation to a single-step LUT mapping.

*[Image: lut.png]*

You can map an image buffer through a LUT, using [`MimLutMap`](../../Reference/im/MimLutMap.md). If the hardware system permits, you can also use LUTs to precondition input data at acquisition time, before it is stored in an image buffer. LUTs can also be used (hardware system permitting) to adjust the color contrast and intensity of an image upon display, without affecting the actual data. To associate a LUT buffer with a display, use [`MdispLut`](../../Reference/disp/MdispLut.md), or, for 3D displays, [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md).
