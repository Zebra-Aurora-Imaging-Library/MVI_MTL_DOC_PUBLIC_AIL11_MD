---
doctype: Reference
module: buf
function: MbufInquireFeature
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufInquireFeature"
---

# MbufInquireFeature

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

> Inquires a feature of the buffer.

## Syntax

```c
void MbufInquireFeature(
    AIL_ID             BufId,        //in
    AIL_INT64          InquireType,  //in
    AIL_CONST_TEXT_PTR FeatureName,  //in
    AIL_INT64          UserVarType,  //in
    void *             UserVarPtr    //out
)
```

## Description

This function allows you to directly inquire various GenICam GenTL standard feature naming convention (SFNC) features and buffer-specific features. When used with a GenICam GenTL SFNC-compliant camera, it allows you to directly inquire various buffer-specific features specified with the GenTL buffer description file (XML). Note that for the purpose of this function, a feature can also be of type category or command. Although category and command features lack a feature value, all other attributes of the feature can be inquired. For a list of standard category feature names, see the GenICam GenTL _Standard Feature Naming Convention_ or your GenTL-compliant camera's documentation.

The features described in this function are primarily available to check existing settings using Aurora Imaging Library code or to retrieve information from which you can build your own interface for the GenTL buffer's description file (XML). Aurora Imaging Library provides two versions of an interface that you can use to interactively inquire the camera's features. At design-time, you can use Aurora Imaging Intellicam's Feature Browser. At runtime, you can launch Feature Browser, using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/buf/MbufControl.md). Both versions of Feature Browser provide a list of available feature names and associated possible values, as well as code snippets with the Aurora Imaging Library functions and Aurora Imaging Library constants associated with the settings selected; you can copy the code snippets to your Aurora Imaging Library application code.

Occasionally, the returned information is a string whose length can change between when you inquires its size and when you inquire its value (for example, the temperature can change from 99 to 100 between calls). In this case, when you inquire the information's length to allocate an array of an appropriate size, you might allocate an array that is too small to retrieve the data and experience an application crash. To avoid an application crash, specify the length of the array passed to the [`UserVarPtr`](../../Reference/buf/MbufInquireFeature.md)parameter using the**M_FEATURE_USER_ARRAY_SIZE()** macro; the data will be truncated to fit in the array and an error will be generated.

To control a buffer-specific feature, use [`MbufControlFeature`](../../Reference/buf/MbufControlFeature.md).

## Parameters

### `BufId` *(in, AIL_ID)*

Specifies the identifier of the buffer to inquire. This parameter must be given a valid buffer identifier, previously allocated using [`MbufAlloc...`](../../Reference/buf/MbufAllocColor.md).

### `InquireType` *(in, AIL_INT64)*

Specifies the type of information to inquire about the feature.

### `FeatureName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the buffer feature to inquire. Note that this parameter expects a case-sensitive string.

*For specifying the name of the feature*

| Value | Description |
| --- | --- |
| `"FeatureName"` | Specifies the name of the feature. Note that the feature name is case-sensitive.

Refer to your GenTL library's documentation for a list of the features available. |
| `"Root"` | Specifies to inquire the highest-level feature of the XML structure; this feature is of type category and typically, its subfeatures are also of type category. Note that this value is only available when using [`M_SUBFEATURE_...`](../../Reference/buf/MbufInquireFeature.md). |

### `UserVarType` *(in, AIL_INT64)*

Specifies the data type of the address pointed to by the [`UserVarPtr`](../../Reference/buf/MbufInquireFeature.md)parameter. If not inquiring the feature's value (using [`M_FEATURE_VALUE`](../../Reference/buf/MbufInquireFeature.md)), set this parameter to `M_DEFAULT`.

*For specifying the UserVarPtr's data type*

| Value | Description |
| --- | --- |
| `M_TYPE_BOOLEAN` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufInquireFeature.md) is passed an address of type_AIL_BOOL_. |
| `M_TYPE_DOUBLE` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufInquireFeature.md) is passed an address of type _AIL_DOUBLE_. |
| `M_TYPE_INT64` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufInquireFeature.md) is passed an address of type _AIL_INT64_. |
| `M_TYPE_STRING` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufInquireFeature.md) is passed an address of type _AIL_TEXT_CHAR_. |
| `M_TYPE_UINT8` | Specifies that [`UserVarPtr`](../../Reference/buf/MbufInquireFeature.md) is passed an address of type _AIL_UINT8_. |

