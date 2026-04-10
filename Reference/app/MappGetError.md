---
doctype: Reference
module: app
function: MappGetError
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappGetError"
---

# MappGetError

> Get error codes and related information.

## Syntax

```c
AIL_INT MappGetError(
    AIL_ID    ContextAppId,  //in
    AIL_INT64 ErrorType,     //in
    void *    ErrorPtr       //out
)
```

## Description

This function obtains current or global application error information (for example, error codes, error subcodes, error messages, error submessages, and function opcodes). This function allows you to check for errors after each Aurora Imaging Library function call or to get the first error that occurred after a series of Aurora Imaging Library function calls.

A typical use of this function is to check whether a buffer allocation call was successful ([`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md), [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md), and [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md)) or to see if a function generates an error.

In multi-thread environments, a call to [`MappGetError`](../../Reference/app/MappGetError.md) returns the last error that occurred in the current thread.

This function can also be used to obtain information about a detected error when error-reporting to the screen has been disabled using [`MappControl`](../../Reference/app/MappControl.md).

To clear errors, call [`MappControl`](../../Reference/app/MappControl.md) with [`M_CLEAR_ERROR`](../../Reference/app/MappControl.md).

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappGetError.md). |

### `ErrorType` *(in, AIL_INT64)*

Specifies the type of error information to read. This parameter can be set to one of the values below.

### `ErrorPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For specifying the type of error information to read

---

### `M_CURRENT`

Retrieves the error code returned by the last function call. The current-error code is reset to **M_NULL_ERROR** before each function call and is set to a specific error code when an error occurs during the execution of a function.

---

### `M_CURRENT_OPCODE`

Retrieves the opcode associated with the last function called, when it returns an error.  > **Note:** To learn which function corresponds to each opcode, refer to _ailfunctioncode.h_.

---

### `M_CURRENT_SUB_1`

Retrieves the first error subcode returned by the last function call.

---

### `M_CURRENT_SUB_2`

Retrieves the second error subcode returned by the last function call.

---

### `M_CURRENT_SUB_3`

Retrieves the third error subcode returned by the last function call.

---

### `M_CURRENT_SUB_NB`

Retrieves the number of error subcodes associated with the last function called, when it returns an error.

---

### `M_GLOBAL`

Retrieves the error code of the first error that has occurred since the last call to [`MappGetError`](../../Reference/app/MappGetError.md) (with [`ErrorType`](../../Reference/app/MappGetError.md) set to [`M_GLOBAL`](../../Reference/app/MappGetError.md)). This value is reset to **M_NULL_ERROR** before the next call to a function (other than [`MappGetError`](../../Reference/app/MappGetError.md)).

---

### `M_GLOBAL_OPCODE`

Retrieves the opcode associated with the first error-generating function since the last call to [`MappGetError`](../../Reference/app/MappGetError.md) (with [`ErrorType`](../../Reference/app/MappGetError.md) set to [`M_GLOBAL`](../../Reference/app/MappGetError.md)).  > **Note:** To learn which function corresponds to each opcode, refer to _ailfunctioncode.h_.

---

### `M_GLOBAL_SUB_1`

Retrieves the first error subcode of the first error that has occurred since the call to [`MappGetError`](../../Reference/app/MappGetError.md) (with [`ErrorType`](../../Reference/app/MappGetError.md) set to [`M_GLOBAL`](../../Reference/app/MappGetError.md)).

---

### `M_GLOBAL_SUB_2`

Retrieves the second error subcode of the first error that has occurred since the call to [`MappGetError`](../../Reference/app/MappGetError.md) (with [`ErrorType`](../../Reference/app/MappGetError.md) set to [`M_GLOBAL`](../../Reference/app/MappGetError.md)).

---

### `M_GLOBAL_SUB_3`

Retrieves the third error subcode of the first error that has occurred since the call to [`MappGetError`](../../Reference/app/MappGetError.md) (with [`ErrorType`](../../Reference/app/MappGetError.md) set to [`M_GLOBAL`](../../Reference/app/MappGetError.md)).

---

### `M_GLOBAL_SUB_NB`

Retrieves the number of error subcodes associated with the first error that occurred since the last call to [`MappGetError`](../../Reference/app/MappGetError.md) (with [`ErrorType`](../../Reference/app/MappGetError.md) set to [`M_GLOBAL`](../../Reference/app/MappGetError.md)).

### Combination Constants — For error types

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set whether to return a message or to make the inquire synchronous.

#### `M_MESSAGE`

Retrieves the error message associated with the specified error type, or the function name associated with a function opcode, and writes it to the address specified by the [`ErrorPtr`](../../Reference/app/MappGetError.md) parameter. The error/function opcode is still returned by [`MappGetError`](../../Reference/app/MappGetError.md), but it is not written to the address specified by [`ErrorPtr`](../../Reference/app/MappGetError.md).  If the message is truncated, use [`M_MESSAGE_EXTENDED`](../../Reference/app/MappGetError.md) for the complete message.  > **Note:** Note that this value cannot be added to [`M_CURRENT_SUB_NB`](../../Reference/app/MappGetError.md) or [`M_GLOBAL_SUB_NB`](../../Reference/app/MappGetError.md).

#### `M_MESSAGE_EXTENDED`

Retrieves the error message associated with the specified error type, or the function name associated with a function opcode, and writes it to the address specified by the [`ErrorPtr`](../../Reference/app/MappGetError.md) parameter. The error/function opcode is still returned by [`MappGetError`](../../Reference/app/MappGetError.md), but it is not written to the address specified by [`ErrorPtr`](../../Reference/app/MappGetError.md).  > **Note:** Note that this value cannot be added to [`M_CURRENT_SUB_NB`](../../Reference/app/MappGetError.md) or [`M_GLOBAL_SUB_NB`](../../Reference/app/MappGetError.md).

#### `M_SYNCHRONOUS`

Forces the function to wait for all pending calls to complete before continuing.

### Combination Constants — For getting the message size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the message's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

## Return Value

**Type:** `AIL_INT`

The returned value is the requested error/function code. If the error code is read and it is equal to `M_NULL_ERROR`, no error has occurred.

Note, when there is no error, the error subcode is set to **M_NULL_ERROR**.
