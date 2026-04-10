---
doctype: Reference
module: buf
function: MbufControlFeature
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufControlFeature"
---

# MbufControlFeature

| Board | Supported |
| --- | --- |
| Host System | No |
| V4L2 | Yes |
| Clarity UHD | No |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | No |
| GigE Vision | No |
| Indio | No |
| Iris GTX | No |
| Radient eV-CL | No |
| Rapixo CL | No |
| Rapixo CoF | No |
| Rapixo CXP | No |
| USB3 Vision | No |

> Controls a feature of a buffer.

## Syntax

```c
void MbufControlFeature(
    AIL_ID             BufId,        //out
    AIL_INT64          ControlType,  //in
    AIL_CONST_TEXT_PTR FeatureName,  //in
    AIL_INT64          UserVarType,  //in
    const void *       UserVarPtr    //in
)
```

## Description

This function allows you to directly control various GenICam standard feature naming convention (SFNC) features and manufacturer-specific features of buffers. For more information, refer to Using Zebra GenTL Consumer (library).

The features described in this function are primarily available to set existing settings using Aurora Imaging Library code or to set information from which you can build your own interface for the GenTL buffer's description file (XML). Aurora Imaging Library provides two versions of an interface that you can use to interactively inquire the buffer's features. At design-time, you can use Aurora Imaging Intellicam's Feature Browser. At runtime, you can launch Feature Browser, using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/buf/MbufControl.md). Both versions of Feature Browser provide a list of available feature names and associated possible values, as well as code snippets with the Aurora Imaging Library functions and Aurora Imaging Library constants associated with the settings selected; you can copy the code snippets to your Aurora Imaging Library application code.

To inquire a buffer-specific feature, use [`MbufInquireFeature`](../../Reference/buf/MbufInquireFeature.md).

## Parameters

### `BufId` *(out, AIL_ID)*

Specifies the identifier of the buffer to control. This parameter must be given a valid buffer identifier, previously allocated using [`MbufAlloc...`](../../Reference/buf/MbufAllocColor.md).

### `ControlType` *(in, AIL_INT64)*

Specifies the type of information to control for the specified feature.

### `FeatureName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the buffer feature to control.

*For specifying the name of the feature*

| Value | Description |
| --- | --- |
| `"FeatureName"` | Specifies the name of the feature. Note that the feature name is case-sensitive.

Refer to the GenTL library's documentation for a list of the features available. |

### `UserVarType` *(in, AIL_INT64)*

Specifies the data type of the address pointed to by the [`UserVarPtr`](../../Reference/buf/MbufControlFeature.md)parameter. If not setting the feature's value (using [`M_FEATURE_VALUE`](../../Reference/buf/MbufControlFeature.md)), set this parameter to `M_DEFAULT`.

*For specifying the UserVarPtr's data type*

| Value | Description |
| --- | --- |
| `M_TYPE_BOOLEAN` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufControlFeature.md) is passed an address of type_AIL_BOOL_. |
| `M_TYPE_DOUBLE` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufControlFeature.md) is passed an address of type _AIL_DOUBLE_. |
| `M_TYPE_INT64` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufControlFeature.md) is passed an address of type _AIL_INT64_. |
| `M_TYPE_STRING` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufControlFeature.md) is passed an address of type _AIL_TEXT_CHAR_. |
| `M_TYPE_UINT8` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufControlFeature.md) is passed an address of type _AIL_UINT8_. |

*For specifying the length of the array*

| Value | Description |
| --- | --- |
| `M_FEATURE_USER_ARRAY_SIZE` | Specifies the length of the array. |

### `UserVarPtr` *(in, *void)*

Specifies the address of the variable in which to write the value of the feature.

## Parameter Associations

### For specifying the type of information about the feature to set and the data type returned

To determine the data type of the [`FeatureName`](../../Reference/buf/MbufControlFeature.md)specified, use [`MbufInquireFeature`](../../Reference/buf/MbufInquireFeature.md) with [`M_FEATURE_VALUE`](../../Reference/buf/MbufInquireFeature.md).

---

### `M_FEATURE_CHANGE_HOOK`

Sets whether to enable an event to occur when the value of the specified feature changes. Once enabled, use [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/buf/MbufHookFunction.md) to hook a specified function to the feature change event. Repeat for each feature that you want to enable a feature change event. Alternatively, to enable an event to occur when the value of any feature changes and at the same time hook a function to any feature change, use [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/buf/MbufHookFunction.md) + [`M_ALL`](../../Reference/buf/MbufHookFunction.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that no event is generated when the value of the specified feature changes. |
| `M_ENABLE` | Specifies that an event is generated when the value of the specified feature changes. |

---

### `M_FEATURE_EXECUTE`

Sets that the specified command feature must be executed.

---

### `M_FEATURE_MAX`

Specifies to set the feature to its maximum value.

---

### `M_FEATURE_MIN`

Specifies to set the feature to its minimum value.

---

### `M_FEATURE_VALUE`

Sets the current value of the feature.