*For specifying the length of the array*

| Value | Description |
| --- | --- |
| `M_FEATURE_USER_ARRAY_SIZE` | Specifies the length of the array. |

### `UserVarPtr` *(out, *void)*

Specifies the address of the variable in which to return the requested information.

## Parameter Associations

### For specifying the type of information to inquire about the feature and the data type returned

The following inquire types are used to specify the type of information about the feature to inquire and the data type returned.

---

### `M_FEATURE_ACCESS_MODE`

**Board availability:** GenTL, V4L2

Inquires whether the specified feature is implemented, available, readable, and/or writable.  Note that an error is generated if the feature does not exist in the camera's device description file (XML). To learn if the specified feature exists in the file and is implemented on your camera, use [`M_FEATURE_PRESENT`](../../Reference/buf/MbufInquireFeature.md).

| Value | Description |
| --- | --- |
| `Bit-encoded access mode value` | Specifies a bit-encoded value that details the access mode. To establish the access mode from the encoded value, use the following macros:  - To learn whether the specified feature is available, use the **M_FEATURE_IS_AVAILABLE** macro. Note that if a feature is not available, use **M_FEATURE_IS_IMPLEMENTED** to learn whether the specified feature is only temporarily unavailable or actually not implemented on your camera. A feature that is temporarily unavailable typically relies on the setting of another feature (for example, when a trigger feature is set to off, its associated trigger polarity feature would be unavailable). - To learn whether the specified feature is implemented, use the **M_FEATURE_IS_IMPLEMENTED** macro.   For example, a Bayer color feature would not be implemented on a monochrome camera because the hardware cannot support it. - To learn whether the specified feature can be read, use the **M_FEATURE_IS_READABLE** macro. - To learn whether the specified feature can be written to, use the **M_FEATURE_IS_WRITABLE** macro. |

---

### `M_FEATURE_CACHING_MODE`

**Board availability:** GenTL, V4L2

Inquires whether a copy of the specified feature's value is stored in Host memory for faster access and retrieval. If so, the feature is known as a cachable feature. A cachable feature is typically only retrieved from the camera whenever the cache is invalidated (such as, before the first read and before the first read to occur after a write operation). A cachable feature is typically a value that only changes based on user-interaction (setting a control value). For example, a temperature feature is not cachable.  To return a true/false value for this inquire, use the **M_FEATURE_IS_CACHABLE** macro on the returned result.

| Value | Description |
| --- | --- |
| `M_FEATURE_CACHING_MODE_NONE` | Specifies that the feature is not cachable. |
| `M_FEATURE_CACHING_MODE_WRITE_AROUND` | Specifies that feature's values are written only to the camera, and the cache is updated the next time the feature's value is inquired. |
| `M_FEATURE_CACHING_MODE_WRITE_THROUGH` | Specifies that when information is written to the camera, the cache is updated simultaneously. |

---

### `M_FEATURE_CHANGE_HOOK`

**Board availability:** GenTL, V4L2

Inquires whether an event occurs when the value of the specified feature changes. If enabled, [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/buf/MbufHookFunction.md) hooks a specified function to the feature change event.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that no event is generated when the value of the specified feature changes. |
| `M_ENABLE` | Specifies that an event is generated when the value of the specified feature changes. |

---

### `M_FEATURE_DEPRECATED`

**Board availability:** GenTL, V4L2

Inquires whether the specified feature is deprecated and should no longer be used.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the feature is not depreciated. |
| `M_TRUE` | Specifies that the feature is depreciated. |

---

### `M_FEATURE_DESCRIPTION`

**Board availability:** GenTL, V4L2

Inquires the description of the specified feature.

| Value | Description |
| --- | --- |
| `Value` | Specifies the description of the feature. |

---

### `M_FEATURE_DISPLAY_NAME`

**Board availability:** GenTL, V4L2

Inquires the specified feature's display name. Note that this can differ slightly from the internally-used feature name ([`M_FEATURE_NAME`](../../Reference/buf/MbufInquireFeature.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the display name of the feature. |

