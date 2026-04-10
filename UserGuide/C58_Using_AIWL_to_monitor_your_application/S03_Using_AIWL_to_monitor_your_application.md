---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: Using_AIWL_to_monitor_your_application
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / Using AIWL to monitor your application"
---

# Fundamentals for creating your Aurora Imaging Web Library client application

This section discusses how to create an Aurora Imaging Web Library application, using either the JavaScript or C/C++ API. For additional information specific to each language (including their respective function references), see [Creating an Aurora Imaging Web Library client application with JavaScript](S04_Creating_an_AIWL_client_with_JavaScript.md) and [Creating an Aurora Imaging Web Library client application with C/C++](S06_Creating_an_AIWL_client_with_C.md).

Aurora Imaging Web Library uses a client-server architecture. Your Aurora Imaging Web Library client application connects to your Aurora Imaging Web Library server application and requests data about a published Aurora Imaging Library object. Your server application accepts the request and sends data about the object back to the client. You can also send custom data between the client and server using Aurora Imaging Library messages.

## Conventions for referring to Aurora Imaging Web Library functions and constants

The following conventions for referring to Aurora Imaging Web Library functions and constants are used in this chapter:

- **AIWL.FunctionName**. A function in the Aurora Imaging Web Library API. If a language is not specified, this refers to a function that is available in both JavaScript and C/C++.
  For JavaScript, you use the name`AIWL.FunctionName` to call the function. For C, you use `AIWLFunctionName`. For C++, you use `AIWL::FunctionName` (or use the namespace` AIWL`).
  > **Note:** If a function exists in both the JavaScript and Aurora Imaging Web Library C/C++ API, a link is provided to the JavaScript version of the function.
- **AIWL.M_CONSTANT_NAME**. A constant in the Aurora Imaging Web Library API. If a language is not specified, this refers to a constant that is available in both JavaScript and C/C++.
  For JavaScript, the name of the constant is `AIWL.M_CONSTANT_NAME`. For C/C++, you do not include the `AIWL` prefix (just use`M_CONSTANT_NAME`).

## Aurora Imaging Web Library allocations and functions

There are no mandatory allocations for an Aurora Imaging Web Library client application. One client application can connect to multiple Aurora Imaging Web Library servers; you do not allocate a separate application context for each one. However, you must connect to the server using [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md). This function returns the client-side Aurora Imaging Library identifier of the server.

You use the client-side Aurora Imaging Library identifier of a connected Aurora Imaging Web Library server application, or that of an Aurora Imaging Library object published by the application, to specify with which server application a function will interact. This is similar to most Aurora Imaging Library functions, for which you must specify the Aurora Imaging Library identifier of either a system or an object that was allocated on that system. Just as specifying the identifier of an Aurora Imaging Library object (such as a digitizer) allocated on a particular system implicitly specifies to use that system, specifying the client-side identifier of an Aurora Imaging Library object (such as a display) allocated in a server application implicitly specifies to interact with that server application.

Most Aurora Imaging Web Library functions interact with the connected Aurora Imaging Web Library server in some way. These functions will return an error if the Aurora Imaging Web Library client is not connected to the server. You should not use any Aurora Imaging Web Library functions except [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) and [AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md) until your client is connected to a server.

## Publishing Aurora Imaging Library objects

Before your Aurora Imaging Web Library client application can access an Aurora Imaging Library object, you must publish the object in your Aurora Imaging Web Library server application. To do so, use [`MobjControl`](../../Reference/obj/MobjControl.md)with [`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md)and[`M_READ_ONLY`](../../Reference/obj/MobjControl.md) or [`M_READ_WRITE`](../../Reference/obj/MobjControl.md). You must also assign a name to the object using [`MobjControl`](../../Reference/obj/MobjControl.md) with[`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md).

In your Aurora Imaging Web Library client application, to learn the client-side Aurora Imaging Library identifier of the published object, use [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md) with `AIWL.M_WEB_PUBLISHED_NAME`and the name you assigned to the object. You use this client-side identifier to access the object, with appropriate functions.

> **Note:** Note that for JavaScript, you cannot use the client-side Aurora Imaging Library identifier immediately after inquiring it; you must hook a function to be executed when the connection to the server or object has been established (which happens asynchronously). For more information, see [Accessing Aurora Imaging Web Library server applications and published Aurora Imaging Library objects](S04_Creating_an_AIWL_client_with_JavaScript.md).

