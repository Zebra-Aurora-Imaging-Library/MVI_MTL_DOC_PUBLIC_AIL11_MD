---
doctype: Reference
module: ocr
function: MocrHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrHookFunction"
---

# MocrHookFunction

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

> Hook a function to an event.

## Syntax

```c
AIL_OCR_HOOK_FUNCTION_PTR MocrHookFunction(
    AIL_ID                    FontContextOcrId,  //in
    AIL_INT                   HookType,          //in
    AIL_OCR_HOOK_FUNCTION_PTR HookHandlerPtr,    //in
    void *                    UserDataPtr        //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to an event when the specified font is used. A type of event to which a user-defined function can be hooked is string validation. This would immediately follow a read or verify operation. When this type of event occurs, the [`MocrReadString`](../../Reference/ocr/MocrReadString.md) or [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md) function will call the hooked function one or many times during the operation to validate each string after it is read but before being written to the OCR result buffer. This function allows you to impose global string constraints, and can be used to implement custom checksum functions or to reject strings that would have otherwise met the character constraints imposed.

Note that a function hooked to an event executes on a distinct thread. This permits the functions to run asynchronously from the operation that fired the event and from functions hooked to another event.

Hooked functions should not take longer to execute than the period in which two of their associated events can occur. You cannot determine the instance of the event that fired the function, and even if this were possible, this information would generally not be very useful. Typically, a hooked function performs the minimum number of operations required and, if necessary, performs longer processes by launching other threads.

## Parameters

### `FontContextOcrId` *(in, AIL_ID)*

Specifies the identifier of the font.

### `HookType` *(in, AIL_INT)*

Specifies the event type. This parameter can be set to the following value:

*For specifying the event type*

| Value | Description |
| --- | --- |
| `M_STRING_VALIDATION` | Specifies the character string to be validated. |

*For the HookType parameter to unhook the function*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/ocr/MocrHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_OCR_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the event occurs.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/ocr/MocrHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if the [`UserDataPtr`](../../Reference/ocr/MocrHookFunction.md) is not required.

## Return Value

**Type:** `AIL_OCR_HOOK_FUNCTION_PTR`

This function returns a _NULL_ pointer.

## Remarks

> Note that the prototype of this function is kept for backwards-compatibility.
