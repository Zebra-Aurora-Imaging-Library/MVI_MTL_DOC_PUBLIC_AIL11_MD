---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: Miscellaneous_AIWL_notes_Command_reference
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / Miscellaneous AIWL notes Command reference"
---

# Miscellaneous reference information for Aurora Imaging Web Library

This section includes additional reference information for Aurora Imaging Web Library. The information found in this section might be a reiteration of content previously documented.

The following functions are available for creating an Aurora Imaging Web Library client using JavaScript:

- [AIWL.Mapp](S08_Miscellaneous_AIWL_notes_Command_reference.md).
- [AIWL.Mbuf](S08_Miscellaneous_AIWL_notes_Command_reference.md).
- [AIWL.Mdisp](S08_Miscellaneous_AIWL_notes_Command_reference.md).
- [AIWL.Mobj](S08_Miscellaneous_AIWL_notes_Command_reference.md).

The following functions are available for creating an Aurora Imaging Web Library client using C/C++:

- [AIWL::Mapp](S08_Miscellaneous_AIWL_notes_Command_reference.md).
- [AIWL::Mbuf](S08_Miscellaneous_AIWL_notes_Command_reference.md).
- [AIWL::Mdisp](S08_Miscellaneous_AIWL_notes_Command_reference.md).
- [AIWL::Mobj](S08_Miscellaneous_AIWL_notes_Command_reference.md).

> **Note:** The C++ function names (including the `AIWL`namespace) are used. For C, replace the namespace with the prefix Aurora Imaging Web Library. For example, instead of `AIWL::MappCloseConnection`, use`AIWLMappCloseConnection`. Unlike JavaScript, Aurora Imaging Library constants have no prefix in C nor namespace in C++.

## AIWL.Mapp

This section presents information about using the Aurora Imaging Web Library Application module with JavaScript.

### AIWL.MappControl

The following lists additional support when using AIWL.MappControl(ApplicationId, ControlType, ControlValue).

- The **ApplicationId** parameter specifies the Aurora Imaging Library application context identifier of the publishing application.

### AIWL.MappGetHookInfo

The Aurora Imaging Web Library Application module with JavaScript supports AIWL.MappGetHookInfo(EventId, InfoType, UserVar).

### AIWL.MappInquire

The following lists additional support when using AIWL.MappInquire(ApplicationId, InquireType, UserVar).

- Support for `AIWL.M_OBJECT_TYPE` InquireType. Inquire returns the `AIWL.M_APPLICATION` unique identifier.

### AIWL.MappInquireConnection

The following lists additional support when using AIWL.MappInquireConnection(ApplicationId, InquireType, ControlFlag, ExtraFlag, UserVar).

- Support for `AIWL.M_WEB_PUBLISHED_LIST` InquireType.
  The **ControlFlag** parameter specifies the array of published Aurora Imaging Library objects.

## AIWL.Mbuf

This section presents information about using the Aurora Imaging Web Library Buffer module with JavaScript.

### AIWL.MbufGet

The following lists additional support when using AIWL.MbufGet(ImageId, UserVar). This function retrieves data from a buffer and puts it in a user-supplied array.

- Support for **ImageId** parameter. Specifies the identifier of the target buffer.
- Support for **UserData** parameter. If successful, specifies the content of the ArrayBuffer object in `UserData.data`. Otherwise, it is set to `AIWL.M_NULL`.

### AIWL.MbufGetHookInfo

For more information about the support when using AIWL.MbufGetHookInfo(EventId, InfoType, UserVar), see [AIWL.MobjGetHookInfo](S05_JavaScript_AIWL_function_reference.md).

### AIWL.MbufpGetHookInfo

The following lists additional support when using AIWL.MbufpGetHookInfo(EventId, InfoType, UserVar). This function inquires about an Aurora Imaging Library data buffer setting.

- Support for **EventId** parameter. Specifies the application event identifier received by the hook-handler function.
- Support for **InfoType** parameter. Specifies the type of information to inquire about. The associated InfoType is:
  - `AIWL.M_BUFFER`. Returns the buffer identifier of the object generating the hook.
- Support for **UserVar** parameter. Specifies a JavaScript object where `UserVar.data` always contains a result.

### AIWL.MbufInquire

For more information about the support when using AIWL.MbufInquire(ImageId, InquireType, UserVar), see [AIWL.MobjInquire](S05_JavaScript_AIWL_function_reference.md).

## AIWL.Mdisp

This section presents information about using the Aurora Imaging Web Library Display module with JavaScript.

### AIWL.MdispControl

The following lists additional support when using AIWL.MdispControl(DisplayId, ControlType, ControlValue).