The only Aurora Imaging Library objects that can be published are Aurora Imaging Web Library displays and message mailboxes.

> **Note:** You use client-side Aurora Imaging Library identifiers in your Aurora Imaging Web Library client applications, to identify both your Aurora Imaging Web Library server and the Aurora Imaging Library objects it has published. However, the literal value of the client-side identifier, assigned to a particular server or Aurora Imaging Library object, is not the same for each instance of your client. Therefore, you must separately inquire the client-side identifier of an Aurora Imaging Library object in each instance of your client (using [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md) with `AIWL.M_WEB_PUBLISHED_NAME`).

## Connecting to an Aurora Imaging Web Library server application

The following steps provide a typical methodology to establish a connection between your Aurora Imaging Web Library client and Aurora Imaging Web Library server:

1. In your Aurora Imaging Library application, enable Aurora Imaging Web Library using [`MappControl`](../../Reference/app/MappControl.md) with [`M_WEB_CONNECTION`](../../Reference/app/MappControl.md). Your Aurora Imaging Library application is now an Aurora Imaging Web Library server application.
   > **Note:** Aurora Imaging Web Library servers are intended to be run on a segmented LAN that has no internet/WAN access; the servers offer no internal security mechanisms. When Aurora Imaging Web Library is enabled, you should typically ensure that your network administrator configures your network to prevent incoming connections from the internet/WAN to the computer running your Aurora Imaging Library application (and from any other devices on your network which do not need access). If this is not possible, at a minimum you should block incoming connections from the internet to the listening port of the Aurora Imaging Web Library server. You can inquire which port is used for this purpose, using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_WEB_CONNECTION_PORT`](../../Reference/app/MappInquire.md).
2. Optionally, specify the listening port on which the Aurora Imaging Web Library server application receives incoming connections, using [`MappControl`](../../Reference/app/MappControl.md) with [`M_WEB_CONNECTION_PORT`](../../Reference/app/MappControl.md). This must be a port not used by any other application running on the computer. The default port is 7861.
3. In your AIWL client application, open a connection to your Aurora Imaging Library application using [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) with the URL of the computer running your Aurora Imaging Web Library server application.
   > **Note:** [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) writes (to the object or variable specified by the `UserVarPtr`or `RemoteContextAppIdPtr ` parameter) the client-side Aurora Imaging Library application identifier that you use with some other Aurora Imaging Web Library functions, such as [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md). For JavaScript, the connection is completed asynchronously after [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) has returned; therefore, you cannot use the client-side identifier until the connection is established. A`AIWL.M_CONNECT` event is generated when the connection is established.
   The URL is`ws://`, followed by the Hostname or local static IP address of the computer running the Aurora Imaging Web Library server, and a colon (for example, `ws://192.168.1.58:`or`ws://VisionController42:`).
   If you changed the Aurora Imaging Web Library listening port, or there is more than one Aurora Imaging Web Library server application running on the same computer, you must append to the URL a colon (:) followed by the listening port. For example, `ws://192.168.1.58:7861`.
   > **Note:** You might find it useful to create an Aurora Imaging Web Library client application that allows the user to enter the IP address or Hostname dynamically before connecting (for example, using a dialog box).
   When running an instance of your Aurora Imaging Web Library client application on the same computer as your Aurora Imaging Web Library server application, you can always use the name `localhost` in place of the IP address or Hostname.
4. For JavaScript only, in your Aurora Imaging Web Library client application, hook a function to new connection events using [AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md) with `AIWL.M_CONNECT` and the client-side application identifier that was written by [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md). When the hooked function is called, the connection has been established and you can use the client-side application identifier with other functions in your application. You might find it useful to set a global_Boolean_ variable within the hooked function as a flag to indicate that the connection has been established.
   Since the connection to the Aurora Imaging Web Library server application is made asynchronously (after the call to [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md)has returned and execution of your code has continued), the hook-handler function will be called when the connection is established (as long as[AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md)immediately follows [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md)).
   > **Note:** This does not apply to C/C++;[AIWL::MappOpenConnection](S07_C_AIWL_function_reference.md) does not return until after the connection has been established. The client-side application identifier can therefore be used immediately.

## Aurora Imaging Web Library displays

