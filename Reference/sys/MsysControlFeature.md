---
doctype: Reference
module: sys
function: MsysControlFeature
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sys / MsysControlFeature"
---

# MsysControlFeature

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

> Controls a feature of the camera.

## Syntax

```c
void MsysControlFeature(
    AIL_ID             SysId,        //in
    AIL_INT64          ControlType,  //in
    AIL_CONST_TEXT_PTR FeatureName,  //in
    AIL_INT64          UserVarType,  //in
    const void *       UserVarPtr    //in
)
```

## Description

This function allows you to directly control various GenICam standard feature naming convention (SFNC) features and manufacturer-specific features of the camera. For more information, refer to [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

The features described in this function are primarily available to set existing settings using Aurora Imaging Library code or to set information from which you can build your own interface for the camera device's description file (XML). Aurora Imaging Library provides two versions of an interface that you can use to interactively inquire the camera's features. At design-time, you can use Aurora Imaging Intellicam's Feature Browser. At runtime, you can launch the Feature Browser, using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md). Both versions of the Feature Browser provide a list of available feature names and associated possible values, as well as code snippets with the Aurora Imaging Library functions and Aurora Imaging Library constants associated with the settings selected; you can copy the code snippets to your Aurora Imaging Library application code.

To inquire a camera's manufacturer-specific feature, use [`MsysInquireFeature`](../../Reference/sys/MsysInquireFeature.md).

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system accessing the camera to control. This parameter must be given a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md).

### `ControlType` *(in, AIL_INT64)*

Specifies the type of information to control for the specified feature.

### `FeatureName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the camera feature to control.

*For specifying the name of the feature*

| Value | Description |
| --- | --- |
| `"FeatureName"` | Specifies the name of the feature. Note that the feature name is case-sensitive.

Refer to your camera's documentation for a list of the features available. |

### `UserVarType` *(in, AIL_INT64)*

Specifies the data type of the address pointed to by the [`UserVarPtr`](../../Reference/sys/MsysControlFeature.md)parameter. If not setting the feature's value (using [`M_FEATURE_VALUE`](../../Reference/sys/MsysControlFeature.md)), set this parameter to `M_DEFAULT`.

*For specifying the UserVarPtr's data type*

| Value | Description |
| --- | --- |
| `M_TYPE_BOOLEAN` | Specifies that [`UserVarPtr`](../../Reference/sys/MsysControlFeature.md) is passed an address of type_AIL_BOOL_. |
| `M_TYPE_DOUBLE` | Specifies that [`UserVarPtr`](../../Reference/sys/MsysControlFeature.md) is passed an address of type _AIL_DOUBLE_. |
| `M_TYPE_INT64` | Specifies that [`UserVarPtr`](../../Reference/sys/MsysControlFeature.md) is passed an address of type _AIL_INT64_. |
| `M_TYPE_STRING` | Specifies that [`UserVarPtr`](../../Reference/sys/MsysControlFeature.md) is passed an address of type _AIL_TEXT_CHAR_. |
| `M_TYPE_UINT8` | Specifies that [`UserVarPtr`](../../Reference/sys/MsysControlFeature.md) is passed an address of type _AIL_UINT8_. |

*For specifying the length of the array*

| Value | Description |
| --- | --- |
| `M_FEATURE_USER_ARRAY_SIZE` | Specifies the length of the array. |

### `UserVarPtr` *(in, *void)*

Specifies the address of the variable in which to write the value of the feature.

## Parameter Associations

### For specifying the type of information about the feature to set and the data type returned

To determine the data type of the [`FeatureName`](../../Reference/sys/MsysControlFeature.md)specified, use [`MsysInquireFeature`](../../Reference/sys/MsysInquireFeature.md) with [`M_FEATURE_VALUE`](../../Reference/dig/MdigInquireFeature.md).

---

### `M_FEATURE_CHANGE_HOOK`

Sets whether to enable an event to occur when the value of the specified feature changes. Once enabled, use [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/sys/MsysHookFunction.md) to hook a specified function to the feature change event. Repeat for each feature that you want to enable a feature change event. Alternatively, to enable an event to occur when the value of any feature changes and at the same time hook a function to any feature change, use [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/sys/MsysHookFunction.md) + [`M_ALL`](../../Reference/sys/MsysHookFunction.md).

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

### Combination Constants — For specifying the configuration file associated with the feature

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the GenTL configuration file (XML file) that is associated with the feature.

| Value | Description |
| --- | --- |
| `M_GENTL_INTERFACE_NUMBER` | Specifies which instance of the GenTL interface configuration file is associated with the feature. |
| `M_GENTL_SYSTEM` *(default)* | Specifies to display the GenTL system configuration information. |
