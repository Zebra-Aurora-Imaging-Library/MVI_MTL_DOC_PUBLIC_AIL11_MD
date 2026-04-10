---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: C_AIWL_function_reference
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / C AIWL function reference"
---

# C/C++ Aurora Imaging Web Library function reference

The following functions are available for creating an Aurora Imaging Web Library client using C/C++.

To use these functions, you must include _AIWL.h_ and link to _AIWLclient.dll_ (under Windows) or _AIWLclient.so_ (under Linux).

> **Note:** The C++ function names (including the `AIWL`namespace) are used. For C, replace the namespace with the prefix Aurora Imaging Web Library. For example, instead of `AIWL::MappCloseConnection`, use`AIWLMappCloseConnection`. Unlike JavaScript, Aurora Imaging Library constants have no prefix in C nor namespace in C++.

## AIWL::Mapp

The functions prefixed with AIWL::Mapp make up the Aurora Imaging Web Library Application module. The Application module allows you to connect to an Aurora Imaging Web Library server, inquire the Aurora Imaging Library identifiers of objects published by an server, and hook functions to connection and publishing events.

### AIWL::MappCloseConnection

This function closes a connection to an Aurora Imaging Web Library server application opened by [AIWL::MappOpenConnection](S07_C_AIWL_function_reference.md).