You can send the contents of an Aurora Imaging Library 2D or 3D display to one or more Aurora Imaging Web Library clients, if the display was allocated with [`M_WEB`](../../Reference/disp/MdispAlloc.md) and was published (as described in [Publishing Aurora Imaging Library objects](S03_Using_AIWL_to_monitor_your_application.md)). All connected Aurora Imaging Web Library clients showing the display receive the same image information; if the Aurora Imaging Web Library server or a connected instance of the Aurora Imaging Web Library client modifies the display (for example, using [AIWL.MdispZoom](S05_JavaScript_AIWL_function_reference.md) or interactive keyboard controls), the display is modified for all instances of the client showing that display. To give each instance of your client an independent display, you must allocate multiple displays.

> **Note:** You cannot show an [`M_WEB`](../../Reference/disp/MdispAlloc.md) display in your Aurora Imaging Web Library server application.

The following steps provide a typical methodology to present a display from your Aurora Imaging Web Library server application in a connected Aurora Imaging Web Library client application:

1. In your server application, allocate an Aurora Imaging Web Library display using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) or [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md)with [`M_WEB`](../../Reference/disp/MdispAlloc.md).
2. In your server application, assign a unique name to the display using[`MobjControl`](../../Reference/obj/MobjControl.md)with [`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md).
3. In your server application, publish the display using[`MobjControl`](../../Reference/obj/MobjControl.md)with [`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md) and either [`M_READ_ONLY`](../../Reference/obj/MobjControl.md) or [`M_READ_WRITE`](../../Reference/obj/MobjControl.md) (for displays, there is no difference between these settings).
4. In your server application, select a buffer/container to display. To do so:
   - For a 2D display, select an image buffer to the display using [`MdispSelect`](../../Reference/disp/MdispSelect.md).
   - For a 3D display, select a 3D-displayable point cloud container or depth map image buffer/container to the display using[`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md).
5. In your client application, retrieve the Aurora Imaging Library identifier of the display using [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md) with `AIWL.M_WEB_PUBLISHED_NAME` and the name you assigned to the display.
6. Present the display in your client application. To do so:
   - For JavaScript, create an HTML5 canvas element in your HTML file and present the display in that canvas using [AIWL.MdispSelectWindow](S05_JavaScript_AIWL_function_reference.md) (this function supports both 2D and 3D displays). For example, you can create an HTML5 canvas element by including the following in your HTML file:
     ```
      <canvas id="PlaceToShowAnAILDisplay" width="1600" height="900"></canvas> 
     ```
     Use [AIWL.MdispSelectWindow](S05_JavaScript_AIWL_function_reference.md)with the canvas`id` that you set, and the client-side Aurora Imaging Library identifier of the display. The display will be automatically presented in the HTML5 canvas.
   - For C/C++, the output of the 2D or 3D display is automatically transmitted to the client as a bitmap image (including annotations). To display it, you must map a bitmap data structure appropriate for the third-party GUI toolkit that you are using (for example, Windows GDI (win32), Qt, or GTK) onto the image data. For more information, see[Presenting a display](S06_Creating_an_AIWL_client_with_C.md).
7. Optionally, control the display in your server application (using functions in the Mdisp or M3ddisp module) or in your client application (using functions in the AIWL.Mdisp module). Aurora Imaging Web Library displays support most of the same functionality as windowed displays.
   > **Note:** The Aurora Imaging Web Library API only provides a subset of the functionality of the full Mdisp and M3ddisp modules. To perform tasks not supported by the API, you must use the standard Aurora Imaging Library display functions in your server application. For example, to associate a 2D display with a 2D graphics list, use[`MdispControl`](../../Reference/disp/MdispControl.md). To associate a 2D display with a LUT, use [`MdispLut`](../../Reference/disp/MdispLut.md). To change the view of a 3D display, use [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md).

## Size of an Aurora Imaging Web Library display

A 2D Aurora Imaging Web Library display is always the same size as the currently selected image buffer. You can set the size of a 3D Aurora Imaging Web Library display using[`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with[`M_SIZE_X`](../../Reference/3ddisp/M3ddispControl.md) and [`M_SIZE_Y`](../../Reference/3ddisp/M3ddispControl.md). In both cases, the visible content of an Aurora Imaging Web Library display is transmitted to the Aurora Imaging Web Library client as bitmap image data after panning, zooming, annotations, and other settings/transformations are applied. Any data outside of the visible area of the Aurora Imaging Web Library display is not transmitted to the client.

For example, if you present a 2D display in an HTML5 canvas using [AIWL.MdispSelectWindow](S05_JavaScript_AIWL_function_reference.md) (only available for JavaScript), and the original HTML5 canvas is larger than the selected image buffer, the display is presented in an area the size of the image buffer. If you scale the displayed image using [AIWL.MdispZoom](S05_JavaScript_AIWL_function_reference.md) (equivalent to using [`MdispZoom`](../../Reference/disp/MdispZoom.md) directly in the Aurora Imaging Web Library server application), the size of the presented display does not change; it remains the same size as the image buffer, but shows only the zoomed region of the image.

*[Image: toucanzoom.png]*

> **Note:** For JavaScript, the canvas size is always updated to match the size of the image buffer for a 2D display. Similarly, for a 3D display, the display is always shown at the specified output resolution regardless of the original canvas size (this can be set by using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with[`M_SIZE_X`](../../Reference/3ddisp/M3ddispControl.md) and [`M_SIZE_Y`](../../Reference/3ddisp/M3ddispControl.md)).

## Interacting with a display

You can allow a user to interactively control a display through your Aurora Imaging Web Library client application, as though they were interacting with a windowed display shown on the computer running your Aurora Imaging Web Library server application. To do so, in your client application, use[AIWL.MdispControl](S05_JavaScript_AIWL_function_reference.md) with `AIWL.M_INTERACTIVE` and `AIWL.M_ENABLE`. For a 2D display annotated by a 2D graphics list that has been made editable (using [`MgraControl`](../../Reference/gra/MgraControl.md)with [`M_EDITABLE`](../../Reference/gra/MgraControl.md)), the user is also able to edit the annotations interactively. For more information, see [Creating and modifying graphics interactively](../C26_Generating_graphics/S07_Creating_and_modifying_graphics_interactively.md).

You can also simulate user input using [AIWL.MdispMessage](S05_JavaScript_AIWL_function_reference.md). Aurora Imaging Library does not have a similar function.

> **Note:** For C/C++, user interaction is not sent to the display automatically. Instead, you must forward user input from your Aurora Imaging Web Library client application to the display using [AIWL::MdispMessage](S07_C_AIWL_function_reference.md). For more information, see[Making a presented display interactive](S06_Creating_an_AIWL_client_with_C.md).

> **Note:** Note that although a display can be shown simultaneously in many instances of your Aurora Imaging Web Library client, only one instance can have interactive control at a time. To give each instance of your client an independent display, you must allocate multiple displays (using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) or [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md)with [`M_WEB`](../../Reference/disp/MdispAlloc.md)).

