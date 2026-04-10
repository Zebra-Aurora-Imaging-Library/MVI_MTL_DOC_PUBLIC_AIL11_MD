---
doctype: Reference
module: app
function: MappGetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappGetHookInfo"
---

# MappGetHookInfo

> Get information about a hooked event.

## Syntax

```c
AIL_INT MappGetHookInfo(
    AIL_ID    ContextAppId,  //in
    AIL_ID    EventId,       //in
    AIL_INT64 InfoType,      //in
    void *    UserVarPtr     //out
)
```

## Description

This function retrieves information about the event that caused the hook-handler function to be called. This function should only be called within the scope of an application hook-handler function call (see [`MappHookFunction`](../../Reference/app/MappHookFunction.md)).

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappGetHookInfo.md). |

### `EventId` *(in, AIL_ID)*

Specifies the application event identifier received by the hook-handler function. See [`MappHookFunction`](../../Reference/app/MappHookFunction.md) for more information.

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information to get.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Note that, when getting parameter values ([`MappGetHookInfo`](../../Reference/app/MappGetHookInfo.md) with [`M_PARAM_VALUE`](../../Reference/app/MappGetHookInfo.md)), the [`UserVarPtr`](../../Reference/app/MappGetHookInfo.md) parameter should be of the same data type as value of the selected parameter.

## Parameter Associations

### For retrieving information about an

If the hook-handler function was called due to an [`M_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md), [`M_ERROR_FATAL`](../../Reference/app/MappHookFunction.md), or [`M_CALLEE_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md)event type, the [`InfoType`](../../Reference/app/MappGetHookInfo.md) parameter can be set to one of the values below.

---

### `M_CURRENT`

Retrieves the error code associated with the error event that caused the hook-handler function to be called.

---

### `M_CURRENT_SUB_1`

Retrieves the first error subcode associated with the error event that caused the hook-handler function to be called.

---

### `M_CURRENT_SUB_2`

Retrieves the second error subcode associated with the error event that caused the hook-handler function to be called.

---

### `M_CURRENT_SUB_3`

Retrieves the third error subcode associated with the error event that caused the hook-handler function to be called.

---

### `M_CURRENT_SUB_NB`

Retrieves the number of error subcodes associated with the error event that caused the hook-handler function to be called.

### For retrieving information about an