- Support for `AIWL.M_UPDATE_WEB ` ControlType. Possible ControlValues are `M_DISABLE` and`M_NOW`.

### AIWL.MdispGetHookInfo

The following lists additional support when using AIWL.MdispGetHookInfo(EventId, InfoType, UserVar).

- Support for `AIWL.M_KEY_VALUE` InfoType. Specifies to retrieve the ASCII value of the key that triggered the event.

### AIWL.MdispHookFunction

The Aurora Imaging Web Library Display module with JavaScript supports AIWL.MdispHookFunction(DisplayId, HookType, HookHandler).

### AIWL.MdispInquire

The following lists additional support when using AIWL.MdispInquire(DisplayId, InquireType, UserVar).

- Support for `AIWL.M_HOST_ADDRESS` InquireType where the **ArrayBuffer** UserVar parameter specifies the host address.

### AIWL.MdispMessage

The following lists additional support when using AIWL.MdispMessage(DisplayId, EventType, MousePositionX, MousePositionY, EventValue, CombinationKeys, UserVar).

- Support for `AIWL.M_MOUSE_LEFT_BUTTON` CombinationKeys.
- Support for `AIWL.M_MOUSE_MIDDLE_BUTTON` CombinationKeys.
- Support for `AIWL.M_MOUSE_RIGHT_BUTTON` CombinationKeys.

## AIWL.Mobj

This section presents information about using the Aurora Imaging Web Library Object module with JavaScript.

### AIWL.MobjHookFunction

The Aurora Imaging Web Library Object module with JavaScript supports AIWL.MobjHookFunction(ObjectId, HookType, HookHandler).

### AIWL.MobjInquire

The following lists additional support when using AIWL.MobjInquire(ObjectId, InquireType, UserVar).

- Support for `AIWL.M_OBJECT_TYPE` InquireType. Inquires the object type (such as `AIWL.M_APPLICATION`, `AIWL.M_DISPLAY`, and `AIWL.M_IMAGE`).
- Support for `AIWL.M_SIZE_BAND` InquireType. Inquires the band size of the object, if supported.
- Support for `AIWL.M_SIZE_BYTE` InquireType. Inquires the size of the object, if supported.
- Support for `AIWL.M_SIZE_X` InquireType. Inquires the X size of the object, if supported.
- Support for `AIWL.M_SIZE_Y` InquireType. Inquires the Y Size of the object, if supported.
- Support for `AIWL.M_TYPE` InquireType. Inquires the data type of the object, if supported.

Note that not all of the inquire types described above are supported by all objects.

### AIWL.MobjMessageRead

The following lists additional support when using AIWL.MobjMessageRead(MessageId, MessageVar, MessageInSize, MessageOutSize, MessageTag, MessageStatus, OperationFlag).

- Support for **MessageStatus** parameter. Specifies that the `AIWL.M_SUCCESS` tag is stored in `MessageStatus.data`. If not used, this parameter can return null.

### AIWL.MobjMessageWrite

The following lists additional support when using AIWL.MobjMessageWrite(MessageId, MessageData, MessageLength, MessageTag, OperationFlag).

- Support for when **OperationFlag** is set to `AIWL.M_DEFAULT`. Specifies that the message is to be sent.

## AIWL::Mapp

This section presents information about using the Aurora Imaging Web Library Application module with C/C++.

### AIWL::MappCloseConnection

The following lists additional support when using AIWL::MappCloseConnection.

- Unlike the JavaScript API, this function waits until the close is terminated.

### AIWL::MappGetInfo

The following lists additional support when using AIWL::MappGetInfo.

Only the following **InfoTypes** are supported by the C/C++ web client.

- Support for the `M_CURRENT` InfoType.
- Support for the `M_CURRENT_SUB_1` InfoType.
- Support for the `M_CURRENT_SUB_NB` InfoType.
- Support for the `M_MESSAGE+M_CURRENT` InfoType.
- Support for the `M_MESSAGE+M_CURRENT_SUB_1` InfoType.
- Support for the `M_OBJECT_ID` InfoType.
- Support for the `M_WEB_CLIENT_INDEX` InfoType. Retrieves the client identifier from the location pointed to by UserVarPtr as an **AIL_INT**.

### AIWL::MappInquire

The following lists additional support when using AIWL::MappInquire.

- Support for `AIWL.M_WEB_CLIENT_INDEX` InquireType. Returns the unique identifier for the web client.

Only the following **InquireTypes** are supported by the C/C++ web client.

- Support for `AIWL.M_ERROR` InquireType.
- Support for `AIWL.M_OBJECT_TYPE` InquireType.

### AIWL::MappInquireConnection

The following lists additional support when using AIWL::MappInquireConnection.

