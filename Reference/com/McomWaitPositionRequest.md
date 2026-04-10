---
doctype: Reference
module: com
function: McomWaitPositionRequest
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomWaitPositionRequest"
---

# McomWaitPositionRequest

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

> Waits to receive a robot's request for a new position.

## Syntax

```c
void McomWaitPositionRequest(
    AIL_ID       ComId,               //in
    AIL_INT64 *  OperationCodePtr,    //out
    AIL_INT64 *  StatusPtr,           //out
    AIL_INT64 *  ObjectSpecifierPtr,  //out
    AIL_DOUBLE * PositionXPtr,        //out
    AIL_DOUBLE * PositionYPtr,        //out
    AIL_DOUBLE * PositionZPtr,        //out
    AIL_DOUBLE * RotationXPtr,        //out
    AIL_DOUBLE * RotationYPtr,        //out
    AIL_DOUBLE * RotationZPtr,        //out
    AIL_INT64    ControlFlag,         //in
    AIL_DOUBLE   ControlValue         //in
)
```

## Description

This function waits to receive a robot's request for a new position. When a robot controller sends a request for a new position, it also sends it's current position and other relevant information.

You can only use this function when you specify an Industrial Communication context that has been allocated to communicate with a robot controller.

If the time to execute this function is greater than the value set using [`McomControl`](../../Reference/com/McomControl.md) with [`M_COM_TIMEOUT`](../../Reference/com/McomControl.md), an error occurs.

The robot's current rotation about the position axes is received in the [`RotationXPtr`](../../Reference/com/McomWaitPositionRequest.md), [`RotationYPtr`](../../Reference/com/McomWaitPositionRequest.md), and [`RotationZPtr`](../../Reference/com/McomWaitPositionRequest.md) parameters. The table below indicates how the robot controller internally represents this data.

| Robot Type | Robot controller representation |
| --- | --- |
| ABB | The orientation is converted by the robot controller to a quaternion. |
| DENSO | Roll | Pitch | Yaw |
| Epson | W | V | U |
| Fanuc | W | P | R |
| KUKA | C | B | A |
| Staubli | Rx | Ry | Rz |

> **Note:** This function is only available if you have the Aurora Imaging Library Industrial & Robot Communications package, or another relevant update installed.

## Parameters

### `ComId` *(in, AIL_ID)*

Specifies the identifier of the Industrial Communication context. The Industrial Communication context must have been allocated to communicate with a robot controller.

### `OperationCodePtr` *(out, *AIL_INT64)*

Specifies the address of the variable in which to write the operation code (opcode) received from the robot controller. This parameter will return the following value:

*For specifying the operation code*

| Value | Description |
| --- | --- |
| `M_COM_FIND_POSITION_REQUEST` | Specifies that the robot controller requests a new position. |
| `1024 <= Value <= 2047` | Specifies the robot controller requests to perform a user-defined operation, identified by the operation code. Note that you can compare the value returned to the specified address with a 0-based opcode using **M_COM_USER_OPCODE** + 0-based opcode. For example, an opcode of 1026 is equivalent to **M_COM_USER_OPCODE** + 2. |

### `StatusPtr` *(out, *AIL_INT64)*

Specifies the address of the variable in which to write the status code received from the robot controller. This status code indicates whether the robot was successful in moving/using the last specified position.

*For specifying the status code returned by the robot controller*

| Value | Description |
| --- | --- |
| `M_COM_ERROR` | Specifies that the robot controller returns a failure status. |
| `M_COM_SUCCESS` | Specifies that the robot controller returns a success status. |

### `ObjectSpecifierPtr` *(out, *AIL_INT64)*

Specifies the address of the variable in which to write the user identifier associated with the object on which to perform the specified operation; the identifier is established and received from the robot-side software. This is typically used for applications with multiple object types or multiple occurrences of objects.

### `PositionXPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the current X-position of the robot.

### `PositionYPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the current Y-position of the robot.

### `PositionZPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the current Z-position of the robot.

### `RotationXPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the rotation around the positional X-axis of the robot.

### `RotationYPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the rotation around the positional Y-axis of the robot.

### `RotationZPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the rotation around the positional Z-axis of the robot.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ControlValue` *(in, AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_DEFAULT`.