---

### `M_FEATURE_EXECUTE_COMPLETED`

**Board availability:** GenTL, V4L2

Inquires whether the specified executable camera feature (command) has finished executing on your camera.  Note that to enable command execution-completion polling, set [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_FEATURE_EXECUTE_POLLING_MODE`](../../Reference/sys/MsysControl.md) to [`M_AUTOMATIC`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the executable camera feature has not finished executing. |
| `M_TRUE` | Specifies that the executable camera feature has finished executing. |

---

### `M_FEATURE_INCREMENT`

**Board availability:** GenTL, V4L2

Inquires the value by which the feature's value increments. For example, if a feature's possible values are 2, 4, 6, ..., then the feature's value increments by 2.

| Value | Description |
| --- | --- |
| `Value` | Specifies the feature's incremental value. |

---

### `M_FEATURE_MAX`

**Board availability:** GenTL, V4L2

Inquires the maximum possible value for the feature.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum possible value for the feature. |

---

### `M_FEATURE_MIN`

**Board availability:** GenTL, V4L2

Inquires the minimum possible value for the feature.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum possible value for the feature. |

---

### `M_FEATURE_NAME`

**Board availability:** GenTL, V4L2

Inquires the specified internally-used feature name. Note that this can differ slightly from the displayed name of the feature ([`M_FEATURE_DISPLAY_NAME`](../../Reference/buf/MbufInquireFeature.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the feature's name. |

---

### `M_FEATURE_POLLING_INTERVAL`

**Board availability:** GenTL, V4L2

Inquires the interval between inquiring an executable feature's completion status, when automatically polling (using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_FEATURE_EXECUTE_POLLING_MODE`](../../Reference/sys/MsysControl.md)).

| Value | Description |
| --- | --- |
| `Value >= -1` | Specifies the polling interval, in msecs. |

---

### `M_FEATURE_PRESENT`

**Board availability:** GenTL, V4L2

Inquires whether the specified feature is present in the camera's device description file. Note that, if the feature is listed in the camera's device description file (XML), use[`M_FEATURE_ACCESS_MODE`](../../Reference/buf/MbufInquireFeature.md) and the **M_FEATURE_IS_IMPLEMENTED** macro to determine if the feature is supported. If the feature is not present in the file, [`M_FEATURE_PRESENT`](../../Reference/buf/MbufInquireFeature.md) will return [`M_NO`](../../Reference/buf/MbufInquireFeature.md), whereas [`M_FEATURE_ACCESS_MODE`](../../Reference/buf/MbufInquireFeature.md) will generate an error.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the specified feature is not present. |
| `M_YES` | Specifies that the specified feature is present. |

---

### `M_FEATURE_REPRESENTATION`

**Board availability:** GenTL, V4L2

