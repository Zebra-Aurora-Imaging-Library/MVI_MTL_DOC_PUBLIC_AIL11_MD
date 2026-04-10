---
doctype: Reference
module: com
function: McomSendPosition
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomSendPosition"
---

# McomSendPosition

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

> Sends a position to the robot. This does not move the robot, but simply sends a found position to the robot controller.

## Syntax

```c
void McomSendPosition(
    AIL_ID     ComId,            //in
    AIL_INT64  OperationCode,    //in
    AIL_INT64  Status,           //in
    AIL_INT64  ObjectSpecifier,  //in
    AIL_DOUBLE PositionX,        //in
    AIL_DOUBLE PositionY,        //in
    AIL_DOUBLE PositionZ,        //in
    AIL_DOUBLE RotationX,        //in
    AIL_DOUBLE RotationY,        //in
    AIL_DOUBLE RotationZ,        //in
    AIL_INT64  ControlFlag,      //in
    AIL_DOUBLE ControlValue      //in
)
```

## Description

This function sends a position to the robot controller. It is typically used to change the robot's position to the specified coordinates.

You can only use this function when you specify an Industrial Communication context that has been allocated to communicate with a robot controller.

Use [`RotationX`](../../Reference/com/McomSendPosition.md), [`RotationY`](../../Reference/com/McomSendPosition.md), [`RotationZ`](../../Reference/com/McomSendPosition.md) to specify rotations about the different positional axes. The table below indicates how the robot controllers internally represent this data.

| Robot type | Robot controller representation |
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

### `OperationCode` *(in, AIL_INT64)*

Specifies the operation code (opcode) of the command. This parameter must be set one of to the following values:

*For specifying the operation code*

| Value | Description |
| --- | --- |
| `M_COM_ROBOT_FIND_POSITION_RESULT` | Specifies that the Aurora Imaging Library application is sending a new position to the robot controller. |
| `1024 <= Value <= 2047` | Specifies the value of the user-defined operation code. Note that if you want to start the user operation code offset at 0 instead of 1024, you can use **M_COM_USER_OPCODE** + opcode value. |

### `Status` *(in, AIL_INT64)*

Specifies the status code to send to the robot controller.

*For specifying the status code sent to the robot controller*

| Value | Description |
| --- | --- |
| `M_COM_ERROR` | Specifies that the Aurora Imaging Library application was not successful in finding a new position or that the robot should stop. |
| `M_COM_SUCCESS` | Specifies that the Aurora Imaging Library application was successful in finding a new position. |

### `ObjectSpecifier` *(in, AIL_INT64)*

Specifies the user identifier associated with the object at the specified position. This should ideally correspond to the value received from [`ObjectSpecifierPtr`](../../Reference/com/McomWaitPositionRequest.md) during the previous call of the [`McomWaitPositionRequest`](../../Reference/com/McomWaitPositionRequest.md) function.

### `PositionX` *(in, AIL_DOUBLE)*

Specifies the X-position to the robot controller.

### `PositionY` *(in, AIL_DOUBLE)*

Specifies the Y-position to the robot controller.

### `PositionZ` *(in, AIL_DOUBLE)*

Specifies the Z-position to the robot controller.

### `RotationX` *(in, AIL_DOUBLE)*

Specifies the rotation around the positional X-axis to the robot controller.

### `RotationY` *(in, AIL_DOUBLE)*

Specifies the rotation around the positional Y-axis to the robot controller.

### `RotationZ` *(in, AIL_DOUBLE)*

Specifies the rotation around the positional Z-axis to the robot controller.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ControlValue` *(in, AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_DEFAULT`.