If the hook-handler function was called due to an [`M_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md), [`M_ERROR_FATAL`](../../Reference/app/MappHookFunction.md), [`M_TRACE_START`](../../Reference/app/MappHookFunction.md), or [`M_TRACE_END`](../../Reference/app/MappHookFunction.md) event type, the [`InfoType`](../../Reference/app/MappGetHookInfo.md) parameter can be set to the value below.

---

### `M_CURRENT_OPCODE`

Retrieves the function opcode associated with the error event that caused the hook-handler function to be called (for [`M_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md), [`M_ERROR_FATAL`](../../Reference/app/MappHookFunction.md), [`M_CALLEE_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md)), just started (for [`M_TRACE_START`](../../Reference/app/MappHookFunction.md)), or just ended (for [`M_TRACE_END`](../../Reference/app/MappHookFunction.md)).  > **Note:** To learn which function corresponds to each opcode, refer to _ailfunctioncode.h_.

### Combination Constants — For getting the associated message instead of the numeric code

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the associated message instead of the numeric code.

#### `M_MESSAGE`

Returns the error message instead of the error opcode, or the function name instead of the function opcode.  If the message is truncated, use [`M_MESSAGE_EXTENDED`](../../Reference/app/MappGetHookInfo.md) for the complete message.

#### `M_MESSAGE_EXTENDED`

Returns the error message instead of the error opcode, or the function name instead of the function opcode.

### Combination Constants — For getting the message size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the message's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### For retrieving information about an

If the hook-handler function was called due to an [`M_OBJECT_PUBLISH_DAIL`](../../Reference/app/MappHookFunction.md) event type, the [`InfoType`](../../Reference/app/MappGetHookInfo.md) parameter can be set to the value below.

---

### `M_OBJECT_ID`

Retrieves the identifier of the object whose publication state was modified.

### For retrieving information about an

If the hook-handler function was called due to an[`M_WEB_CLIENT_CONNECTED`](../../Reference/app/MappHookFunction.md)or[`M_WEB_CLIENT_DISCONNECTED`](../../Reference/app/MappHookFunction.md)event type, the [`InfoType`](../../Reference/app/MappGetHookInfo.md) parameter can be set to the value below.

---

### `M_WEB_CLIENT_INDEX`

Retrieves the index that uniquely identifies the instance of your Aurora Imaging Web Library client application that connected or disconnected. This index refers to the same instance of the client application, as long as that instance is connected. If the instance disconnects and then reconnects, it is assigned a new index.  > **Note:** Indices of clients are not zero- or one-based, nor are they guaranteed to be sequential.

### For retrieving information about an

If the hook-handler function was called due to an [`M_TRACE_START`](../../Reference/app/MappHookFunction.md) or [`M_TRACE_END`](../../Reference/app/MappHookFunction.md)   event type, the [`InfoType`](../../Reference/app/MappGetHookInfo.md) parameter can be set to one of the values below.

---

### `M_PARAM_NB`

Retrieves the number of parameters associated with the function called.

---

### `M_PARAM_SIZE`

Retrieves the size, in bytes, of the specified parameter. For an array parameter, the size is: `_Number of items in the array_ X _Size of the item._`

---

### `M_PARAM_TYPE_INFO`

Retrieves the data type of the specified parameter of the function called.

| Value | Description |
| --- | --- |
| `M_PARAM_TYPE_AIL_DOUBLE` | Specifies that the parameter is of type _AIL_DOUBLE_. |
| `M_PARAM_TYPE_AIL_FLOAT` | Specifies that the parameter is of type _AIL_FLOAT_. |
| `M_PARAM_TYPE_AIL_ID` | Specifies that the parameter is of type _AIL_ID_. |
| `M_PARAM_TYPE_AIL_INT` | Specifies that the parameter is of type _AIL_INT_. |
| `M_PARAM_TYPE_AIL_INT8` | Specifies that the parameter is of type _AIL_INT8_. |
| `M_PARAM_TYPE_AIL_INT16` | Specifies that the parameter is of type _AIL_INT16_. |
| `M_PARAM_TYPE_AIL_INT32` | Specifies that the parameter is of type _AIL_INT32_. |
| `M_PARAM_TYPE_AIL_INT64` | Specifies that the parameter is of type _AIL_INT64_. |
| `M_PARAM_TYPE_AIL_TEXT` | Specifies that the parameter is of type _AIL_TEXT_. This implies that the character encoding scheme of the _AIL_TEXT_ parameter is the scheme you are currently using. |
| `M_PARAM_TYPE_AIL_TEXT_ASCII` | Specifies that the parameter is of type _AIL_TEXT_ASCII_. This only appears when you are currently using a character encoding scheme other than ASCII, and the parameter you are inquiring is specifically in ASCII. |
| `M_PARAM_TYPE_AIL_TEXT_UNICODE` | Specifies that the parameter is of type _AIL_TEXT_UNICODE_. This only appears when you are currently using a character encoding scheme other than unicode, and the parameter you are inquiring is specifically in unicode. |
| `M_PARAM_TYPE_AIL_UINT` | Specifies that the parameter is of type _AIL_UINT_. |
| `M_PARAM_TYPE_AIL_UINT8` | Specifies that the parameter is of type _AIL_UINT8_. |
| `M_PARAM_TYPE_AIL_UINT16` | Specifies that the parameter is of type _AIL_UINT16_. |
| `M_PARAM_TYPE_AIL_UINT32` | Specifies that the parameter is of type _AIL_UINT32_. |
| `M_PARAM_TYPE_AIL_UINT64` | Specifies that the parameter is of type _AIL_UINT64_. |
| `M_PARAM_TYPE_ARRAY_AIL_DOUBLE` | Specifies that the parameter is an array of type _AIL_DOUBLE_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_FLOAT` | Specifies that the parameter is an array of type _AIL_FLOAT_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_ID` | Specifies that the parameter is an array of type _AIL_ID_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT` | Specifies that the parameter is an array of type _AIL_INT_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT8` | Specifies that the parameter is an array of type _AIL_INT8_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT16` | Specifies that the parameter is an array of type _AIL_INT16_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT32` | Specifies that the parameter is an array of type _AIL_INT32_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT64` | Specifies that the parameter is an array of type _AIL_INT64_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT` | Specifies that the parameter is an array of type _AIL_UINT_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT8` | Specifies that the parameter is an array of type _AIL_UINT8_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT16` | Specifies that the parameter is an array of type _AIL_UINT16_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT32` | Specifies that the parameter is an array of type _AIL_UINT32_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT64` | Specifies that the parameter is an array of type _AIL_UINT64_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_AIL_TEXT` | Specifies that the parameter is a constant of type _AIL_TEXT_. This implies that the character encoding scheme of the _AIL_TEXT_ parameter is the scheme you are currently using. |
| `M_PARAM_TYPE_CONST_AIL_TEXT_ASCII` | Specifies that the parameter is a constant of type _AIL_TEXT_ASCII_. This only appears when you are currently using a character encoding scheme other than ASCII, and the parameter you are inquiring is specifically in ASCII. |
| `M_PARAM_TYPE_CONST_AIL_TEXT_UNICODE` | Specifies that the parameter is a constant of type _AIL_TEXT_UNICODE_. This only appears when you are currently using a character encoding scheme other than unicode, and the parameter you are inquiring is specifically in unicode. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_DOUBLE` | Specifies that the parameter is a constant array of type _AIL_DOUBLE_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_FLOAT` | Specifies that the parameter is a constant array of type _AIL_FLOAT_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_ID` | Specifies that the parameter is a constant array of type _AIL_ID_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT` | Specifies that the parameter is a constant array of type _AIL_INT_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT8` | Specifies that the parameter is a constant array of type _AIL_INT8_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT16` | Specifies that the parameter is a constant array of type _AIL_INT16_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT32` | Specifies that the parameter is a constant array of type _AIL_INT32_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT64` | Specifies that the parameter is a constant array of type _AIL_INT64_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT` | Specifies that the parameter is a constant array of type _AIL_UINT_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT8` | Specifies that the parameter is a constant array of type _AIL_UINT8_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT16` | Specifies that the parameter is a constant array of type _AIL_UINT16_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT32` | Specifies that the parameter is a constant array of type _AIL_UINT32_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT64` | Specifies that the parameter is a constant array of type _AIL_UINT64_. Note that the pointer to the data stored in the array is unavailable after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_DATA_PTR` | Specifies that the parameter is a constant data pointer. Note that the pointer to the data is invalid after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_DATA_PTR` | Specifies that the parameter is a data pointer. Note that the pointer to the data is invalid after exiting the hook-handler function. For future use, copy and save the data. |
| `M_PARAM_TYPE_FILENAME` | Specifies that the parameter is of type _FILENAME_. This implies that the character encoding scheme of the _FILENAME_ parameter is the scheme you are currently using. |
| `M_PARAM_TYPE_FILENAME_ASCII` | Specifies that the parameter is of type _FILENAME_ASCII_. This only appears when you are currently using a character encoding scheme other than ASCII, and the parameter you are inquiring is specifically in ASCII. |
| `M_PARAM_TYPE_FILENAME_UNICODE` | Specifies that the parameter is of type _FILENAME_UNICODE_. This only appears when you are currently using a character encoding scheme other than unicode, and the parameter you are inquiring is specifically in unicode. |

---

### `M_PARAM_VALUE`

Retrieves the value of the specified parameter of the function called.

### Combination Constants — For specifying the parameter

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to determine the number of the parameter for which to receive the information.

| Value | Description |
| --- | --- |
| `1 <= Value <= 16` | Specifies the number of the parameter for which to retrieve the information. This value must be a positive integer, and should not exceed the number of parameters associated with the function call ([`M_PARAM_NB`](../../Reference/app/MappGetHookInfo.md)). |

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (\![`M_NULL`](../../Reference/app/MappGetHookInfo.md)) value is returned.