Inquires the display format for the feature's value. The display format dictates the size of the field and its form (check box, slider, text-box, etc.) used in a user interface for the GenTL buffer's description file (XML), such as Feature Browser (accessible through Aurora Imaging Intellicam or[`MbufControl`](../../Reference/buf/MbufControl.md)with[`M_GC_FEATURE_BROWSER`](../../Reference/buf/MbufControl.md).  Note that this inquire type can only be used with a feature that has a data type of _AIL_INT64_ or _AIL_DOUBLE_.

| Value | Description |
| --- | --- |
| `M_FEATURE_REPRESENTATION_BOOLEAN` | Specifies that the feature should be displayed as a checkbox. |
| `M_FEATURE_REPRESENTATION_HEX_NUMBER` | Specifies that the feature should be displayed as a hexadecimal value. |
| `M_FEATURE_REPRESENTATION_IPV4_ADDRESS` | Specifies that the feature should be displayed as an IP address in dotted decimal notation. |
| `M_FEATURE_REPRESENTATION_LINEAR` | Specifies that the feature should be displayed as a slider with the appropriate range shown. |
| `M_FEATURE_REPRESENTATION_LOGARITHMIC` | Specifies that the feature should be displayed as a slider with the appropriate logarithmic range shown. |
| `M_FEATURE_REPRESENTATION_MAC_ADDRESS` | Specifies that the feature should be displayed as a hexadecimal MAC address. |
| `M_FEATURE_REPRESENTATION_PURE_NUMBER` | Specifies that the feature should be displayed as edit text-box with a decimal display. |

---

### `M_FEATURE_SELECTOR_COUNT`

**Board availability:** GenTL, V4L2

Inquires the number of selectors that are ancestors of the feature.  Selectors take an enumerator or integer to index which instance of a subfeature should be accessed, when multiple instances of a subfeature exist. For example, the GenICam standard `TriggerSelector` feature is used to select which type of trigger to configure ( for example,`FrameStart` and `AcquisitionStart`are both enumerators that can be passed to `TriggerSelector`). All subfeatures of the selector (such as its source) are referenced from the trigger selector (`TriggerSource[AcquisitionStart]`).  This inquire type only returns a value greater than 0 if the feature is a subfeature of a selector. It returns 1 if the feature is a subfeature of a selector. It returns 2 if its selector is also referenced by another selector. The [`M_FEATURE_SELECTOR_COUNT`](../../Reference/buf/MbufInquireFeature.md) for `TriggerMode`, in this example, would be 1.  To inquires the name of the selector associated with the specified feature, use [`M_FEATURE_SELECTOR_NAME + n`](../../Reference/buf/MbufInquireFeature.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of ancestor selectors of the feature. |

---

### `M_FEATURE_SELECTOR_NAME + n`

**Board availability:** GenTL, V4L2

Inquires the name of the ancestor selector of the specified subfeature, where _n_ is the number of generations. So, if inquiring the feature selector name of the 1st selector of `TriggerSource`, the returned name would be `TriggerSelector`.  To inquires the number of selectors that are associated with the specified feature, use [`M_FEATURE_SELECTOR_COUNT`](../../Reference/buf/MbufInquireFeature.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the name associated with the selector index. |

---

### `M_FEATURE_SIZE`

**Board availability:** GenTL, V4L2

Inquires the size of the feature's value.

| Value | Description |
| --- | --- |
| `Value` | Specifies the feature's size, in bytes. |

---

### `M_FEATURE_STREAMABLE`

**Board availability:** GenTL, V4L2

Inquires whether the specified feature is streamable. A streamable feature can be stored in the local camera's description file (an XML file stored on your computer). Streamable features are typically not persistent values (such as pixel format and image size). In most cases, a streamable feature is capable of being saved and loaded from file at a later date, possibly on a different camera.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the feature is not streamable. |
| `M_TRUE` | Specifies that the feature is streamable. |

---

### `M_FEATURE_TOOLTIP`

**Board availability:** GenTL, V4L2

Inquires the tooltip of the specified feature.

| Value | Description |
| --- | --- |
| `Value` | Specifies the feature's tooltip. |

---

### `M_FEATURE_TYPE`

**Board availability:** GenTL, V4L2

Inquires the specified feature's type.

| Value | Description |
| --- | --- |
| `M_TYPE_BOOLEAN` | Specifies that the feature's value is a boolean. |
| `M_TYPE_CATEGORY` | Specifies that the feature is a category feature, and its value cannot be inquired. |
| `M_TYPE_COMMAND` | Specifies that the feature is a command and its value cannot be inquired. |
| `M_TYPE_DOUBLE` | Specifies that the feature's value is a floating-point. |
| `M_TYPE_ENUMERATION` | Specifies that the feature's value is an enumeration. |
| `M_TYPE_INT64` | Specifies that the feature's value is a 64-bit integer. |
| `M_TYPE_REGISTER` | Specifies that the feature's value is mapped to a multi-byte register. |
| `M_TYPE_STRING` | Specifies that the feature's value is a string. |

---

### `M_FEATURE_VALUE`

**Board availability:** GenTL, V4L2

Inquires the current value of the feature.

| Value | Description |
| --- | --- |
| `Value` | Specifies the feature's value. |

---

### `M_FEATURE_VALUE_ARRAY`

**Board availability:** GenTL, V4L2

Inquires the array of values of the specified feature. The array of values will be inquired without needing to be indexed by a selector. This is internally handled by Aurora Imaging Library and simplifies implementation.

---

### `M_FEATURE_VALUE_ARRAY_SIZE`

**Board availability:** GenTL, V4L2

Inquires the number of elements in the specified feature's array of values. For use with [`M_FEATURE_VALUE_ARRAY`](../../Reference/buf/MbufInquireFeature.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of elements in the feature array. |

---

### `M_FEATURE_VISIBILITY`

**Board availability:** GenTL, V4L2

Inquires the visibility level of the specified feature. The visibility level indicates the level of expertise required to view or select the feature.  When creating a custom user interface for the camera's device description file (XML), the visibility level of the specified feature can be used to potentially hide the specified feature. The current level of visibility should be set within your custom user interface, and the enumeration entry for the visibility level of the specified feature must be compared against it. If the specified enumeration entry has a level of visibility higher than the current level of visibility, it is hidden.

| Value | Description |
| --- | --- |
| `M_FEATURE_VISIBILITY_BEGINNER` | Specifies that the feature is suggested for beginners. |
| `M_FEATURE_VISIBILITY_EXPERT` | Specifies that the feature is suggested for experts. |
| `M_FEATURE_VISIBILITY_GURU` | Specifies that the feature is suggested for advanced experts. |
| `M_FEATURE_VISIBILITY_INVISIBLE` | Specifies that the feature should not be shown in the user interface for the camera's device description file (XML). Note that an invisible feature can still be set or inquired using [`MbufControlFeature`](../../Reference/buf/MbufControlFeature.md) and [`MbufInquireFeature`](../../Reference/buf/MbufInquireFeature.md), respectively. |

### For inquiring about the enumeration entry of a feature's value

When the specified feature is of type enumeration, you can use one of the following to inquire about the specified enumeration entry in the supported enumeration list of the feature. When the information pertains to a specific enumeration entry (for example, [`M_FEATURE_ENUM_ENTRY_ACCESS_MODE + n`](../../Reference/buf/MbufInquireFeature.md)), the value is described in relation to its position in the enumerated list, where _n_ is the index into the enumerated list.  > **Note:** Note that these values can only be used when the feature type to inquire is an enumeration.

> **Board availability:** GenTL, V4L2

---

### `M_FEATURE_ENUM_ENTRY_ACCESS_MODE + n`

Inquires whether the specified enumeration entry is implemented, available, readable, and/or writable, where _n_ is the index into the enumerated list.  Note that an error is generated if the enumeration entry does not exist in the camera's device description file (XML). To learn if the specified enumeration entry exists in the file and is implemented on your camera, use the **M_FEATURE_IS_IMPLEMENTED** macro.

| Value | Description |
| --- | --- |
| `Bit-encoded access mode value` | Specifies a bit-encoded value that details the access mode. To establish the access mode from the encoded value, use the following macros:  - To learn whether the specified enumeration entry is available, use the **M_FEATURE_IS_AVAILABLE** macro. Note that if an enumeration entry is not available, use **M_FEATURE_IS_IMPLEMENTED** to learn whether the specified enumeration entry is only temporarily unavailable or actually not implemented on your camera. An enumeration entry that is temporarily unavailable typically relies on the setting of another feature or enumeration entry. - To learn whether the specified enumeration entry is implemented, use the **M_FEATURE_IS_IMPLEMENTED** macro. - To learn whether the specified enumeration entry can be read, use the **M_FEATURE_IS_READABLE** macro. |

---

### `M_FEATURE_ENUM_ENTRY_COUNT`

Inquires the total number of enumeration entries in the supported enumerated list of the feature. Note that this inquire type can only be used when the feature to inquire is of type enumeration.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of enumeration entries. |

---

### `M_FEATURE_ENUM_ENTRY_DESCRIPTION + n`

Inquires the description of the specified enumeration entry, where _n_ is the index into the enumerated list. Note that this inquire type can only be used when the feature type to inquire is an enumeration.

| Value | Description |
| --- | --- |
| `Value` | Specifies the enumeration entry's description. |

---

### `M_FEATURE_ENUM_ENTRY_DISPLAY_NAME + n`

Inquires the display name of the specified enumeration entry, where _n_ is the index into the enumerated list. Note that this can differ slightly from the internally-used name of the entry ([`M_FEATURE_ENUM_ENTRY_NAME + n`](../../Reference/buf/MbufInquireFeature.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the enumeration entry's display name. |

---

### `M_FEATURE_ENUM_ENTRY_NAME + n`

Inquires the internally-used name of the specified enumeration entry of the feature, where _n_ is the index into the enumerated list. Note that this can differ slightly from the display name of the entry ([`M_FEATURE_ENUM_ENTRY_DISPLAY_NAME + n`](../../Reference/buf/MbufInquireFeature.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the enumeration entry's name. |

---

### `M_FEATURE_ENUM_ENTRY_TOOLTIP + n`

Inquires the tool-tip for the specified enumeration entry, where _n_ is the index into the enumerated list.

| Value | Description |
| --- | --- |
| `Value` | Specifies the enumeration entry's tooltip. |

---

### `M_FEATURE_ENUM_ENTRY_VALUE + n`

Inquires the enumeration integer value of the specified enumeration entry, where _n_ is the index into the enumerated list.

| Value | Description |
| --- | --- |
| `Value` | Specifies the enumeration entry's integer value. |

---

### `M_FEATURE_ENUM_ENTRY_VISIBILITY + n`

Inquires the visibility level of the specified enumeration entry, where _n_ is the index into the enumerated list. The visibility level indicates the level of expertise required to view or select the enumeration entry.  When creating a custom user interface for the camera's device description file (XML), the visibility level of the specified enumeration entry can be used to potentially hide the specified enumeration entry. The current level of visibility should be set within your custom user interface, and the visibility level of the specified enumeration entry must be compared against it. If the specified enumeration entry has a level of visibility higher than the current level of visibility, it is hidden.

| Value | Description |
| --- | --- |
| `M_FEATURE_VISIBILITY_BEGINNER` | Specifies that the enumeration entry is suggested for beginners. |
| `M_FEATURE_VISIBILITY_EXPERT` | Specifies that the enumeration entry is suggested for experts. |
| `M_FEATURE_VISIBILITY_GURU` | Specifies that the enumeration entry is suggested for advanced experts. |
| `M_FEATURE_VISIBILITY_INVISIBLE` | Specifies that the enumeration entry should not be shown in the user interface for the camera's device description file (XML). Note that an invisible feature can still be set or inquired using [`MbufControlFeature`](../../Reference/buf/MbufControlFeature.md) and [`MbufInquireFeature`](../../Reference/buf/MbufInquireFeature.md), respectively. |

### For inquiring about the subfeatures of a feature

When the specified feature is of type category or [`"Root"`](../../Reference/buf/MbufInquireFeature.md), you can use one of the following to inquire about its subfeatures. This is useful for enumerating the features of your camera.

> **Board availability:** GenTL, V4L2

---

### `M_SUBFEATURE_COUNT`

Inquires the number of subfeatures or subcategories that the specified category feature (node) has.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of subfeatures. |

---

### `M_SUBFEATURE_NAME + n`

Inquires the name of the specified subfeature, where _n_ is the index of subfeature.

| Value | Description |
| --- | --- |
| `Value` | Specifies the name of the subfeature. |

---

### `M_SUBFEATURE_TYPE + n`

Inquires the type of the specified subfeature, where _n_ is the index of subfeature.

| Value | Description |
| --- | --- |
| `M_TYPE_BOOLEAN` | Specifies that the feature's value is a boolean. |
| `M_TYPE_CATEGORY` | Specifies that the feature is a category. |
| `M_TYPE_COMMAND` | Specifies that the feature is a command to be executed. |
| `M_TYPE_DOUBLE` | Specifies that the feature's value is a floating-point. |
| `M_TYPE_ENUMERATION` | Specifies that the feature's value is an enumeration. |
| `M_TYPE_INT64` | Specifies that the feature's value is a 64-bit integer. |
| `M_TYPE_REGISTER` | Specifies that the feature is mapped to a multi-byte register. |
| `M_TYPE_STRING` | Specifies that the feature's value is a string. |

### Combination Constants — To inquire the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

> **Board availability:** GenTL, V4L2

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0"). Note that, when used with[`M_FEATURE_VALUE`](../../Reference/buf/MbufInquireFeature.md), you can only use this combination value if the feature is of type string.