This function is similar, but not identical, to the Aurora Imaging Library function[`MappCloseConnection`](../../Reference/app/MappCloseConnection.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MappCloseConnection**(_AIL_ID_**ContextAppId**) |
| --- | --- |
| **ContextAppId** | Specifies the client-side Aurora Imaging Library application context identifier of the Aurora Imaging Web Library server application from which to disconnect. The application context identifier must have been previously obtained, typically when connecting to the server application using[AIWL::MappOpenConnection](S07_C_AIWL_function_reference.md). |

### AIWL::MappControl

This function controls the settings of your Aurora Imaging Web Library client application environment.

This function is similar, but not identical, to the Aurora Imaging Library function[`MappControl`](../../Reference/app/MappControl.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MappControl**(_AIL_ID_**ContextAppId**,_AIL_INT64_**ControlType**,_AIL_INT_**ControlValue**) |
| --- | --- |
| **ContextAppId** | This parameter is reserved for future expansion and must be set to `M_DEFAULT`. |
| **ControlType** | Specifies the type of application setting about which to inquire. This parameter can be set to one of the following values: |
| `M_ERROR` | Sets whether the printing of error messages to screen is enabled. In this case, set**ControlValue** to one of the following values: |
| `M_PRINT_DISABLE` | Specifies not to print error messages. If error printing is disabled, you can still check for errors. To do so, hook a function to application errors using [AIWL::MappHookFunction](S07_C_AIWL_function_reference.md)with `M_ERROR`. You can retrieve the error code within the hook-handler function, using [AIWL::MappGetHookInfo](S07_C_AIWL_function_reference.md) with `M_ERROR`. |
| `M_PRINT_ENABLE` | Specifies to print error messages. This is the default value. |
| **ControlValue** | Specifies the setting's new value. |

### AIWL::MappGetHookInfo

This function retrieves information about the event that caused the hook-handler function to be called. This function should only be called within the scope of an application hook-handler function call (see [AIWL::MappHookFunction](S07_C_AIWL_function_reference.md)).

The returned value is the requested information.

This function is similar, but not identical, to the Aurora Imaging Library function[`MappGetHookInfo`](../../Reference/app/MappGetHookInfo.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT_**AIWL::MappGetHookInfo**(_AIL_ID_**ContextAppId**,_AIL_ID_**EventId**,_AIL_INT64_**InfoType**,_void *_**UserVarPtr **) |
| --- | --- |
| **ContextAppId** | This parameter is reserved for future expansion and must be set to`M_DEFAULT`. |
| **EventId** | Specifies the application event identifier received by the hook-handler function |
| **InfoType** | Specifies the type of information to get. This parameter can be set to one of the following values: |
| `M_CURRENT` | Retrieves the error code returned by the last Aurora Imaging Web Library function call (if the hook-handler function was called due to an `M_ERROR` event type). The current-error code is reset to `M_NULL_ERROR` before each function call and is set to a specific error code if an error occurs while trying to execute the function. > **Note:** This only retrieves error codes for your Aurora Imaging Web Library client application; it does not retrieve error codes for the Aurora Imaging Web Library server. In this case,**UserVarPtr**returns the error code; the recommended casting type for the data is _AIL_INT_. > **Note:** You can combine this value with `M_MESSAGE`. |
| `M_CURRENT_SUB_1` | Retrieves the first error subcode returned by the last Aurora Imaging Web Library function call (if the hook-handler function was called due to an `M_ERROR` event type). Note that when there is no error, the error subcode is set to `M_NULL_ERROR`. > **Note:** This only retrieves error subcodes for your Aurora Imaging Web Library client application; it does not retrieve error codes for the Aurora Imaging Web Library server. In this case,**UserVarPtr**returns the error subcode; the recommended casting type for the data is _AIL_INT_. > **Note:** You can combine this value with `M_MESSAGE`. |
| `M_CURRENT_SUB_NB` | Retrieves the number of error subcodes associated with the last Aurora Imaging Web Library function called, when it returns an error (if the hook-handler function was called due to an `M_ERROR` event type). In this case,**UserVarPtr**returns the number of error subcodes; the recommended casting type for the data is _AIL_INT_. |
| `M_OBJECT_ID` | Retrieves the client-side Aurora Imaging Library identifier of the Aurora Imaging Web Library server application or Aurora Imaging Library object that generated the event. > **Note:** Note that this Aurora Imaging Library identifier should only be used within the instance of your Aurora Imaging Web Library client application that called this function. The server application, and other instances of your client application, use a different Aurora Imaging Library identifier to refer to the same object. In this case,**UserVarPtr**returns the client-side Aurora Imaging Library identifier of the server application; the recommended casting type for the data is _AIL_ID_. |
| `M_WEB_CLIENT_INDEX` | Retrieves the index of this instance of the Aurora Imaging Web Library client application, from the perspective of the Aurora Imaging Web Library server application that generated the event. > **Note:** This index is guaranteed not to change until this instance of the client application is disconnected. Note that this is not an iterable index; connected instances of your client are not guaranteed to be assigned sequential indices, and the indices do not shift when a client disconnects. If this instance of the Aurora Imaging Web Library client is connected to multiple Aurora Imaging Web Library servers, each Aurora Imaging Web Library server assigns a different index to this instance of the client. In this case,**UserVarPtr** returns the index of this instance of the Aurora Imaging Web Library client; the recommended casting type for the data is _AIL_INT_. |
| You can combine `M_CURRENT` and `M_CURRENT_SUB_1`with the following: |
| `M_MESSAGE` | Returns the error message instead of the error code. In this case, the recommended casting type for the data written to **UserVarPtr** is an array of_AIL_TEXT_CHAR _[optionally, in C++: _AIL_STRING_]. > **Note:** You can combine this value with `M_STRING_SIZE`. |
| You can combine `M_MESSAGE` with the following: |
| `M_STRING_SIZE` | Retrieves the length of the string, including the terminating null character ("\0"). In this case, the recommended casting type for the data written to `UserVarPtr` is _AIL_INT_. |
| **UserVarPtr ** | Specifies the address in which to write the requested information. Since this function also returns the requested information, you can set this parameter to `M_NULL`. |

### AIWL::MappHookFunction

This function allows you to attach or detach a user-defined function to a specified application event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

This function is similar, but not identical, to the Aurora Imaging Library function[`MappHookFunction`](../../Reference/app/MappHookFunction.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MappHookFunction**(_AIL_ID_**ContextAppId**,_AIL_INT_**HookType**,_AIL_HOOK_FUNCTION_PTR_**HookHandlerPtr**,_void *_**UserDataPtr **) |
| --- | --- |
| **ContextAppId** | Specifies the client-side Aurora Imaging Library application context identifier of the Aurora Imaging Web Library server application to which to hook the function. The application context identifier must have been previously obtained, typically when connecting to the server application using[AIWL::MappOpenConnection](S07_C_AIWL_function_reference.md). |
| **HookType** | Specifies the event type. This parameter can be set to one of the following values: To detach (unhook) a function that you previously hooked, combine the event type with` M_UNHOOK` (for example, ` M_CONNECT`+` M_UNHOOK`). |
| `M_CONNECT` | Specifies to call the hook-handler function when your Aurora Imaging Web Library client application establishes a connection with the specified Aurora Imaging Web Library server application (or one of its published Aurora Imaging Library objects). You can use[AIWL::MappGetHookInfo](S07_C_AIWL_function_reference.md)with` M_OBJECT_ID`to learn which application or object connected, and [AIWL::MobjInquire](S07_C_AIWL_function_reference.md)with ` M_OBJECT_TYPE`to learn the type of the application or object. |
| `M_DISCONNECT` | Specifies to call the hook-handler function when your Aurora Imaging Web Library client application disconnects from the specified Aurora Imaging Web Library server application (or one of its published Aurora Imaging Library objects). You can use[AIWL::MappGetHookInfo](S07_C_AIWL_function_reference.md)with` M_OBJECT_ID`to learn which application or object disconnected, and [AIWL::MobjInquire](S07_C_AIWL_function_reference.md)with ` M_OBJECT_TYPE`to learn the type of the application or object. |
| `M_ERROR` | Specifies to call the hook-handler function when an error occurs while attempting to connected to the Aurora Imaging Web Library server application or published Aurora Imaging Library object. |
| `M_OBJECT_PUBLISH_WEB ` | Specifies to call the hook-handler function each time the specified Aurora Imaging Web Library server application publishes or unpublishes an object. |
| **HookHandlerPtr** | Specifies the address of the function that should be called when an event occurs. This function must have the following prototype:`AIL_INT MFTYPE HookHandler(AIL_INT HookType, AIL_ID EventId, void * UserDataPtr)`. |
| **UserDataPtr ** | Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its **UserDataPtr** parameter, when the specified event occurs. Set this parameter to `M_NULL` if user data is not required. |

### AIWL::MappInquire

This function inquires about the specified application setting of either the Aurora Imaging Web Library client or Aurora Imaging Web Library server application.

The returned value is the requested information.

This function is similar, but not identical, to the Aurora Imaging Library function[`MappInquire`](../../Reference/app/MappInquire.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT_**AIWL::MappInquire**(_AIL_ID_**ContextAppId**,_AIL_INT64_**InquireType**,_void *_**UserDataPtr **) |
| --- | --- |
| **ContextAppId** | Specifies the client-side Aurora Imaging Library application context identifier of the Aurora Imaging Web Library server application for which to inquire the settings. The application context identifier must have been previously obtained, typically when connecting to the Aurora Imaging Web Library server application using[AIWL::MappOpenConnection](S07_C_AIWL_function_reference.md). To inquire about a setting of the client application, set this parameter to `M_DEFAULT`. > **Note:** See the description of a setting for the**InquireType**parameter to learn whether the setting inquires the client application or server application. |
| **InquireType** | Specifies the type of application setting about which to inquire. This parameter can be set to one of the following values: |
| `M_ERROR` | Inquires the error printing mode of the Aurora Imaging Web Library client application. In this case, the recommended casting type for the data written to **UserDataPtr** is _AIL_INT_, and it returns one of the following values: |
| `M_PRINT_DISABLE` | Specifies not to print error messages. |
| `M_PRINT_ENABLE` | Specifies to print error messages. |
| `M_WEB_CLIENT_INDEX` | Retrieves the index of this instance of the Aurora Imaging Web Library client application, from the perspective of the specified Aurora Imaging Web Library server application. This index does not change for this instance of the client as long as it is connected. If this instance of the client disconnects and then reconnects, it is assigned a new index by the server. If this instance of the client is connected to multiple servers, each server assigns a different index to this instance of the client. > **Note:** Indices of clients are not zero or one-based, nor are they sequential. In this case,**UserDataPtr**returns the index of this instance of the client; the recommended casting type for the data is _AIL_INT_. |
| **UserDataPtr ** | Specifies the address in which to write the requested information. Since this function also returns the requested information, you can set this parameter to `M_NULL`. |

### AIWL::MappInquireConnection

This function inquires about the published Aurora Imaging Library objects in the Aurora Imaging Web Library server application.

The returned value is the requested information.

This function is similar, but not identical, to the Aurora Imaging Library function[`MappInquireConnection`](../../Reference/app/MappInquireConnection.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT_**AIWL::MappInquireConnection**(_AIL_ID_**ContextAppId**,_AIL_INT64_**InquireType**,_AIL_INT64_**ControlFlag**,_AIL_INT64_**ExtraFlag**,_void *_**UserVarPtr**) |
| --- | --- |
| **ContextAppId** | Specifies the client-side Aurora Imaging Library application context identifier of the Aurora Imaging Web Library server application, For which to inquire the settings of published objects. The application context identifier must have been previously obtained, typically when connecting to the server application using[AIWL::MappOpenConnection](S07_C_AIWL_function_reference.md). |
| **InquireType** | Specifies the type of application setting about which to inquire. This parameter can be set to one of the following values: |
| `M_WEB_PUBLISHED_LIST` | Inquires the list of client-side Aurora Imaging Library identifiers associated with the Aurora Imaging Library objects published by the Aurora Imaging Web Library server application. Published Aurora Imaging Library objects are those Aurora Imaging Library objects set to read or read/write. In this case,**UserVarPtr**returns an array of client-side Aurora Imaging Library identifiers of the published objects; the recommended casting type for the data is an array of type_AIL_ID_. > **Note:** Use this function with`M_WEB_PUBLISHED_LIST_SIZE`to learn the number of published objects. |
| `M_WEB_PUBLISHED_LIST_SIZE` | Inquires the current number of Aurora Imaging Library objects published by the Aurora Imaging Web Library server application. In this case,**UserVarPtr**returns the number of published objects; the recommended casting type for the data is _AIL_INT_. |
| `M_WEB_PUBLISHED_NAME` | Inquires the client-side Aurora Imaging Library identifier of a published Aurora Imaging Library object associated with the name specified by the **ControlFlag** parameter. This name must have been previously associated with an object in the Aurora Imaging Web Library server application using [`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md). In this case, set **ControlFlag**to `M_PTR_TO_AIL_INT(AIL_TEXT("ObjectName"))`. If you are passing the object name in a variable, don't enclose it in `AIL_TEXT()`. In this case,**UserVarPtr**returns the client-side Aurora Imaging Library identifier of the published object; the recommended casting type for the data is _AIL_ID_. > **Note:** Note that this client-side Aurora Imaging Library identifier should only be used within the instance of your Aurora Imaging Web Library client application that called this function. The server application, and other instances of your client application, use a different Aurora Imaging Library identifier to refer to the same object. |
| **ControlFlag** | Specifies an attribute of the inquire operation to perform. If this parameter is not required for the inquire operation, it must be set to `M_DEFAULT`. |
| **ExtraFlag** | This parameter is reserved for future expansion and must be set to`M_DEFAULT`. |
| **UserVarPtr ** | Specifies the address in which to write the requested information. Since this function also returns the requested information, you can set this parameter to `M_NULL`. |

### AIWL::MappOpenConnection

This function opens a connection to an Aurora Imaging Web Library server application, through the listening port of the server application. The server application can be running on a local or remote computer.

You can later close the connection using [AIWL::MappCloseConnection](S07_C_AIWL_function_reference.md).

This function is similar, but not identical, to the Aurora Imaging Library function[`MappOpenConnection`](../../Reference/app/MappOpenConnection.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MappOpenConnection**(_AIL_CONST_TEXT_PTR _**ConnectionDescriptor**,_AIL_INT64_**InitFlag**,_AIL_INT64_**ControlFlag**,_AIL_ID *_**RemoteContextAppIdPtr **) |
| --- | --- |
| **ConnectionDescriptor** | Specifies the computer to which to connect, and if necessary the listening port of the Aurora Imaging Web Library server application. You must specify the prefix "ws://", followed by the domain name or IP address of the computer, optionally followed by the listening port number. This should be enclosed in the `AIL_TEXT` macro. For example, `AIL_TEXT("ws://localhost:7861")` or `AIL_TEXT("ws://192.168.1.51:7861")`. > **Note:** You can always use the Hostname `localhost` to connect to a server application running on the same computer as the instance of the client application. In your Aurora Imaging Library application, you can set the listening port using [`MappControl`](../../Reference/app/MappControl.md)with [`M_WEB_CONNECTION_PORT`](../../Reference/app/MappControl.md). The default port is 7861. > **Note:** You might find it useful to create an Aurora Imaging Web Library client application that allows the user to enter the IP address or Hostname dynamically before connecting (for example, using a dialog box). |
| **InitFlag** | This parameter is reserved for future expansion and must be set to`M_DEFAULT`. |
| **ControlFlag** | This parameter is reserved for future expansion and must be set to`M_DEFAULT`. |
| **RemoteContextAppIdPtr ** | Specifies the address in which to write the client-side identifier of the Aurora Imaging Web Library server application. If the connection fails, the value `M_NULL`is written. > **Note:** Note that this client-side Aurora Imaging Library identifier should only be used within the instance of your Aurora Imaging Web Library client application that called this function. The server application, and other instances of your client application, use a different Aurora Imaging Library identifier to refer to the same object. |

## AIWL::Mdisp

The functions prefixed with AIWL::Mdisp make up the Aurora Imaging Web Library Display module. The Display module allows you to access the image output of a published 2D or 3D display, enable interactive manipulation of a published display, and hook functions to display events (such as user interaction).

> **Note:** Unlike Aurora Imaging Library, Aurora Imaging Web Library uses the same module for both 2D and 3D displays. For a 3D display, only the final rendered image of the display is transmitted to your Aurora Imaging Web Library client application; 3D displays are therefore functionally equivalent to 2D displays from the perspective of a client. All functions and settings in this module are supported for both 2D and 3D displays, except where otherwise noted.

### AIWL::MdispControl

This function controls the specified Aurora Imaging Library display setting.

This function is similar, but not identical, to the Aurora Imaging Library function[`MdispControl`](../../Reference/disp/MdispControl.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MdispControl**(_AIL_ID_**DisplayId**,_AIL_INT64_**ControlType**,_AIL_DOUBLE_**ControlValue**) |
| --- | --- |
| **DisplayId** | Specifies the client-side Aurora Imaging Library display identifier of the display. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **ControlType** | Specifies the type of display setting to control. This parameter can be set to one of the following values: |
| `M_INTERACTIVE` | Sets whether the local user can interact with the display. For C/C++, Aurora Imaging Web Library does not automatically transmit user input to the Aurora Imaging Web Library server. You must manually pass user input to the Aurora Imaging Library display using [AIWL::MdispMessage](S07_C_AIWL_function_reference.md). For more information, see[Making a presented display interactive](S06_Creating_an_AIWL_client_with_C.md). > **Note:** To learn the default keyboard controls, see [`M_KEYBOARD_USE`](../../Reference/disp/MdispControl.md). In this case, set**ControlValue** to one of the following values: |
| `M_DISABLE` | Specifies that the user cannot interact with the display. > **Note:** Note that this instance of your Aurora Imaging Web Library client application will still generate mouse and keyboard events for the purposes of hooked functions (if you generate those events using [AIWL::MdispMessage](S07_C_AIWL_function_reference.md)). However, the events are not sent to the Aurora Imaging Web Library server application, or other instances of your client application. This is the default value. |
| `M_ENABLE` | Specifies that the user can interact with the display, using a keyboard and mouse. Only one connected instance of your Aurora Imaging Web Library client application can enable this feature at a time for the specified display. If another instance of the client application has this feature enabled for the specified display, this function has no effect. |
| `M_UPDATE_WEB` | Sets whether the Aurora Imaging Web Library server application should send updated images of this display to this instance of the Aurora Imaging Web Library client application. In this case, set**ControlValue** to one of the following values: |
| `M_DISABLE` | Specifies not to update the display. |
| `M_ENABLE` | Specifies to update the display. Also, the display is forced to update immediately. This is the default value. |
| `M_NOW` | Forces an immediate update of the display. In this case, the display is updated even if `M_UPDATE_WEB` is set to `M_DISABLE`. |
| `M_WEB_PUBLISHED_FORMAT` | Sets whether to automatically convert the transmitted image to a different color format. > **Note:** The image of the display is always transmitted in the RGBA32 color format (the alpha channel always specifies 100 percent opacity for all pixels). If you specify to convert the image to a different color format, the conversion is performed on the local computer. This setting does not affect the display on the Aurora Imaging Web Library server, or other instances of the Aurora Imaging Web Library client. In this case, set**ControlValue** to oe of the following values: |
| `M_BGR32` | Specifies to convert the image to the BGRA32 color format (the alpha channel always specifies 100 percent opacity for all pixels). |
| `M_RGB32` | Specifies that the image is not converted; it remains in the transmitted RGBA32 format (the alpha channel always specifies 100 percent opacity for all pixels). This is the default value. |
| **ControlValue** | Specifies the setting's new value. |

### AIWL::MdispGetHookInfo

This function retrieves information about the event that caused the hook-handler function to be called. This function should only be called within the scope of a display hook-handler function call (see [AIWL::MdispHookFunction](S07_C_AIWL_function_reference.md)).

The returned value is the requested information.

This function is similar, but not identical, to the Aurora Imaging Library function[`MdispGetHookInfo`](../../Reference/disp/MdispGetHookInfo.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT_**AIWL::MdispGetHookInfo**(_AIL_ID_**EventId**_AIL_INT64_**InfoType**,_void *_**UserVarPtr **) |
| --- | --- |
| **EventId** | Specifies the display event identifier received by the hook-handler function. |
| **InfoType** | Specifies the type of information to get. This parameter can be set to one of the following values: |
| `M_DISPLAY` | Retrieves the client-side Aurora Imaging Library identifier of the display that generated the event. > **Note:** Note that this Aurora Imaging Library identifier should only be used within the instance of your Aurora Imaging Web Library client application that called this function. The Aurora Imaging Web Library server application, and other instances of your client application, use a different Aurora Imaging Library identifier to refer to the same object. In this case,**UserVarPtr**returns the client-side Aurora Imaging Library identifier of the display; the recommended casting type for the data is _AIL_ID_. |
| **UserVarPtr ** | Specifies the address in which to write the requested information. Since this function also returns the requested information, you can set this parameter to `M_NULL`. |

### AIWL::MdispHookFunction

This function allows you to attach or detach a user-defined function to a specified display event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs. For more information, see [Hooking to display events](S03_Using_AIWL_to_monitor_your_application.md).

This function is similar, but not identical, to the Aurora Imaging Library function[`MdispHookFunction`](../../Reference/disp/MdispHookFunction.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MdispHookFunction**(_AIL_ID_**DisplayId**,_AIL_INT_**HookType**,_AIL_HOOK_FUNCTION_PTR_**HookHandlerPtr**,_void *_**UserDataPtr **) |
| --- | --- |
| **DisplayId** | Specifies the client-side Aurora Imaging Library display identifier of the display to use. This identifier must have been previously inquired, typically using `MappInquireConnection`with `M_WEB_PUBLISHED_NAME`. |
| **HookType** | Specifies the event type. This parameter can be set to one of the following values: To detach (unhook) a function that you previously hooked, combine the event type with` M_UNHOOK` (for example, `M_UPDATE_INTERACTIVE_STATE`+` M_UNHOOK`). |
| `M_UPDATE_INTERACTIVE_STATE` | Specifies to call the hook-handler function each time an Aurora Imaging Web Library client takes or releases interactive control of the display (using [AIWL::MdispControl](S07_C_AIWL_function_reference.md)with` M_INTERACTIVE`). This includes other connected instances of your client application. |
| **HookHandlerPtr** | Specifies the address of the function that should be called when an event occurs. This function must have the following prototype:`AIL_INT MFTYPE HookHandler(AIL_INT HookType, AIL_ID EventId, void * UserDataPtr)`. |
| **UserDataPtr ** | Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its **UserDataPtr** parameter, when the specified event occurs. Set this parameter to `M_NULL` if user data is not required. |

### AIWL::MdispInquire

This function inquires about the specified display setting.

The returned value is the requested information.

This function is similar, but not identical, to the Aurora Imaging Library function[`MdispInquire`](../../Reference/disp/MdispInquire.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT_**AIWL::MdispInquire**(_AIL_ID_**DisplayId**,_AIL_INT64_**InquireType**,_void *_**UserVarPtr **) |
| --- | --- |
| **DisplayId** | Specifies the client-side Aurora Imaging Library display identifier of the display to use. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **InquireType** | Specifies the type of display setting about which to inquire. This parameter can be set to one of the following values: |
| `M_IMAGE_HOST_ADDRESS` | Inquires the Host address (on the computer running your Aurora Imaging Web Library client application) of the image of the display transmitted by the Aurora Imaging Web Library server application. This address can be used to directly access the data of the transmitted image with the Host CPU. > **Note:** For a 3D display, this image is the rendered output of the display. This is functionally equivalent to receiving the output of a 2D display; you can present both 2D and 3D displays using the same methods. The image is always transmitted in the RGBA32 packed format, whereby the different color components of each pixel are stored sequentially as 8-bit values (starting with the R value). You can specify to automatically convert the data to the BGRA32 packed format using [AIWL::MdispControl](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_FORMAT` and `M_BGR32`. In this case, [AIWL::MdispInquire](S07_C_AIWL_function_reference.md)returns the Host address of the converted image data. To learn the size and pitch of the image, use [AIWL::MdispInquire](S07_C_AIWL_function_reference.md)with `M_SIZE_X`,`M_SIZE_Y`, and `M_PITCH_BYTE`. In this case,**UserVarPtr**returns the Host address of the image; the recommended casting type for the data is _AIL_INT8 *_. |
| `M_INTERACTIVE` | Inquires whether the user can interact with the display. In this case, the recommended casting type for the data written to **UserVarPtr** is _AIL_INT_, and it returns one of the following values: |
| `M_DISABLE` | Specifies that the user cannot interact with the display, using a keyboard and mouse. |
| `M_ENABLE` | Specifies that the user can interact with the display, using a keyboard and mouse. |
| `M_PITCH_BYTE` | Inquires the number of bytes between the beginnings of any two adjacent lines of the transmitted image data. In this case,**UserVarPtr**returns the pitch of the transmitted image (in bytes); the recommended casting type for the data is _AIL_INT_. |
| `M_SIZE_BYTE` | Inquires the total number of bytes in the transmitted image data. In this case,**UserVarPtr**returns the size of the image data (in bytes); the recommended casting type for the data is _AIL_INT_. |
| `M_SIZE_X` | Inquires the width of the transmitted image. In this case,**UserVarPtr**returns the width of the transmitted image (in pixels); the recommended casting type for the data is _AIL_INT_. |
| `M_SIZE_Y` | Inquires the height of the transmitted image. In this case,**UserVarPtr**returns the height of the transmitted image (in pixels); the recommended casting type for the data is _AIL_INT_. |
| `M_WEB_PUBLISHED_FORMAT` | Inquires the color format of the image of the display, transmitted by the Aurora Imaging Web Library server application. > **Note:** The image of the display is always transmitted in the RGBA32 format. If another color format is specified, the conversion is performed automatically on the local computer. In this case, the recommended casting type for the data written to **UserDataPtr** is _AIL_INT_, and it returns one of the following values: |
| `M_BGR32` | Specifies that the image is automatically converted to the BGRA32 format (the alpha channel always specifies 100 percent opacity for all pixels). |
| `M_RGB32` | Specifies that the image is not converted; it remains in the transmitted RGBA32 format (the alpha channel always specifies 100 percent opacity for all pixels). |
| **UserVarPtr** | Specifies the address in which to write the requested information. Since this function also returns the requested information, you can set this parameter to `M_NULL`. |

### AIWL::MdispMessage

This function generates a mouse or keyboard event for the specified display. This manipulates the display if interactive control is enabled (using [AIWL::MdispControl](S07_C_AIWL_function_reference.md)with `AIWL.M_INTERACTIVE`), and generates an event for the purposes of hooked functions. This allows you to simulate keyboard and mouse input.

> **Note:** Note that if interactive control is not enabled, the event is only generated for this instance of your Aurora Imaging Web Library client application (not for the Aurora Imaging Web Library server application). The event is never generated for other instances of your client application.

This function is not similar to any Aurora Imaging Library function.

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MdispMessage**(_AIL_ID_**DisplayId**,_AIL_INT_**EventType**,_AIL_INT_**MousePositionX**,_AIL_INT_**MousePositionY**,_AIL_INT_**EventValue**,_AIL_INT_**CombinationKeys**,_AIL_INT_**UserValue**) |
| --- | --- |
| **DisplayId** | Specifies the client-side Aurora Imaging Library display identifier of the display to use. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **EventType** | Specifies the type of event to generate. This parameter can be set to one of the following values: |
| `M_KEY_DOWN` | Specifies to generate an event as though a key was pressed. In this case, set **EventValue**to one of the Aurora Imaging Library constants that can be written to the [`UserVarPtr`](../../Reference/disp/MdispGetHookInfo.md)parameter of[`MdispGetHookInfo`](../../Reference/disp/MdispGetHookInfo.md) when the[`InfoType`](../../Reference/disp/MdispGetHookInfo.md) parameter is set to[`M_AIL_KEY_VALUE`](../../Reference/disp/MdispGetHookInfo.md). |
| `M_KEY_UP` | Specifies to generate an event as though a key was released. In this case, set **EventValue**to one of the Aurora Imaging Library constants that can be written to the [`UserVarPtr`](../../Reference/disp/MdispGetHookInfo.md)parameter of[`MdispGetHookInfo`](../../Reference/disp/MdispGetHookInfo.md) when the[`InfoType`](../../Reference/disp/MdispGetHookInfo.md) parameter is set to[`M_AIL_KEY_VALUE`](../../Reference/disp/MdispGetHookInfo.md). |
| `M_MOUSE_LEFT_BUTTON_DOWN` | Specifies to generate an event as though the left mouse button was pressed. In this case, set **EventValue**to `M_NULL`. |
| `M_MOUSE_LEFT_BUTTON_UP` | Specifies to generate an event as though the left mouse button was released. In this case, set **EventValue**to `M_NULL`. |
| `M_MOUSE_MIDDLE_BUTTON_DOWN` | Specifies to generate an event as though the middle mouse button was pressed. In this case, set **EventValue**to `M_NULL`. |
| `M_MOUSE_MIDDLE_BUTTON_UP` | Specifies to generate an event as though the middle mouse button was released. In this case, set **EventValue**to `M_NULL`. |
| `M_MOUSE_MOVE` | Specifies to generate an event as though the mouse was moved. In this case, set **MousePositionX**and **MousePositionY**to the X and Y positions (in display coordinates) to which to simulate moving the mouse. |
| `M_MOUSE_RIGHT_BUTTON_DOWN` | Specifies to generate an event as though the right mouse button was pressed. In this case, set **EventValue**to `M_NULL`. |
| `M_MOUSE_RIGHT_BUTTON_UP` | Specifies to generate an event as though the right mouse button was released. In this case, set **EventValue**to `M_NULL`. |
| `M_MOUSE_WHEEL` | Specifies to generate an event as though the mouse wheel was moved. In this case, set **EventValue**to the simulated value for the wheel's rotation. A positive value indicates that the wheel was rotated forward, away from the user; a negative value indicates that the wheel was rotate backward, toward the user. |
| **MousePositionX** | Specifies the simulated position of the mouse cursor along the X-axis in display coordinates. If not used, set this value to `M_DEFAULT`. |
| **MousePositionY** | Specifies the simulated position of the mouse cursor along the Y-axis in display coordinates. If not used, set this value to `M_DEFAULT`. |
| **EventValue** | Specifies additional information related to the event specified by the **EventType**parameter. If not used, set this parameter to `M_NULL`. |
| **CombinationKeys** | Specifies which combination keys to simulate holding. This parameter can be set to`M_NULL`(which specifies no combination keys), or any combination of the following values: |
| `M_KEY_ALT` | Specifies the Alt key. |
| `M_KEY_CTRL` | Specifies the CTRL key. |
| `M_KEY_SHIFT` | Specifies the Shift key. |
| `M_KEY_WIN` | Specifies the Windows key. |
| **UserVar** | This parameter is reserved for future expansion and must be set to `M_NULL`. |

### AIWL::MdispPan

This function associates X- and Y-panning offsets with the specified display. The transmitted image is a panned version of the selected image buffer, offset from the top-left corner and cropped to that image buffer's pixel dimensions. For more information, see [Size of an Aurora Imaging Web Library display](S03_Using_AIWL_to_monitor_your_application.md).

> **Note:** This pans the display for all instances of your Aurora Imaging Web Library client application. If you need to pan the image separately for each instance of your client application, you must allocate and publish a separate display for each instance and select the image to each display.

This function is not supported for 3D displays.

This function causes the Aurora Imaging Web Library server application to call the Aurora Imaging Library function[`MdispPan`](../../Reference/disp/MdispPan.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MdispPan**(_AIL_ID_**DisplayId**,_AIL_DOUBLE_**XOffset**,_AIL_DOUBLE_**YOffset**) |
| --- | --- |
| **DisplayId** | Specifies the client-side Aurora Imaging Library display identifier of the 2D display to use. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **XFactor** | Specifies the number of image pixels by which to pan the display. Specify a positive offset value to displace the image to the left. |
| **YFactor** | Specifies the number of image pixels by which to pan the display. Specify a positive offset value to displace the image upwards. |

### AIWL::MdispZoom

This function associates a zoom factor in X and/or in Y with the specified display. The transmitted image is a zoomed version of the selected image buffer, cropped to that image buffer's pixel dimensions. For more information, see [Size of an Aurora Imaging Web Library display](S03_Using_AIWL_to_monitor_your_application.md).

> **Note:** This zooms the display for all instances of your Aurora Imaging Web Library client application. If you need to zoom the image separately for each instance of your client application, you must allocate and publish a separate display for each instance and select the image to each display.

This function is not supported for 3D displays.

This function causes the Aurora Imaging Web Library server application to call the Aurora Imaging Library function[`MdispZoom`](../../Reference/disp/MdispZoom.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MdispZoom**(_AIL_ID_**DisplayId**,_AIL_DOUBLE_**XFactor**,_AIL_DOUBLE_**YFactor**) |
| --- | --- |
| **DisplayId** | Specifies the client-side Aurora Imaging Library display identifier of the 2D display to use. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **XFactor** | Specifies the zoom factor for the X-direction of the display. This value must be larger than 0. A value greater than 1.0 will zoom in, while a value less than 1.0 will zoom out. |
| **YFactor** | Specifies the zoom factor for the Y-direction of the display. This value must be larger than 0. A value greater than 1.0 will zoom in, while a value less than 1.0 will zoom out. |

## AIWL::Mobj

The functions prefixed with AIWL::Mobj make up the Aurora Imaging Web Library Object module. The Object module allows you to read and write messages in Aurora Imaging Library message mailboxes, inquire the types of published objects, and hook functions to the update and connection events of published objects.

### AIWL::MobjGetHookInfo

This function retrieves information about the event that caused the hook-handler function to be called. This function should only be called within the scope of an object hook-handler function call (see [AIWL::MobjHookFunction](S07_C_AIWL_function_reference.md)).

The returned value is the requested information.

This function is similar, but not identical, to the Aurora Imaging Library function[`MobjGetHookInfo`](../../Reference/obj/MobjGetHookInfo.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT_**AIWL::MobjGetHookInfo**(_AIL_ID_**EventId**,_AIL_INT64_**InfoType**,_void *_**UserPtr**) |
| --- | --- |
| **EventId** | Specifies the object event identifier that was received by the hook-handler function. |
| **InfoType** | Specifies the type of information to return about the event. This parameter can be set to the following value: |
| `M_OBJECT_ID` | Retrieves the Aurora Imaging Library identifier of the object that generated the event. > **Note:** Note that this Aurora Imaging Library identifier should only be used within the instance of your Aurora Imaging Web Library client application that called this function. The Aurora Imaging Web Library server application, and other instances of your client application, use a different Aurora Imaging Library identifier to refer to the same object. |
| **UserPtr** | Specifies the address in which to write the requested information. Since this function also returns the requested information, you can set this parameter to `M_NULL`. |

### AIWL::MobjHookFunction

This function allows you to attach or detach a user-defined function to a specified object event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

This function is similar, but not identical, to the Aurora Imaging Library function[`MobjHookFunction`](../../Reference/obj/MobjHookFunction.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MobjHookFunction**(_AIL_ID_**ObjectId**,_AIL_INT_**HookType**,_AIL_HOOK_FUNCTION_PTR_**HookHandlerPtr**,_void *_**UserDataPtr **) |
| --- | --- |
| **ObjectId** | Specifies the client-side Aurora Imaging Library identifier of the object to use. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **HookType** | Specifies the event type. This parameter can be set to one of the following values: To detach (unhook) a function that you previously hooked, combine the event type with` M_UNHOOK` (for example, ` M_UPDATE_WEB`+` M_UNHOOK`). |
| `M_UPDATE_WEB` | Specifies to call the hook-handler function each time the contents of the object changes. |
| **HookHandlerPtr** | Specifies the address of the function that should be called when an event occurs. This function must have the following prototype:`AIL_INT MFTYPE HookHandler(AIL_INT HookType, AIL_ID EventId, void * UserDataPtr)`. |
| **UserDataPtr ** | Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its **UserDataPtr** parameter, when the specified event occurs. Set this parameter to `M_NULL` if user data is not required. |

### AIWL::MobjInquire

This function inquires about the specified object setting.

The returned value is the requested information.

This function is similar, but not identical, to the Aurora Imaging Library function[`MobjInquire`](../../Reference/obj/MobjInquire.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT_AIWL::**MobjInquire**(_AIL_ID_**ObjectId**,_AIL_INT64_**InquireType**,_void *_**UserDataPtr **) |
| --- | --- |
| **ObjectId** | Specifies the client-side Aurora Imaging Library identifier of the object about which to inquire. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **InquireType** | Specifies the type of display setting about which to inquire. This parameter can be set to one of the following values: |
| `M_OBJECT_NAME` | Inquires the Aurora Imaging Library object's user-defined name (set in the Aurora Imaging Web Library server application, using [`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md)). In this case,**UserDataPtr**returns the name,; the recommended casting type for the data is an array of_AIL_TEXT_CHAR _[optionally, in C++: _AIL_STRING_]. |
| `M_OBJECT_TYPE` | Inquires what type of access to the object your Aurora Imaging Web Library client application has (set in the Aurora Imaging Web Library server application, using [`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md)). In this case, the recommended casting type for the data written to **UserDataPtr** is _AIL_INT_, and it returns one of the following values: |
| `M_APPLICATION` | Specifies a server application. |
| `M_DISPLAY` | Specifies an Aurora Imaging Library display (allocated using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) in the server application). |
| `M_MESSAGE_MAILBOX` | Specifies an Aurora Imaging Library message mailbox (allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md)with [`M_MESSAGE_MAILBOX`](../../Reference/obj/MobjAlloc.md) in the server application). |
| `M_WEB_PUBLISH` | Inquires what type of access to the object your Aurora Imaging Web Library client application has (set in the Aurora Imaging Web Library server application, using [`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md)). In this case, the recommended casting type for the data written to **UserDataPtr** is _AIL_INT_, and it returns one of the following values: |
| `M_READ_ONLY` | Specifies that the client application has read-only access to the object. |
| `M_READ_WRITE` | Specifies that the client application has both read and write access to the object. |
| **UserDataPtr ** | Specifies the address in which to write the requested information. Since this function also returns the requested information, you can set this parameter to `M_NULL`. |

### AIWL::MobjMessageRead

This function reads data from a message sent to a message mailbox (previously published by an Aurora Imaging Web Library server application).

> **Note:** Reading a message using this function always removes the message from the mailbox (if the read operation is successful).

The returned value is the size of the message (the same as the value written in **MessageOutSizePtr**).

This function is similar, but not identical, to the Aurora Imaging Library function[`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md).

| *[Image: z_TableSpacer20x20.png]* | _AIL_INT64_**AIWL::MobjMessageRead**(_AIL_ID_**MessageId**,_AIL_INT64 *_**MessagePtr**,_AIL_INT64_**MessageInSize**,_AIL_INT64 *_**MessageTagPtr**,_AIL_INT64 *_**StatusPtr**,_AIL_INT64_**OperationFlag**) |
| --- | --- |
| **MessageId** | Specifies the client-side Aurora Imaging Library identifier of the message mailbox. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **MessagePtr** | Specifies the address of the variable in which to write the message. This must be a pointer to an array of type _AIL_UINT8_ [optionally, in C++: a reference to a _std::vector&lt;AIL_UINT8>_]. > **Note:** If `M_NULL` is specified, this function will instead return the length of the message. |
| **MessageInSize** | Specifies the size of the array pointed to by MessagePtr, in bytes. You must set this value to 0 if **MessagePtr** is set to `M_NULL`. > **Note:** When using a standard vector (_std::vector_) overload function in C++, you can pass `M_DEFAULT` to this parameter and Aurora Imaging Library will automatically determine the size based on the number of items in the vector passed to the parameter. |
| **MessageOutSizePtr** | Specifies the address of the variable in which to write the size of the message to be read. If unused, set this parameter to `M_NULL`. |
| **MessageTagPtr** | Specifies the address of the variable in which to write the message tag associated with the message. If unused, set this parameter to `M_NULL`. |
| **StatusPtr** | Specifies the address of the variable in which the status of the read operation will be written. This parameter returns one of the following values. |
| `M_BUFFER_TOO_SMALL` | Specifies the current message is not copied to **MessagePtr** or deleted. The returned message length will be the required length in bytes. |
| `M_SUCCESS` | Specifies the message is copied to **MessagePtr**. |
| **OperationFlag** | This parameter is reserved for future expansion and must be set to `M_DEFAULT`. |

### AIWL::MobjMessageWrite

This function writes a message that will be added to a message mailbox (previously published by an Aurora Imaging Web Library server application).

This function is similar, but not identical, to the Aurora Imaging Library function[`MobjMessageWrite`](../../Reference/obj/MobjMessageWrite.md).

| *[Image: z_TableSpacer20x20.png]* | _void_**AIWL::MobjMessageWrite**(_AIL_ID_**MessageId**,_const void *_**MessagePtr**,_AIL_INT64_**MessageSize**,_AIL_INT64_**MessageTag**,_AIL_INT64_**OperationFlag**) |
| --- | --- |
| **MessageId** | Specifies the client-side Aurora Imaging Library identifier of the message mailbox. This identifier must have been previously inquired, typically using [AIWL::MappInquireConnection](S07_C_AIWL_function_reference.md)with `M_WEB_PUBLISHED_NAME`. |
| **MessagePtr** | Specifies the address of the variable of the user message to write. This must be a pointer to an array of type _AIL_UINT8_ [optionally, in C++: a reference to a _std::vector&lt;AIL_UINT8>_]. |
| **MessageSize** | Specifies the length of the message to write, in bytes. > **Note:** When using a standard vector (_std::vector_) overload function in C++, you can pass `M_DEFAULT` to this parameter and Aurora Imaging Library will automatically determine the size based on the number of items in the vector passed to the parameter. |
| **MessageTag** | Specifies a user-defined tag to send with the message. |
| **OperationFlag** | This parameter is reserved for future expansion and must be set to `M_DEFAULT`. |
