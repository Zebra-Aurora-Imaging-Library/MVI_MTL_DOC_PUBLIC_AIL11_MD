---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Working_with_Linux
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Working with Linux"
---

# Working with Linux

Aurora Imaging Library is available under the Linux operating system. In general, using the library under Linux is similar to using the library under Windows, unless otherwise specified.

## General considerations under Linux

Due to the particularities of the Linux operating system, you should note the following information, with regards to Aurora Imaging Library.

- Control over window attributes, such as window resizing and overlapping, is more extensive under Windows.
- Storage formats are fully available with minimal differences (the device-independent bitmap (DIB) storage format is an exception).
- Tools and utilities are fully available with minimal differences (Aurora Imaging Intellicam and the graphical user interfaces (GUIs) for the processing and analysis modules are exceptions).
- Aurora Imaging Library modules are fully supported. Also note the following:
  - You cannot train (deep learning with the Classification or Deep Learning OCR module) under Linux.

Note, the [Computer Requirements](../C02_Building_an_application/S01_Requirements.md) discussed earlier in the Help also apply to Linux.

## Annotating the display (overlay) with Cairo

Cairo is required when annotating the display (overlay) with Aurora Imaging Library under Linux. To draw native Linux annotations in the buffer selected to the display, or the display's overlay buffer, allocate a Cairo surface for either the image buffer or the overlay buffer of the display using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_SURFACE_ALLOC`](../../Reference/buf/MbufControl.md); then, inquire its identifier using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_SURFACE_HANDLE`](../../Reference/buf/MbufInquire.md). Once you have finished painting your annotations on the Cairo surface, free the surface using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_SURFACE_FREE`](../../Reference/buf/MbufControl.md).

### Cairo library package requirements

When annotating the display (overlay) in a Linux application, the libcairo2-dev Cairo development package must be installed for Ubuntu LTS (64-bit).

### Annotating the display (overlay) with Linux examples

The following examples demonstrate how to add annotations to an Aurora Imaging Library user selected window using the library's overlay mechanism. To run these examples, use Aurora Imaging Example Launcher.

- The example _MdispQtview.cpp_ demonstrates how to annotate the display in a Qt project. It integrates Qt menus and functionality with Aurora Imaging Library displays, buffers, timers, and a digitizer.
  > **Note:** When painting on this widget (using **PaintEvent** with Qt), use the Aurora Imaging Library Overlay.
- The example _MdispGtkview.cpp_ demonstrates how to annotate the display in a GTK project.
  > **Note:** When painting on this widget (using the**Expose event** with GTK), use the Aurora Imaging Library Overlay.
