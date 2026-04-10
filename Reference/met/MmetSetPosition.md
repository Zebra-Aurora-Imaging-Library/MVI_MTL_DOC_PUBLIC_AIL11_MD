---
doctype: Reference
module: met
function: MmetSetPosition
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetSetPosition"
---

# MmetSetPosition

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Set the position of a feature.

## Syntax

```c
void MmetSetPosition(
    AIL_ID     ContextId,            //out
    AIL_INT    FeatureLabelOrIndex,  //in
    AIL_INT64  TransformationType,   //in
    AIL_DOUBLE Param1,               //in
    AIL_DOUBLE Param2,               //in
    AIL_DOUBLE Param3,               //in
    AIL_DOUBLE Param4,               //in
    AIL_INT64  ControlFlag           //in
)
```

## Description

This function moves the specified feature's position in the metrology template of a metrology context.

This function is valid for all measured ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_MEASURED`](../../Reference/met/MmetAddFeature.md)) and parametrically constructed ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_CONSTRUCTED`](../../Reference/met/MmetAddFeature.md) and [`M_PARAMETRIC`](../../Reference/met/MmetAddFeature.md)) features, unless otherwise specified. Typically, this function is used to move the position of a parametrically constructed local frame (which is considered a feature). When specifying a frame, all features and metrology regions that use that frame as their reference frame are moved accordingly. Note that you can also position features using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_POSITION_X`](../../Reference/met/MmetControl.md) and [`M_POSITION_Y`](../../Reference/met/MmetControl.md). However, in this case you must enter exact values; you cannot, for example, position features based on the results of another module, as you can using [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md) with [`M_RESULT`](../../Reference/met/MmetSetPosition.md).

If you want to move the whole metrology template (for example, when moving the global frame), you should use [`McalFixture`](../../Reference/cal/McalFixture.md) instead of [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md).

Note that if you save the context using [`MmetSave`](../../Reference/met/MmetSave.md), the positional information set using [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md) is not saved with the context; after restoring the context, you will need to call [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md) again.

Aurora Imaging Library moves the specified feature relative to its reference frame, except for the global frame, which Aurora Imaging Library moves relative to its default origin (0,0). If the target image is not calibrated, the global frame's default origin is aligned with the center of the top-left corner pixel of the target image. If the target image is calibrated, the global frame's default origin is aligned with the origin of the relative (world) coordinate system.

If a camera calibration context is associated with the target image, set positional and dimensional values in real-world units. Otherwise, use pixel units.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the identifier of the metrology context. The metrology context must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md).

### `FeatureLabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of the feature you want to position. Set this parameter to one of the following values.

*For specifying a feature*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the global frame. |
| `M_FEATURE_INDEX` | Specifies the index value of an existing individual feature. |
| `M_FEATURE_LABEL` | Specifies the label value of an existing individual feature. |

### `TransformationType` *(in, AIL_INT64)*

Specifies the type of transformation that will be used to set the new position of the feature.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the position to set. Its definition is dependent on the type of position chosen.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the position to set. Its definition is dependent on the type of position chosen.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the position to set. Its definition is dependent on the type of position chosen.

### `Param4` *(in, AIL_DOUBLE)*

Specifies an attribute of the position to set. Its definition is dependent on the type of position chosen.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For selecting the operation that will be used to position the feature

---

### `M_FORWARD_COEFFICIENTS`

Specifies to position the feature using an operation based on forward transformation coefficients, using the equations that follow.  *[Image: MmetSetPosition_ForwardCoefficientsEqn.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient. |

---

### `M_GEOMETRIC`

Specifies to position the feature using an operation based on a translation and rotation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the distance to move the feature, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the distance to move the feature, in pixel or world units. |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_POSITION`

Specifies to position the feature at a specific position and angle. This should be used only for a parametrically constructed local frame.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is 0.0 pixels. |
| `Value` | Specifies the X-coordinate, in pixel or world units. |
| `M_DEFAULT` | Specifies the default value. The default value is 0.0 pixels. |
| `Value` | Specifies the Y-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_RESULT`

Specifies to position the feature at the position calculated by another Aurora Imaging Library module. Supported modules are Code, Geometric Model Finder, Pattern Matching, and String Reader. If you are using a Model Finder result, you can combine [`M_RESULT`](../../Reference/met/MmetSetPosition.md) with [`M_FORWARD_COEFFICIENTS`](../../Reference/met/MmetSetPosition.md). In this case, the transformation uses the forward coefficients of Model Finder's result occurrence.
