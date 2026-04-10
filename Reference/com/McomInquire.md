---
doctype: Reference
module: com
function: McomInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomInquire"
---

# McomInquire

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | No |
| Clarity UHD | No |
| Concord PoE | No |
| GenTL | No |
| GevIQ | No |
| GigE Vision | No |
| Indio | Yes |
| Iris GTX | Yes |
| Radient eV-CL | No |
| Rapixo CL | No |
| Rapixo CoF | No |
| Rapixo CXP | No |
| USB3 Vision | No |

> Inquire about an Industrial Communication context setting.

## Syntax

```c
AIL_INT McomInquire(
    AIL_ID    ComId,        //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function inquires about a setting of the Industrial Communication context.

> **Note:** This function is only available if you have the Aurora Imaging Library Industrial & Robot Communications package, or another relevant update installed.

## Parameters

### `ComId` *(in, AIL_ID)*

Specifies the identifier of the Industrial Communication context.

### `InquireType` *(in, AIL_INT64)*

Specifies the setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address of the variable in which to return the value of the inquired setting. Since the [`McomInquire`](../../Reference/com/McomInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about the system

The following [`InquireType`](../../Reference/com/McomInquire.md) parameter settings can be specified for any Industrial Communication context type, to inquire the system on which the context was allocated.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the Industrial Communication context was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For any type of Industrial Communication context

The following [`InquireType`](../../Reference/com/McomInquire.md) parameter settings can be specified for any Industrial Communication context type.

---

### `M_COM_DEVICE_DESCRIPTION`

Inquires the description of the device.

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies to communicate with the default device described in Aurora Imaging Configurator that uses the specified protocol. |
| `"InstanceName"` | Specifies the instance name of a given Industrial Communication protocol. |
| `"IPOrName:Port"` | Specifies the IP address and port on the robot controller to use to communicate with it. |

---

### `M_COM_GET_CONNECTION_STATE`

Inquires whether the device is connected.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the device is not connected. |
| `M_TRUE` | Specifies that the device is connected. |

---

### `M_COM_INIT_VALUE`

Inquires how the Industrial Communication context was initialized.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
| `M_COM_NO_CONNECT` | Specifies not to automatically connect to the robot controller. |

---

### `M_COM_IS_HARDWARE`

Inquires if the Aurora Imaging Library system is using a hardware-assisted acceleration device to communicate with the target device.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the system is not using a hardware-assisted acceleration device. |
| `M_TRUE` | Specifies that the system is using a hardware-assisted acceleration device. |

---

### `M_COM_PROTOCOL_TYPE`

Inquires the type of protocol for which the context was allocated.

| Value | Description |
| --- | --- |
| `M_COM_PROTOCOL_ABB` | Specifies an ABB robot controller protocol. |
| `M_COM_PROTOCOL_CCLINK` | Specifies a CC-Link IE Field Basic protocol. |
| `M_COM_PROTOCOL_DENSO` | Specifies a DENSO robot controller protocol. |
| `M_COM_PROTOCOL_EPSON` | Specifies an Epson robot controller protocol. |
| `M_COM_PROTOCOL_ETHERNETIP` | Specifies an Ethernet/IP protocol. |
| `M_COM_PROTOCOL_FANUC` | Specifies a Fanuc robot controller protocol. |
| `M_COM_PROTOCOL_KUKA` | Specifies a KUKA robot controller protocol. |
| `M_COM_PROTOCOL_MODBUS` | Specifies a Modbus protocol. |
| `M_COM_PROTOCOL_PROFINET` | Specifies a PROFINET protocol. |
| `M_COM_PROTOCOL_STAUBLI` | Specifies a Staubli robot controller protocol. |

---

### `M_COM_TIMEOUT`

Inquires the maximum amount of time to wait for a receiving function (for example, [`McomRead`](../../Reference/com/McomRead.md) or [`McomWaitPositionRequest`](../../Reference/com/McomWaitPositionRequest.md)) to finish its execution.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum amount of time to wait, in msecs. |

### For an Industrial Communication context for use with a robot controller

The following [`InquireType`](../../Reference/com/McomInquire.md) parameter settings can be specified for an Industrial Communication context allocated to communicate with a robot controller.

---

### `M_COM_REMOTE_ADDRESS`

Inquires the IP address of the robot controller.

| Value | Description |
| --- | --- |
| `"n.n.n.n"` | Specifies the IP address of the robot controller. |

---

### `M_COM_REMOTE_PORT`

Inquires the port on the robot controller to use to communicate with it.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the port on the robot controller. |

---

### `M_COM_ROBOT_CONNECT_RETRY`

Inquires the maximum number of times to attempt to reconnect to the robot controller.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum number of times to attempt to reconnect. |

---

### `M_COM_ROBOT_CONNECT_RETRY_WAIT`

Inquires the amount of time to wait between attempts to connect to the robot-controller.

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of time to wait, in msecs. |

---

### `M_COM_ROBOT_ISCONNECTED`

Inquires whether the robot controller is connected.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies the robot controller is not connected. |
| `M_TRUE` | Specifies the robot controller is connected. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### For an Industrial Communication context for use with a PROFINET device

The following [`InquireType`](../../Reference/com/McomInquire.md) parameter settings can be specified for an Industrial Communication context allocated to communicate using the PROFINET protocol ([`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_PROFINET`](../../Reference/com/McomAlloc.md)).

---

### `M_COM_PROFINET_GET_MODULE_INSIZE`

Inquires the amount of memory that the PROFINET controller expects to be allocated to the module in local memory, which can be used to send input data to the controller (for example, a PLC). The expected module's size can be modified using the Siemens Totally Integrated Automation (TIA) portal of the controller.  > **Note:** Note that if the PROFINET controller is not currently connected, the module's last known size is returned.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the expected input module's size, in bytes. |

---

### `M_COM_PROFINET_GET_MODULE_OUTSIZE`

Inquires the amount of memory that the PROFINET controller expects to be allocated to the module in local memory, to which the controller can write. The expected module's size can be modified using the Siemens Totally Integrated Automation (TIA) portal of the controller.  > **Note:** Note that if the PROFINET controller is not currently connected, the module's last known size is returned.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the expected output module's size, in bytes. |

---

### `M_COM_PROFINET_GET_PLC_STATE`

Inquires the state of the PROFINET controller (PLC) connected to the local computer. The inquired controller state is the state as of the last alarm transfer cycle.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_COM_PROFINET_STATUS_RUN` | Specifies that the PROFINET PLC is running. |
| `M_COM_PROFINET_STATUS_STOP` | Specifies that the PROFINET PLC has stopped. |

