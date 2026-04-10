---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Displaying_an_image_in_a_userdefined_window
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Displaying an image in a userdefined window"
---

# Displaying an image in a user-defined window

Images selected to a windowed display using [`MdispSelect`](../../Reference/disp/MdispSelect.md) are presented in a default window created by Aurora Imaging Library. This function dynamically creates a window on the Windows desktop for the specified display, if the display is not already selected. The created window respects any window control type setting associated with the display using an [`Mdisp...`](../../Reference/disp/MdispAlloc.md) function.

Alternatively, for windowed displays, you can choose to display image buffers in a user-defined window, using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md). Note that the window does not require the same dimensions as the image buffer. If the defined window is of a different dimension than the image buffer, any excess window area will be left untouched or any excess image area will be cropped.

## Using MdispSelectWindow()

The [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) function is similar to [`MdispSelect`](../../Reference/disp/MdispSelect.md), except that it allows you to specify the handle of the user-defined window or child window to use for display, rather than displaying into an Aurora Imaging Library created window. This user-defined window is automatically refreshed when the display is modified (for example, when the image data is modified). You can use [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) with [`M_NULL`](../../Reference/disp/MdispSelectWindow.md) to remove the image from the display.

The user-defined window must have a window handle of type _HWND_ or X11. You can use, for example, the Windows API functions to create a window with an _HWND_ handle, and third-party functions (such as GTK, Tkinter, or Qt) to create an X11 window handle. In addition, if the handle parameter of [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) is set to zero, this function behaves like [`MdispSelect`](../../Reference/disp/MdispSelect.md).

> **Note:** Note that, to use a display with a window created using the third-party GTK, Tkinter, or Qt frameworks, you should enable a special mode. You can enable the mode using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GTK_MODE`](../../Reference/disp/MdispControl.md), [`M_TK_MODE`](../../Reference/disp/MdispControl.md), or[`M_QT_MODE`](../../Reference/disp/MdispControl.md), respectively. These modes prevent the display from appearing incorrectly when the window is moved or resized, while introducing a small amount of latency.

To select an image to a user-defined window, the display cannot be allocated on a remote computer ([`MdispAlloc`](../../Reference/disp/MdispAlloc.md) with [`M_REMOTE_DISPLAY`](../../Reference/disp/MdispAlloc.md)).

> **Note:** Note that, by default, mouse and keyboard use are disabled in a user-defined window. You can enable mouse and keyboard use in a user-defined window using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_MOUSE_USE`](../../Reference/disp/MdispControl.md) and [`M_KEYBOARD_USE`](../../Reference/disp/MdispControl.md), respectively.

The following example shows how to display an image in a user-defined window, grab into such a window, and remove the image from the display.

> **Code example:** [MDispWindowQT.cpp](MDispWindowQT.cpp)
