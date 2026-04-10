---
doctype: Reference
module: obj
function: MobjControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / obj / MobjControl"
---

# MobjControl

> Control an Aurora Imaging Library object's settings and associate information with an Aurora Imaging Library object.

## Syntax

```c
void MobjControl(
    AIL_ID     ObjectId,     //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function controls the settings of an Aurora Imaging Library object. You can control the read/write properties of a remote Aurora Imaging Library object and associate information with an Aurora Imaging Library object. You can also control Aurora Imaging Library message mailboxes and HTTP servers.

## Parameters

### `ObjectId` *(out, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library object to control. The Aurora Imaging Library object can be an Aurora Imaging Library utility object (allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md)) or any other Aurora Imaging Library object, except for Aurora Imaging Library objects allocated using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md).

### `ControlType` *(in, AIL_INT64)*

Specifies the type of Aurora Imaging Library object setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library object setting's new value.

## Parameter Associations

### For controlling the access of remote Aurora Imaging Library objects

The following [`ControlType`](../../Reference/obj/MobjControl.md) and corresponding [`ControlValue`](../../Reference/obj/MobjControl.md) parameter settings are used to control the access of remote Aurora Imaging Library objects.

---

### `M_DAIL_PUBLISH`

Controls an Aurora Imaging Library object's remote access rights. If you set an Aurora Imaging Library object to [`M_READ_ONLY`](../../Reference/obj/MobjControl.md) or [`M_READ_WRITE`](../../Reference/obj/MobjControl.md), a monitoring application will be able to access that Aurora Imaging Library object once a connection has been established [`MappOpenConnection`](../../Reference/app/MappOpenConnection.md) between the monitoring and publishing applications.  > **Note:** Note that the access rights of all Aurora Imaging Library objects in a given publishing application cannot be greater than that application's permission level specified in [`MappControl`](../../Reference/app/MappControl.md) with [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md).

| Value | Description |
| --- | --- |
| `M_NO` *(default)* | Specifies that the Aurora Imaging Library object will not be visible. |
| `M_READ_ONLY` | Specifies that the Aurora Imaging Library object can only be used as a source or to be inquired.  An Aurora Imaging Library object can only be set to [`M_READ_ONLY`](../../Reference/obj/MobjControl.md) if [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md) is set to either [`M_DAIL_CONTROL`](../../Reference/app/MappControl.md) or [`M_DAIL_MONITOR`](../../Reference/app/MappControl.md). |
| `M_READ_WRITE` | Specifies that the Aurora Imaging Library object can be used as a destination or can be controlled by Aurora Imaging Library functions.  An Aurora Imaging Library object can only be set to [`M_READ_WRITE`](../../Reference/obj/MobjControl.md) if [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md) is set to [`M_DAIL_CONTROL`](../../Reference/app/MappControl.md). |

### For controlling the access of Aurora Imaging Web Library objects

The following [`ControlType`](../../Reference/obj/MobjControl.md) and corresponding [`ControlValue`](../../Reference/obj/MobjControl.md) parameter settings are used to control whether the object is accessible using Aurora Imaging Web Library.

---

### `M_WEB_PUBLISH`

Sets whether the object is accessible using Aurora Imaging Web Library.  This setting is only supported for message mailboxes and Aurora Imaging Web Library displays (allocated using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md)with [`M_WEB`](../../Reference/disp/MdispAlloc.md)). It is not supported for objects allocated on a Distributed Aurora Imaging Library remote system.  > **Note:** This setting requires Aurora Imaging Library driver update 47 (or later).

| Value | Description |
| --- | --- |
| `M_NO` *(default)* | Specifies that the object is not accessible using Aurora Imaging Web Library. |
| `M_READ_ONLY` | Specifies that the object can be read from by an Aurora Imaging Web Library client application.  An Aurora Imaging Library object can only be set to [`M_READ_ONLY`](../../Reference/obj/MobjControl.md) if [`M_WEB_CONNECTION`](../../Reference/app/MappControl.md) is set to [`M_ENABLE`](../../Reference/app/MappControl.md). |
| `M_READ_WRITE` | Specifies that the object can be read from and written to by an Aurora Imaging Web Library client application.  An Aurora Imaging Library object can only be set to [`M_READ_WRITE`](../../Reference/obj/MobjControl.md) if [`M_WEB_CONNECTION`](../../Reference/app/MappControl.md) is set to [`M_ENABLE`](../../Reference/app/MappControl.md). |

### For associating an Aurora Imaging Library object with information

The following [`ControlType`](../../Reference/obj/MobjControl.md) and corresponding [`ControlValue`](../../Reference/obj/MobjControl.md) parameter settings are used to associate information to an Aurora Imaging Library object.

---

### `M_OBJECT_NAME`

Sets the name to associate with an Aurora Imaging Library object.

| Value | Description |
| --- | --- |
| `"ObjectName"` | Specifies the name to associate with the Aurora Imaging Library object. |

---

### `M_OBJECT_USER_DATA_PTR`

Sets the address of the user data to associate with an Aurora Imaging Library object. This is useful, for example, to associate specific data to the different buffers passed to [`MdigProcess`](../../Reference/dig/MdigProcess.md). When used with Distributed Aurora Imaging Library, the user data pointer is stored locally in the process doing MobjControl.

| Value | Description |
| --- | --- |
| `ptr` | Specifies the address of the user data. |

### For controlling message mailboxes

The following [`ControlType`](../../Reference/obj/MobjControl.md) and corresponding [`ControlValue`](../../Reference/obj/MobjControl.md) parameter settings are used to control message mailbox objects allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md)with [`M_MESSAGE_MAILBOX`](../../Reference/obj/MobjAlloc.md).

---

### `M_QUEUE_FULL_MODE`

Sets the behavior of the queue when the message limit is attained.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ERROR` | Specifies that an Aurora Imaging Library error is generated when the message limit is attained. |
| `M_WRITE_TIMEOUT` *(default)* | Specifies to wait for the write timeout to elapse. |