### Combination Constants — For a PROFINET PLC in a redundant mode

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify in which redundant mode the PLC is running.

| Value | Description |
| --- | --- |
| `M_COM_PROFINET_STATUS_BACKUP` | Specifies that the PROFINET PLC, running in a redundant mode, is using the backup network channel. |
| `M_COM_PROFINET_STATUS_PRIMARY` | Specifies that the PROFINET PLC, running in a redundant mode, is using the primary network channel. |

### Combination Constants — To specify the module's index in a PROFINET automation device

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the module's index in the local computer.

| Value | Description |
| --- | --- |
| `0 <= Index <= 15` | Specifies the module's index in the local computer. |

### For an Industrial Communication context for use with a EtherNet/IP device

The following [`InquireType`](../../Reference/com/McomInquire.md) parameter settings can be specified for an Industrial Communication context allocated to communicate using the EtherNet/IP protocol ([`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_ETHERNETIP`](../../Reference/com/McomAlloc.md)).

---

### `M_COM_ETHERNETIP_CONFIG_SIZE`

Inquires the amount of memory that is allocated to the configuration assembly in local memory, from which the controller can read EtherNet/IP configuration data that you have written about the local computer.  > **Note:** Note that the size of this assembly can be changed using the Aurora Imaging Configurator utility. The controller must be configured to recognize this size.

| Value | Description |
| --- | --- |
| `Value` | Specifies the size of the configuration assembly, in bytes. |

---

### `M_COM_ETHERNETIP_CONSUMER_SIZE`

Inquires the amount of memory that is allocated to the consumer assembly in local memory, to which the controller can write.  > **Note:** Note that the size of this assembly can be changed using the Aurora Imaging Configurator utility. The controller must be configured to recognize this size.

| Value | Description |
| --- | --- |
| `Value` | Specifies the size of the consumer assembly, in bytes. |

---

### `M_COM_ETHERNETIP_PRODUCER_SIZE`

Inquires the amount of memory that is allocated to the producer assembly in local memory, from which the controller can read.  > **Note:** Note that the size of this assembly can be changed using the Aurora Imaging Configurator utility. The controller must be configured to recognize this size.

| Value | Description |
| --- | --- |
| `Value` | Specifies the size of the producer assembly, in bytes. |

### For an Industrial Communication context for use with a CC-Link IE Field Basic device

The following [`InquireType`](../../Reference/com/McomInquire.md) parameter settings can be specified for an Industrial Communication context allocated to communicate using the CC-Link IE Field Basic protocol ([`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_CCLINK`](../../Reference/com/McomAlloc.md)).

---

### `M_COM_CCLINK_TOTAL_OCCUPIED_STATIONS`

Inquires the number of stations occupied by the CC-Link IE Field Basic protocol instance. This determines the size of the register and flag data tables available for the controller to read from and write to. For each station, there are 32 input registers, 32 output registers, 64 input flags, and 64 output flags.  > **Note:** Note that the number of stations occupied by the protocol instance can be changed using the Aurora Imaging Configurator utility. The controller must be configured to make this number of stations available.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of stations. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.