### Hooking to display events

You can hook a function to mouse or key press events in both your Aurora Imaging Web Library client and Aurora Imaging Web Library server applications (using [AIWL.MdispHookFunction](S05_JavaScript_AIWL_function_reference.md) and [`MdispHookFunction`](../../Reference/disp/MdispHookFunction.md) respectively).

For your client, the function is hooked locally; the function is only called when the specified event is generated in the same instance of your client, and [AIWL.MdispGetHookInfo](S05_JavaScript_AIWL_function_reference.md) returns results (such as the mouse position) relative to this instance.

For your server, the function is hooked to whichever instance of the client has interactive control of the display; the function is only called when the specified event is generated on the computer with interactive control, and [`MdispGetHookInfo`](../../Reference/disp/MdispGetHookInfo.md)returns results (such as the mouse position) about that instance.

For example, there might be 2 instances of your client (named client A and client B) running on 2 different computers, both connected to the same server and showing the same display. In this case, both clients have hooked a function to the `AIWL.M_MOUSE_LEFT_BUTTON_DOWN ` event and the server has hooked a function to the [`M_MOUSE_LEFT_BUTTON_DOWN`](../../Reference/disp/MdispHookFunction.md) event. Client A has interactive control of the display.

When the user of client A clicks on the display with the left mouse button, both client A and the Aurora Imaging Web Library server application call the functions that they have hooked to the event. [AIWL.MdispGetHookInfo](S05_JavaScript_AIWL_function_reference.md) and [`MdispGetHookInfo`](../../Reference/disp/MdispGetHookInfo.md) return results about client A.

