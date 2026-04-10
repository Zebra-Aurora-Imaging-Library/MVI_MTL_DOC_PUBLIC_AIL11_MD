---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Overview
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Overview"
---

# Overview

Aurora Imaging Library can display images. It will use the most appropriate graphics controller for display purposes. Aurora Imaging Library will typically display the image on the computer running the main Aurora Imaging Library application (local computer); however, Aurora Imaging Library can also display an image on a remote computer on your network.

To display an image buffer, you must allocate the buffer with a displayable attribute ([`M_DISP`](../../Reference/buf/MbufAlloc2d.md)). In addition, you must allocate a 2D display, using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md), on the same system as the buffer. Once both are allocated, use [`MdispSelect`](../../Reference/disp/MdispSelect.md) to select the image buffer to display.

A buffer can be displayed on the local computer or a remote computer, in a window on the desktop or without a window on a dedicated screen in exclusive mode.

Besides other display effects, you can pan and zoom the displayed image, as well as overlay annotations on the displayed image non-destructively using the overlay mechanism or a 2D graphics list. If using Aurora Imaging Library's overlay mechanism, you can overlay any image on the displayed image and select the transparency color; when you pan and zoom the displayed image, the overlay data is also panned and zoomed.

> **Note:** An image buffer, or any of its child buffers, can be selected on more than one display.

Aurora Imaging Library documentation uses the term **display memory** to refer to physical display (graphics controller) memory.
