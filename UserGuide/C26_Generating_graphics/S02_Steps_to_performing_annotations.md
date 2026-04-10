---
doctype: UserGuide
part: "2D related information"
chapter: Generating_graphics
section: Steps_to_performing_annotations
module_tag: gra
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / graphics / Steps to performing annotations"
---

# Steps to performing annotations

The following steps provide a basic methodology for using the Aurora Imaging Library Graphics module to perform annotations:

1. Allocate a 2D graphics context, using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) to store your drawing preference (for example, foreground and background color).
2. Allocate an image buffer, using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md), or, allocate a 2D graphics list, using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), on which to perform the drawing operation.
3. If necessary, associate the 2D graphics list with a display, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md), to non-destructively annotate the image selected to the display.
4. If necessary, modify 2D graphics context or 2D graphics list settings, using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md).
5. Draw graphics destructively in an image buffer or add graphics to the 2D graphics list, using:
   - One of the functions provided in the Aurora Imaging Library Graphics module, such as [`MgraArc`](../../Reference/gra/MgraArc.md) or [`MgraRect`](../../Reference/gra/MgraRect.md).
   - A draw function of a processing or analysis module (for example, [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) or [`MmetDraw`](../../Reference/met/MmetDraw.md)).
   - [`MgraInteractive`](../../Reference/gra/MgraInteractive.md), to interactively create and add graphics to the 2D graphics list. A display must have the 2D graphics list associated with it ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)) and the display must allow modification to graphics associated with it ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md)).
6. If necessary, annotate an image destructively with the graphics contained in the 2D graphics list, using [`MgraDraw`](../../Reference/gra/MgraDraw.md).
7. Free all your allocated objects using [`MgraFree`](../../Reference/gra/MgraFree.md), unless [`M_UNIQUE_ID`](../../Reference/gra/MgraAlloc.md) was specified during allocation.