Only the following **InquireTypes** are supported by the C/C++ web client.

- Support for the `M_WEB_PUBLISHED_LIST` InquireType. Inquires the list of Aurora Imaging Library identifiers associated with the published Aurora Imaging Library objects. The associated **ControlValue** must be an array of Aurora Imaging Library objects.
- Support for the `M_WEB_PUBLISHED_NAME` InquireType. Inquires the object identifier associated with its name. The associated **ControlValue** must be an Aurora Imaging Library object identifier.

### AIWL::MappOpenConnection

The following lists additional support when using AIWL::MappOpenConnection.

- Support for **ConnectionDescriptor** parameter.
  AILTEXT("ws://localhost[:Port]") opens a connection to a valid websocket URL. The connection uses the WebSocket protocol (ws://). Unlike the JavaScript API, this function is synchronous and will return a valid application identifier or `M_NULL` if the connection fails. No `M_CONNECT` hook is generated.

## AIWL::Mbuf

This section presents information about using the Aurora Imaging Web Library Buffer module with C/C++.

### AIWL::MbufGet

The Aurora Imaging Web Library Buffer module with C/C++ supports AIWL::MbufGet.

### AIWL::MbufGetHookInfo

For more information about the support when using AIWL::MbufGetHookInfo, see [AIWL::MobjGetHookInfo](S07_C_AIWL_function_reference.md).

### AIWL::MbufInquire

For more information about the support when using AIWL::MbufInquire, see [AIWL::MobjInquire](S07_C_AIWL_function_reference.md).

## AIWL::Mdisp

This section presents information about using the Aurora Imaging Web Library Display module with C/C++.

### AIWL::MdispControl

The following lists additional support when using AIWL::MdispControl.

Only the following **ControlTypes** are supported by the C/C++ web client.

- Support for `M_INTERACTIVE` ControlType. Sets whether the user can interact with the display. The associated **ControlValues** are:
  - `M_DISABLE`. Specifies not to update the display.
  - `M_ENABLE`. Specifies that the user can interact with the display, using a keyboard and mouse. Note that only one client can enable this feature at a time.
- Support for `M_UPDATE_WEB` ControlType. Sets whether Aurora Imaging Library should update the display. The associated **ControlValues** are:
  - `M_DISABLE`. Specifies not to update the display.
  - `M_NOW`. Specifies to force an immediate update of the display.
- Support for `M_WEB_PUBLISHED_FORMAT` ControlType. Specifies the format of the image data published. The associated **ControlValue** is:
  - `M_BGR32`. Specifies that image data is in BGR32 format.

### AIWL::MdispInquire

The following lists additional support when using AIWL::MdispInquire.

Only the following **InquireTypes** are supported by the C/C++ web client.

- Support for `M_INTERACTIVE` InquireType. Inquires whether the user can interact with the display.
  The associated UserVarPtr can be set to either `M_ENABLE` or `M_DISABLE`.
- Support for `M_HOST_ADDRESS` InquireType. Inquires the Host address of the current image in the display. The associated UserVarPtr must be an array buffer that contains the Host address.

### AIWL::MdispMessage

The following lists additional support when using AIWL::MdispMessage.

- Support for **MousePositionX** parameter.
  When set to `M_DEFAULT`, specifies that keyboard or other message is to be displayed.
- Support for **MousePositionY** parameter.
  When set to `M_DEFAULT`, specifies that keyboard or other message is to be displayed.
- Support for `AIWL.M_MOUSE_LEFT_BUTTON` CombinationKeys.
- Support for `AIWL.M_MOUSE_MIDDLE_BUTTON` CombinationKeys.
- Support for `AIWL.M_MOUSE_RIGHT_BUTTON` CombinationKeys.

## AIWL::Mobj

This section presents information about using the Aurora Imaging Web Library Object module with C/C++.

### AIWL::MobjInquire

The following lists additional support when using AIWL::MobjInquire.

Only the following **InquireTypes** are supported by the C/C++ web client.

- Support for `M_OBJECT_NAME` InquireType.
- Support for `M_OBJECT_TYPE` InquireType.
- Support for `M_SIZE_BAND` InquireType. Inquires the band size of the object, if supported.
- Support for `M_SIZE_BYTE` InquireType. Inquires the size of the object, if supported.
- Support for `M_SIZE_X` InquireType. Inquires the X size of the object, if supported.
- Support for`M_SIZE_Y` InquireType. Inquires the Y Size of the object, if supported.
- Support for `M_TYPE` InquireType. Inquires the data type of the object, if supported.
- Support for `M_WEB_PUBLISH` InquireType. Inquires the access type (`M_READ_WRITE` or `M_READ_ONLY`).
