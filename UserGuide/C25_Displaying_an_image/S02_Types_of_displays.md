---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Types_of_displays
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Types of displays"
---

# Types of displays

You can allocate a display so that an image buffer selected to this display is:

- Displayed in a window on a Windows desktop screen. This is called a **windowed display** ([`M_WINDOWED`](../../Reference/disp/MdispAlloc.md)).
- Displayed on a dedicated screen that is also a Windows desktop screen. This is called an **exclusive display** ([`M_EXCLUSIVE`](../../Reference/disp/MdispAlloc.md)).
- Displayed by one or more Aurora Imaging Web Library clients over a network. This is called an**Aurora Imaging Web Library display** ([`M_WEB`](../../Reference/disp/MdispAlloc.md)).

You must specify the required type of display upon allocating the display, with [`MdispAlloc`](../../Reference/disp/MdispAlloc.md).

## Windowed display

An image buffer selected to a windowed display is presented within a window on the Windows desktop screen(s). To choose a windowed display, set the [`InitFlag`](../../Reference/disp/MdispAlloc.md) parameter of [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) to [`M_WINDOWED`](../../Reference/disp/MdispAlloc.md).

Windowed displays can be presented on a desktop that is displayed using one screen or multiple screens. We refer to these screens, either one or many, as the **Windows desktop screen(s)**. Your desktop can be extended over screens at different resolutions.

All windowed displays are displayed in their own Aurora Imaging Library default window (or, as discussed later, in a user-allocated window). This window is transparently tracked and updated with the image buffer selected to the display; that is, if the window moves or is occluded, the window is automatically updated with the image buffer accordingly. You can show the update rate in the display's title bar. To do so, press `Ctrl+F` on the keyboard. The update rate will appear, in display updates per second (for example, Updates/sec: 29.74). To hide the update rate, press `Ctrl+Shift+F`.

Multiple windowed displays can be allocated and selected for display.

The display's device number should always be set to [`M_DEFAULT`](../../Reference/disp/MdispAlloc.md). Based on the specified format, Aurora Imaging Library will find the best device to use when displaying an image.

For windowed displays, the display format parameter of [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) is ignored. When you select an image buffer to a windowed display, Windows will create a display that has the same size as the image, unless such a display would not fit on the Windows desktop or would be too small. If the image is too large to fit in the largest possible window at the resolution of the selected display, the top-left corner of the image will be aligned with the top-left corner of the largest possible window, and the right and bottom portion of the image, the part that exceeds the window, will not be displayed; to view the missing portions of the image, you can pan the displayed image (see [Panning and zooming](S07_Panning_and_zooming.md) for more information). If the image is smaller in size than the smallest possible window, the image will be centered in the smallest possible window, and the surrounding area will be filled with the background color ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)). If the window is enlarged or maximized, the image will remain the same size and centered.

For windowed displays, Aurora Imaging Library does not typically communicate directly with the graphics controller, but uses the normal Windows mechanisms to display images. Upon selecting a windowed display, Aurora Imaging Library uses internal image buffers to store the selected image buffer and overlay information; it then uses Windows GDI functions for the final blit to screen.

When allocating a windowed display for a Distributed Aurora Imaging Library application, you can set the [`SystemId`](../../Reference/disp/MdispAlloc.md) parameter to a remote system. Image buffers selected to such a display are presented on the local computer by default. However, if required, you can allocate the display so that image buffers are presented on the remote computer. For more information on allocating such a display, see [Distributed Aurora Imaging Library](../C63_Distributed_AIL/ChapterInformation.md).

## Exclusive display

An image selected to an exclusive display is presented full-screen, without a windowed border or frame, on one of the Windows desktop screens. The image buffer is presented at the center of the screen. This screen is referred to as an **exclusive screen**.

*[Image: DualHead_exclusive.png]*

To use an exclusive display, your Windows desktop should be using more than one screen. Allocating an exclusive display on the main screen might not be convenient; if you do so, the Aurora Imaging Library exclusive display will hide third-party applications designed to start on the main screen and hide the Windows taskbar.