---

### `M_QUEUE_SIZE`

Sets the number of messages the queue can hold.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of messages the queue can hold. |

---

### `M_READ_TIMEOUT`

Sets the amount of time to wait for a read operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the read operation waits indefinitely. |
| `Value > 0` | Specifies the amount of time to wait, in msecs. |

---

### `M_RESET`

Resets the message mailbox by removing all messages.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_WRITE_TIMEOUT`

Sets the amount of time to wait for a write operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the write operation waits indefinitely. |
| `Value > 0` | Specifies the amount of time to wait, in msecs. |

### For controlling HTTP servers

The following [`ControlType`](../../Reference/obj/MobjControl.md) and corresponding [`ControlValue`](../../Reference/obj/MobjControl.md) parameter settings are used to control Aurora Imaging Library HTTP server objects allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md)with [`M_HTTP_SERVER`](../../Reference/obj/MobjAlloc.md).

---

### `M_HTTP_ADDRESS`

Sets the web address (URL) with which the Aurora Imaging Library HTTP server can be accessed in a web browser.  > **Note:** This setting requires Aurora Imaging Library driver update 47 (or later).

| Value | Description |
| --- | --- |
| `"http://server:port"` | Specifies the web address (URL). This must consist of`http://`, followed by the domain name or local static IP address of the local computer (for example, `http://192.168.1.58`or`http://VisionController42`).  Optionally, you can specify the port number in the address; this must be the same as the port specified by [`M_HTTP_PORT`](../../Reference/obj/MobjControl.md). Typically, specifying the port is only useful if you have multiple HTTP server processes running on the same computer. Note that this includes third-party HTTP server software.  The default URL is "http://localhost". This web address is only accessible from a web browser running on the local computer. |

---

### `M_HTTP_PORT`

Sets the port on which the Aurora Imaging Library HTTP server listens for incoming connections.  If you specified a port number using[`M_HTTP_ADDRESS`](../../Reference/obj/MobjControl.md), this must be the same port.  > **Note:** For increased security, you should ensure that remote computers on the internet cannot access this port of the local computer.  > **Note:** This setting requires Aurora Imaging Library driver update 47 (or later).

| Value | Description |
| --- | --- |
| `0 <= Value <=65535` *(default)* | Specifies the port number.  > **Note:** This port must not be used by any other application (including third-party applications) running on the local computer. |

---

### `M_HTTP_ROOT_DIRECTORY`

Sets the path of the folder on the local computer in which the files hosted by the Aurora Imaging Library HTTP server are stored (for example, HTML and JavaScript files). All files in this folder (and its subfolders) are made available to connected computers when the HTTP server is enabled (using [`M_HTTP_START`](../../Reference/obj/MobjControl.md)).  You should typically ensure that there are no sensitive files in this folder (or its subfolders).  > **Note:** There is no default directory; you must manually set a directory before enabling the HTTP server.  > **Note:** This setting requires Aurora Imaging Library driver update 47 (or later).

| Value | Description |
| --- | --- |
| `"DirectoryName"` | Specifies the drive and path of the folder (for example, _"C:\mydirectory"_). |

---

### `M_HTTP_START`

Enables the Aurora Imaging Library HTTP server.  > **Note:** Aurora Imaging Library HTTP servers are intended to be run on a segmented LAN that has no internet/WAN access; Aurora Imaging Library HTTP servers offer no internal security mechanisms. When an Aurora Imaging Library HTTP server is enabled, you should typically ensure that your network administrator configures your network to prevent incoming connections from the internet to the computer running your Aurora Imaging Library application (and from any other devices on your network that do not need access). If this is not possible, at a minimum, you should block incoming connections from the internet to the listening port of the HTTP server. You can inquire which port is used for this purpose, using [`MobjInquire`](../../Reference/obj/MobjInquire.md) with [`M_HTTP_PORT`](../../Reference/obj/MobjInquire.md).  > **Note:** This setting requires Aurora Imaging Library driver update 47 (or later).

---

### `M_HTTP_STOP`

Disables the Aurora Imaging Library HTTP server.  > **Note:** This setting requires Aurora Imaging Library driver update 47 (or later).
