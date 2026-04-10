---
doctype: Reference
module: com
function: McomControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomControl"
---

# McomControl

| Board | Supported |
| --- | --- |
| Host System | Yes |
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

> Control an Industrial Communication context setting.

## Syntax

```c
void McomControl(
    AIL_ID     ComId,        //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function changes a setting of an Industrial Communication context.

> **Note:** This function is only available if you have the Aurora Imaging Library Industrial & Robot Communications package, or another relevant update installed.

## Parameters

### `ComId` *(out, AIL_ID)*

Specifies the identifier of the Industrial Communication context.

### `ControlType` *(in, AIL_INT64)*

Specifies the Industrial Communication setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For any type of Industrial Communication context

The following [`ControlType`](../../Reference/com/McomControl.md) and [`ControlValue`](../../Reference/com/McomControl.md) parameter settings can be specified for any type of Industrial Communication context:

---

### `M_COM_TIMEOUT`

Sets the maximum amount of time to wait for a receiving function (for example, [`McomRead`](../../Reference/com/McomRead.md) or [`McomWaitPositionRequest`](../../Reference/com/McomWaitPositionRequest.md)) to finish its execution.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum amount of time to wait, in msecs. |

### For an Industrial Communication context for use with a robot controller

The following [`ControlType`](../../Reference/com/McomControl.md) and [`ControlValue`](../../Reference/com/McomControl.md) parameter settings can be specified for an Industrial Communication context used to communicate with a robot controller:

---

### `M_COM_REMOTE_ADDRESS`

Sets the IP address of a robot controller.  If an IP address was specified upon context allocation (using [`McomAlloc`](../../Reference/com/McomAlloc.md) with [`"IPOrName:Port"`](../../Reference/com/McomAlloc.md)), setting this control type will overwrite that address.  Changing the value of this control type will have no effect until you call [`McomControl`](../../Reference/com/McomControl.md) with [`M_COM_ROBOT_CONNECT`](../../Reference/com/McomControl.md). If the Industrial Communication context was previously connected to a robot controller, you must first disconnect it from the robot controller using [`McomControl`](../../Reference/com/McomControl.md) with [`M_COM_ROBOT_DISCONNECT`](../../Reference/com/McomControl.md).

| Value | Description |
| --- | --- |
| `"n.n.n.n"` | Specifies the IP address of the robot controller. Express the address as a string in dotted decimal notation, where each dotted decimal value (_n_) is a number between 0 and 255. |

---

### `M_COM_REMOTE_PORT`

Sets the port on the robot controller to use to communicate with it.  If a port number was specified upon context allocation (using [`McomAlloc`](../../Reference/com/McomAlloc.md) with [`"IPOrName:Port"`](../../Reference/com/McomAlloc.md)), setting this control type will overwrite that port number.  Changing the value of this control type will have no effect until you call [`McomControl`](../../Reference/com/McomControl.md) with [`M_COM_ROBOT_CONNECT`](../../Reference/com/McomControl.md). If the Industrial Communication context was previously connected to a robot controller, you must first disconnect it from the robot controller using [`McomControl`](../../Reference/com/McomControl.md) with [`M_COM_ROBOT_DISCONNECT`](../../Reference/com/McomControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the port on the robot controller. |

---

### `M_COM_ROBOT_CONNECT`

Connects to the robot controller.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_COM_ROBOT_CONNECT_RETRY`

Sets the maximum number of times to attempt to reconnect to the robot controller.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum number of times to attempt to reconnect. |

---

### `M_COM_ROBOT_CONNECT_RETRY_WAIT`

Sets the amount of time to wait between attempts to connect to the robot controller.

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of time to wait, in msecs. |

---

### `M_COM_ROBOT_DISCONNECT`

Disconnects from the robot controller.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