When allocating an exclusive display using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md), you can set the display number parameter of [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) to [`M_DEFAULT`](../../Reference/disp/MdispAlloc.md), or to the position of the display device in a 3x3 multi-screen arrangement. For exclusive displays, [`M_DEFAULT`](../../Reference/disp/MdispAlloc.md) will select the best available screen, avoiding the main desktop screen if possible. In screen arrangements of up to 3x3 screens, you can select the display device by specifying its position in the arrangement; you can specify that the screen is at the bottom, top, left, right, and/or center of the multi-screen arrangement (for example, [`M_BOTTOM`](../../Reference/disp/MdispAlloc.md) + [`M_RIGHT`](../../Reference/disp/MdispAlloc.md)).

Additionally, when allocating an exclusive display using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md), you must specify the required video output format for the screen. You can set it to the current Windows resolution for the screen ([`M_CURRENT_RESOLUTION`](../../Reference/disp/MdispAlloc.md)) or you can select a video configuration format (VCF) with the required resolution. If the image is too large to fit on screen given the selected display resolution, the top-left corner of the image will be aligned with the top-left corner of the screen, and the right and bottom portion of the image, the part that exceeds the screen area, will not be displayed; to view the missing portions of the image, you can pan the displayed image (see [Panning and zooming](S07_Panning_and_zooming.md) for more information). If the image is smaller in size than the screen area, the image will be centered on screen, and the surrounding area will be filled with the background color ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)).

By default, the mouse cursor cannot move over an exclusive display; however, you can remove this restriction using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_RESTRICT_CURSOR`](../../Reference/disp/MdispControl.md). In addition, by default, the display reacts to standard key strokes, but you can disable this behavior using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_KEYBOARD_USE`](../../Reference/disp/MdispControl.md).

For exclusive displays, Aurora Imaging Library does not typically communicate directly with the graphics controller, but uses the normal Windows mechanisms to display images. Upon selecting an exclusive display, Aurora Imaging Library uses internal image buffers to store the selected image buffer and overlay information; it then uses Windows GDI functions for the final blit to screen.

When allocating an exclusive display for a Distributed Aurora Imaging Library application, you can set the [`SystemId`](../../Reference/disp/MdispAlloc.md) parameter to a remote system. Image buffers selected to such a display are presented on the local computer by default. However, if required, you can allocate the display so that image buffers are presented on the remote computer. For more information on allocating such a display, see [Distributed Aurora Imaging Library](../C63_Distributed_AIL/ChapterInformation.md).

If only using one screen, you can hook a function to a keypress or mouse movement, using [`MdispHookFunction`](../../Reference/disp/MdispHookFunction.md) with either [`M_KEY...`](../../Reference/disp/MdispHookFunction.md) or [`M_MOUSE...`](../../Reference/disp/MdispHookFunction.md). Once the display event occurs, the exclusive display could be freed to allow the user to perform other work on the single screen.

## Aurora Imaging Web Library display

An image selected to an Aurora Imaging Web Library display is presented by one or more instances of connected Aurora Imaging Web Library client applications (running on the local computer or a remote computer). To use an Aurora Imaging Web Library display, your Aurora Imaging Library application should be configured to be an Aurora Imaging Web Library server application (for more information, see [Using AIWL to monitor your application](../C58_Using_AIWL_to_monitor_your_application/ChapterInformation.md)). The server application will not actually show the display; it will transmit the selected buffer to connected instances of the client applications (when they establish a connection with the display).

*[Image: WebOverview.png]*

You can create an Aurora Imaging Web Library client application using the provided JavaScript or C/C++ Aurora Imaging Web Library API. You can write a client application for a variety of different platforms, including compatible web browsers. Regardless of the chosen API, the remote computer is not required to have Aurora Imaging Library installed, have an Aurora Imaging Library license, nor contain Zebra hardware to run a client application.

Aurora Imaging Web Library displays support most of the same functionality as windowed displays. It is possible to interact with a display through an Aurora Imaging Web Library client application. By interacting with the display, the user can pan, zoom, and transform the image. If the display is annotated by a 2D graphics list that has been made editable, the user is also able to edit those annotations interactively. Only one client can have interactive control of the display at a time. When a change is made, the updated content of the display is sent to all connected clients.

For more information on using Aurora Imaging Web Library, including how to create a client application, see [Using AIWL to monitor your application](../C58_Using_AIWL_to_monitor_your_application/ChapterInformation.md).
