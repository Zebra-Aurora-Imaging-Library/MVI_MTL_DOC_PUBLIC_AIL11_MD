---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: Creating_an_AIWL_client_with_C
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / Creating an AIWL client with C"
---

# Creating an Aurora Imaging Web Library client application with C/C++

The Aurora Imaging Web Library C/C++ API is suitable for writing a standalone Aurora Imaging Web Library client application executable. Unlike an Aurora Imaging Library application, an Aurora Imaging Web Library client application can run on a Windows or Linux computer without Aurora Imaging Library installed; it only requires the client application and Aurora Imaging Web Library (DLL or DSO) file to be installed.

Typically, you should write your Aurora Imaging Web Library client application using JavaScript, unless C/C++ is specifically required (for example, if you are integrating Aurora Imaging Web Library client functionality into an existing C/C++ application). A client application written in JavaScript can be run in a web browser on a wide variety of platforms, without the need to install any files on the user's computer.

> **Note:** This section only discusses information specific to the Aurora Imaging Web Library C/C++ API. For general information about creating an Aurora Imaging Web Library client application, see [Fundamentals for creating your Aurora Imaging Web Library client application](S03_Using_AIWL_to_monitor_your_application.md). For details about the C/C++ versions of specific Aurora Imaging Web Library functions, see[C/C++ Aurora Imaging Web Library function reference](S07_C_AIWL_function_reference.md).

## Writing an Aurora Imaging Web Library client application in C/C++

The C/C++ web client application uses _AIWLclient.dll_ (under Windows) or _AIWLclient.so_ (under Linux). These libraries provide functions to access objects, hook on updates, and send input data to the server. To access these libraries, you must include `AIWL.h` in your project.

When deploying a C/C++ Aurora Imaging Web Library client application,_AIWLclient.dll_ or _AIWLclient.so_ must be added as a dependency and installed on the user's computer. Note that no Aurora Imaging Library dependency is required.

C/C++ Aurora Imaging Web Library client applications are only supported on Windows and Linux platforms.

> **Note:** The Windows version of Aurora Imaging Library only includes _AIWLclient.dll_. The Linux version of Aurora Imaging Library only includes _AIWLclient.so_.

## Presenting and interacting with Aurora Imaging Library displays

The output of an Aurora Imaging Library 2D or 3D display is transmitted from your Aurora Imaging Web Library server application to your Aurora Imaging Web Library client application as a bitmap image, after all transformations (such as zooming) and annotations are applied. You must manually present the transmitted image in the client window of your application using your chosen GUI toolkit. To enable interactive control of the display, you must also manually capture user input and send it to the display using [AIWL::MdispMessage](S07_C_AIWL_function_reference.md).

> **Note:** This only applies to C/C++. For JavaScript, presenting and interacting with a display are handled automatically.

For a 3D display, only the final rendered image of the display is transmitted to your AIWL client application. 3D displays are therefore functionally equivalent to 2D displays from the perspective of your Aurora Imaging Web Library client. For this reason, with Aurora Imaging Web Library you use functions from the [AIWL::Mdisp](S07_C_AIWL_function_reference.md) module to work with both 2D and 3D displays (you still use functions from the [`M3ddisp...`](../../Reference/3ddisp/M3ddispControl.md) module to work with 3D displays in your Aurora Imaging Web Library server application).

### Presenting a display

The exact steps to present an Aurora Imaging Web Library display in a client window depend upon the GUI toolkit that you are using in your Aurora Imaging Web Library client application. Specific C++ Aurora Imaging Library examples are provided for Windows GDI (win32), Qt, and GTK.

Regardless of the GUI toolkit that you use to present the display in a client window, your Aurora Imaging Web Library client application will need to inquire the Host address, width, height, and pixel pitch of the transmitted image using [AIWL::MdispInquire](S07_C_AIWL_function_reference.md). Typically, you should also hook a function to display updates using[AIWL::MobjHookFunction](S07_C_AIWL_function_reference.md)with `M_UPDATE_WEB`. You can then update the presented image within the hook-handler function whenever the content of the display is changed in the Aurora Imaging Web Library server application (including if that change was initiated by an Aurora Imaging Web Library client application (for example, using[AIWL::MdispZoom](S07_C_AIWL_function_reference.md))).

Some GUI toolkits (such as Windows GDI and Qt) do not support the default RGBA32 color format of the transmitted image. You can specify that the client-side computer should automatically convert the transmitted image to the BGRA32 color format using[AIWL::MdispControl](S07_C_AIWL_function_reference.md)with`M_WEB_PUBLISHED_FORMAT`and `M_BGR_32`.

> **Note:** Note that, once enabled, you cannot disable this automatic conversion using [AIWL::MdispControl](S07_C_AIWL_function_reference.md). To disable conversion, you must disconnect from the Aurora Imaging Web Library server application and reconnect. When you do this, the display is also assigned a new client-side Aurora Imaging Library identifier.

### Making a presented display interactive

To allow users of your Aurora Imaging Web Library client application to interactively control a display, you must manually pass their mouse and keyboard interactions to the display using [AIWL::MdispMessage](S07_C_AIWL_function_reference.md) (in addition to making the display interactive using [AIWL::MdispControl](S07_C_AIWL_function_reference.md)with `M_INTERACTIVE`). For example, if the user clicks the mouse button within the display, you can use [AIWL::MdispMessage](S07_C_AIWL_function_reference.md)with `M_MOUSE_LEFT_BUTTON_DOWN`to forward the interaction to the display.

In general, if you are using the Microsoft Win32 API, you can map Win32 mouse and keyboard input messages directly to settings for [AIWL::MdispMessage](S07_C_AIWL_function_reference.md). For example, the following code snippet demonstrates a standard window processing callback function. Whenever the user moves the cursor within the window's client area, the callback function receives the `WM_MOUSEMOVE` notification and sends the updated cursor position to the display.

> **Code example:** [userguide.creating_a_ailweb_client_with_c.forwarding_user_input](userguide.creating_a_ailweb_client_with_c.forwarding_user_input)

For each input event that you want to forward to the display, you can include a separate case. If you are forwarding many types of events, you might find it useful to store a mapping between the Win32 and Aurora Imaging Library constants, instead of writing a separate call to [AIWL::MdispMessage](S07_C_AIWL_function_reference.md) for each case.

> **Note:** The[AIWL::MdispMessage](S07_C_AIWL_function_reference.md)**CombinationKeys** parameter does not support the same set of keys that are provided as combination keys with some Win32 input messages.