When the user of client B clicks on the display with the left mouse button, only client B calls the function that it has hooked to the event. [AIWL.MdispGetHookInfo](S05_JavaScript_AIWL_function_reference.md)returns results about client B. The server application does not call its hook function when the user of client B clicks on the display because interactive control is disabled for client B.

## Aurora Imaging Web Library messages

You can exchange any type of data between the Aurora Imaging Web Library client and Aurora Imaging Web Library server using message-passing through Aurora Imaging Library message mailboxes.

Message mailboxes are allocated in your server application and accessed remotely by your client application. Your client can only access a message mailbox while connected to the server in which the message mailbox was allocated.

### Sending messages from your Aurora Imaging Web Library server application

The following is a basic methodology to send a message from your Aurora Imaging Web Library server application to your Aurora Imaging Web Library client application:

1. In your server application, allocate a message mailbox using[`MobjAlloc`](../../Reference/obj/MobjAlloc.md) with[`M_MESSAGE_MAILBOX`](../../Reference/obj/MobjAlloc.md)and[`M_OVERWRITE`](../../Reference/obj/MobjAlloc.md).
   > **Note:** It is recommended to allocate message mailboxes for sending messages to clients with[`M_OVERWRITE`](../../Reference/obj/MobjAlloc.md). However, this is not a requirement. For more information, see [Message mailbox operation modes](../C63_Distributed_AIL/S06_Message_mailboxes.md).
2. In your server application, set the message mailbox to be published for access by clients using[`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md)and[`M_READ_ONLY`](../../Reference/obj/MobjControl.md).
   > **Note:** It is recommended to set message mailboxes for sending messages to clients to be[`M_READ_ONLY`](../../Reference/obj/MobjControl.md). However, this is not a requirement.
3. In your server application, assign a name to the message mailbox using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md).
4. In your client application, retrieve the Aurora Imaging Library identifier of the message mailbox using [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md) and the name that you assigned to the message mailbox.
5. In your client application, hook a function to new messages being added to the message mailbox using [AIWL.MobjHookFunction](S05_JavaScript_AIWL_function_reference.md) with `AIWL.M_UPDATE_WEB`.
6. In your server application, write a message and add it to the message mailbox using [`MobjMessageWrite`](../../Reference/obj/MobjMessageWrite.md).
7. In your client application, in the hook-handler function, read the contents of the message using [AIWL.MobjMessageRead](S05_JavaScript_AIWL_function_reference.md).
   > **Note:** Messages are always transmitted as an array of type _AIL_UINT8_. For JavaScript, if the message contains text, you can convert the data to a string using the utility function `AIWL.convertUTF8ToString`, which takes the message data as a parameter and returns it as a string. In rare cases where you send a message using UTF-16 encoding, use `AIWL.convertUTF16ToString`instead.

### Sending messages from your Aurora Imaging Web Library client application

The following is a basic methodology to send a message from your Aurora Imaging Web Library client application to your Aurora Imaging Web Library server application:

1. In your server application, allocate a message mailbox using[`MobjAlloc`](../../Reference/obj/MobjAlloc.md) with[`M_MESSAGE_MAILBOX`](../../Reference/obj/MobjAlloc.md)and [`M_QUEUE`](../../Reference/obj/MobjAlloc.md).
   > **Note:** It is recommended to allocate message mailboxes for receiving messages from clients with[`M_QUEUE`](../../Reference/obj/MobjAlloc.md). However, this is not a requirement. For more information, see [Message mailbox operation modes](../C63_Distributed_AIL/S06_Message_mailboxes.md).
2. In your server application, set the message mailbox to be published for access by clients using[`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md)and[`M_READ_WRITE`](../../Reference/obj/MobjControl.md).
3. In your server application, assign a name to the message mailbox using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md).
4. In your server application, hook a function to messages being added to the message mailbox by the client(s), using [`MobjHookFunction`](../../Reference/obj/MobjHookFunction.md) with [`M_MESSAGE_RECEIVED`](../../Reference/obj/MobjHookFunction.md).
5. In your client application, retrieve the Aurora Imaging Library identifier of the message mailbox using [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md)and the name that you assigned to the message mailbox.
6. In your client application, write a message and add it to the message mailbox using [AIWL.MobjMessageWrite](S05_JavaScript_AIWL_function_reference.md).
7. In your server application, in the hook-handler function, read the contents of the message using [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md).
