---
doctype: Reference
module: func
function: MfuncInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncInquire"
---

# MfuncInquire

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

> Retrieve information about an Aurora Imaging Library object.

## Syntax

```c
AIL_INT MfuncInquire(
    AIL_ID    AilObjectId,  //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function retrieves information about the specified Aurora Imaging Library object.

## Parameters

### `AilObjectId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library object. Depending on the inquire type, this object must have been allocated using a specific Aurora Imaging Library allocation function.

### `InquireType` *(in, AIL_INT64)*

Specifies the Aurora Imaging Library object feature about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MfuncInquire`](../../Reference/func/MfuncInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For different Aurora Imaging Library object types

The following [`InquireType`](../../Reference/func/MfuncInquire.md) parameter settings can be specified for different Aurora Imaging Library object types.

---

### `M_BUFFER_INFO`

Inquires the handle of the Aurora Imaging Library buffer. This buffer must have been allocated using one of the [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md) functions.

---

### `M_OBJECT_PTR`

Inquires the address of the user-defined Aurora Imaging Library object allocated using [`MfuncAllocId`](../../Reference/func/MfuncAllocId.md).

### For user-defined Aurora Imaging Library functions

The following [`InquireType`](../../Reference/func/MfuncInquire.md) parameter settings can be specified when the object passed to [`AilObjectId`](../../Reference/func/MfuncInquire.md) is a user-defined Aurora Imaging Library function context allocated using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) or [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md).

---

### `M_CALLER_BITNESS`

Inquires the bitness of the operating system on which the function context was created.

| Value | Description |
| --- | --- |
| `32` | Specifies a 32-bit platform. |
| `64` | Specifies a 64-bit platform. |

---

### `M_PARAM_NUMBER`

Inquires the number of parameters associated with the specified user-defined Aurora Imaging Library function context.

---

### `M_PARAM_SIZE`

Inquires the size, in bytes, of the specified parameter. For an array parameter, the size is: `_Number of items in the array_ X _Size of the item._`

---

### `M_PARAM_TYPE_INFO`

Inquires the data type of the specified parameter.

| Value | Description |
| --- | --- |
| `M_PARAM_TYPE_AIL_DOUBLE` | Specifies that the parameter is of type _AIL_DOUBLE_. |
| `M_PARAM_TYPE_AIL_ID` | Specifies that the parameter is of type _AIL_ID_. |
| `M_PARAM_TYPE_AIL_INT` | Specifies that the parameter is of type _AIL_INT_. |
| `M_PARAM_TYPE_AIL_INT32` | Specifies that the parameter is of type _AIL_INT32_. |
| `M_PARAM_TYPE_AIL_INT64` | Specifies that the parameter is of type _AIL_INT64_. |
| `M_PARAM_TYPE_AIL_TEXT` | Specifies that the parameter is of type _AIL_TEXT_. This implies that the character encoding scheme of the _AIL_TEXT_ parameter is the scheme you are currently using. |
| `M_PARAM_TYPE_AIL_TEXT_ASCII` | Specifies that the parameter is of type _AIL_TEXT_ASCII_. This only appears when you are currently using a character encoding scheme other than ASCII, and the parameter you are inquiring is specifically in ASCII. |
| `M_PARAM_TYPE_AIL_TEXT_UNICODE` | Specifies that the parameter is of type _AIL_TEXT_UNICODE_. This only appears when you are currently using a character encoding scheme other than unicode, and the parameter you are inquiring is specifically in unicode. |
| `M_PARAM_TYPE_AIL_UINT` | Specifies that the parameter is of type _AIL_UINT_. |
| `M_PARAM_TYPE_AIL_UINT32` | Specifies that the parameter is of type _AIL_UINT32_. |
| `M_PARAM_TYPE_AIL_UINT64` | Specifies that the parameter is of type _AIL_UINT64_. |
| `M_PARAM_TYPE_ARRAY_AIL_DOUBLE` | Specifies that the parameter is an array of type _AIL_DOUBLE_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_ID` | Specifies that the parameter is an array of type _AIL_ID_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT` | Specifies that the parameter is an array of type _AIL_INT_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT32` | Specifies that the parameter is an array of type _AIL_INT32_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_INT64` | Specifies that the parameter is an array of type _AIL_INT64_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT` | Specifies that the parameter is an array of type _AIL_UINT_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT32` | Specifies that the parameter is an array of type _AIL_UINT32_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_ARRAY_AIL_UINT64` | Specifies that the parameter is an array of type _AIL_UINT64_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_AIL_TEXT` | Specifies that the parameter is a constant of type _AIL_TEXT_. This implies that the character encoding scheme of the _AIL_TEXT_ parameter is the scheme you are currently using. |
| `M_PARAM_TYPE_CONST_AIL_TEXT_ASCII` | Specifies that the parameter is a constant of type _AIL_TEXT_ASCII_. This only appears when you are currently using a character encoding scheme other than ASCII, and the parameter you are inquiring is specifically in ASCII. |
| `M_PARAM_TYPE_CONST_AIL_TEXT_UNICODE` | Specifies that the parameter is a constant of type _AIL_TEXT_UNICODE_. This only appears when you are currently using a character encoding scheme other than unicode, and the parameter you are inquiring is specifically in unicode. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_DOUBLE` | Specifies that the parameter is a constant array of type _AIL_DOUBLE_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_ID` | Specifies that the parameter is a constant array of type _AIL_ID_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT` | Specifies that the parameter is a constant array of type _AIL_INT_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT32` | Specifies that the parameter is a constant array of type _AIL_INT32_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_INT64` | Specifies that the parameter is a constant array of type _AIL_INT64_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT` | Specifies that the parameter is a constant array of type _AIL_UINT_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT32` | Specifies that the parameter is a constant array of type _AIL_UINT32_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT64` | Specifies that the parameter is a constant array of type _AIL_UINT64_. Note that the pointer to the data stored in the array is unavailable after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_CONST_DATA_PTR` | Specifies that the parameter is a constant data pointer. Note that the pointer to the data is invalid after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_DATA_PTR` | Specifies that the parameter is a data pointer. Note that the pointer to the data is invalid after exiting the user-defined function. For future use, copy and save the data. |
| `M_PARAM_TYPE_FILENAME` | Specifies that the parameter is of type _FILENAME_. This implies that the character encoding scheme of the _FILENAME_ parameter is the scheme you are currently using. |
| `M_PARAM_TYPE_FILENAME_ASCII` | Specifies that the parameter is of type _FILENAME_ASCII_. This only appears when you are currently using a character encoding scheme other than ASCII, and the parameter you are inquiring is specifically in ASCII. |
| `M_PARAM_TYPE_FILENAME_UNICODE` | Specifies that the parameter is of type _FILENAME_UNICODE_. This only appears when you are currently using a character encoding scheme other than unicode, and the parameter you are inquiring is specifically in unicode. |

### Combination Constants — For specifying the number of the parameter for which to inquire the information

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to determine the number of the parameter for which to receive the information.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of the parameter for which to inquire the information. This value must be a positive integer, and should not exceed the number of parameters associated with the Aurora Imaging Library function context ([`M_PARAM_NUMBER`](../../Reference/func/MfuncInquire.md)). |

### For the script-based user-defined Aurora Imaging Library object feature

The following [`InquireType`](../../Reference/func/MfuncInquire.md) parameter settings can be specified when the object passed to [`AilObjectId`](../../Reference/func/MfuncInquire.md) is a script-based user-defined Aurora Imaging Library function context allocated using [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md).

---

### `M_COMPILE`

Inquires whether the script must be reloaded from disk and recompiled.

| Value | Description |
| --- | --- |
| `M_MODIFIED` *(default)* | Specifies that the script is recompiled only if the file on disk has changed. |
| `M_ONCE` | Specifies that the script is only compiled once. |

---

### `M_DEBUG_INFORMATION`

Inquires whether the interpreter interface DLL generates debug information for the script.

| Value | Description |
| --- | --- |
| `M_NO` *(default)* | Specifies that the script is compiled in release mode, using optimization flags. |
| `M_YES` | Specifies that the script is compiled in debug mode, and that the necessary debug information is generated. |

---

### `M_DEBUG_INFORMATION_PATH`

Inquires the path of the directory in which the compiled version of the script and other files are created, if they are relevant for the scripting language.

---

### `M_NUMBER_OF_SCRIPT_REFERENCES`

Inquires the number of DLLs that are included in the list of DLLs referred to during runtime.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of DLLs in the list. |

---

### `M_SCRIPT_REFERENCE`

Inquires the path of the specified DLL in the list of DLLs referred to during runtime.

### Combination Constants — For specifying the index number of the script reference to be inquired.

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the index number of the script reference to be inquired.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the index of the script reference about which to inquire. This value must be a positive integer. |

### Combination Constants — For inquiring the size of a string

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
