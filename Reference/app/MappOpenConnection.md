---
doctype: Reference
module: app
function: MappOpenConnection
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / app / MappOpenConnection"
---

# MappOpenConnection

> Connect to a Distributed Aurora Imaging Library publishing application.

## Syntax

```c
void MappOpenConnection(
    AIL_CONST_TEXT_PTR ConnectionDescriptor,  //in
    AIL_INT64          InitFlag,              //in
    AIL_INT64          ControlFlag,           //in
    AIL_ID *           RemoteContextAppIdPtr  //out
)
```

## Description

This function opens a connection to a local or remote computer that is running a publishing application, through the listening port of the publishing application. This function should be run from the monitoring application and it gives access to the objects that the publishing application publishes (read-only or read/write). Once it opens the connection, this function returns the application context identifier of the publishing application.

You must specify the monitoring application's connection port if the default connection port, set with the Aurora Imaging Configurator utility, is different from the publishing application's listening port. Furthermore, if you have set up a passkey for publishing applications on the remote computer (using the Aurora Imaging Configurator utility on the remote computer), you must specify the remote computer's passkey when establishing a connection from the monitoring application. A passkey permits a secure connection between the publishing and monitoring applications. The passkey is a case-sensitive alphanumeric string of up to 16 characters in length.

> **Note:** Note that the publishing application must have called [`MappControl`](../../Reference/app/MappControl.md) with [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md) set to either [`M_DAIL_CONTROL`](../../Reference/app/MappControl.md) or [`M_DAIL_MONITOR`](../../Reference/app/MappControl.md) before the connection can take place.

Prior to connecting to a Distributed Aurora Imaging Library publishing application, certain conditions must be met. For more information, see [Distributed Aurora Imaging Library](../../UserGuide/C63_Distributed_AIL/ChapterInformation.md).

## Parameters

### `ConnectionDescriptor` *(in, AIL_CONST_TEXT_PTR)*

Specifies the computer to which to connect, and if necessary the listening port of the publishing application. A publishing application must have already enabled publishing using [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md).

*For specifying the remote application*

| Value | Description |
| --- | --- |
| `"dailshm://[Passkey:]localhost[:Port]"` | Opens a connection to a Distributed Aurora Imaging Library publishing application on the local computer using the DAIL SHM protocol. To connect to a publishing application on the local computer using the DAIL SHM protocol, the local computer must have a valid Aurora Imaging Library installation.

When specifying the string that indicates local computer:

1. If required, replace _[Passkey]_ with the passkey for publishing applications on the local computer. The passkey is an alphanumeric string of up to 16 characters in length. If a passkey was set up for the publishing application, you must specify the passkey when establishing a connection from the monitoring application to the publishing application. If a passkey was not set for publishing applications on the local computer, omit the passkey and the ":" that follows it.
2. Following "localhost:", enter the port that the monitoring application should access to initiate a new connection with the publishing application, unless the default connection port is the listening port of the publishing application; in which case, omit the port number and the ":" that follows it.

> **Note:** The DAIL SHM protocol only supports connections to localhost. |
| `"dailtcp://[Passkey:]RemoteComputerName[:Port]"` | Opens a connection to a Distributed Aurora Imaging Library publishing application on a remote or local computer. To connect to a Distributed Aurora Imaging Library remote or local computer, that computer must have a valid Aurora Imaging Library installation.

When specifying the string that indicates the remote computer (or local computer for a publishing application also running locally):

1. If required, replace _[Passkey]_ with the passkey for publishing applications on the remote computer. The passkey is an alphanumeric string of up to 16 characters in length. If a passkey was set up for publishing applications on the remote computer, you must specify the passkey when establishing a connection from the monitoring application to the publishing application. If a passkey was not set for publishing applications on the remote computer, omit the passkey and the ":" that follows it. If you are attempting to connect to a local computer, specify the passkey for publishing applications on the local computer.
2. Replace _RemoteComputerName_ with the remote computer's name or IP address; both IPv4 and IPv6 addresses are supported. A typical IPv4 string has the format n.n.n.n, where _n_ is a number between 0 and 255. A typical IPv6 string has the format x:x:x:x:x:x:x:x, where _x_ is a hexadecimal number between 0000 and FFFF. If you are supplying an IPv6 address, you must use square brackets to separate the address from the port. If you are attempting to connect to a local computer, set this to _localhost_.
3. If required, enter the port that the monitoring application should access on the remote computer to initiate a new connection, unless the default connection port is the listening port of the publishing application on the remote computer; in which case, omit the port number and the ":" that follows it. If you are attempting to connect to a local computer, specify the listening port of the publishing application on the local computer. |

### `InitFlag` *(in, AIL_INT64)*

Reserved for future expansion. Set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. Set to `M_DEFAULT`.

### `RemoteContextAppIdPtr` *(out, *AIL_ID)*

Specifies the address in which to write the remote application context identifier returned by this function.
