---
doctype: Reference
module: str
function: MstrPreprocess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrPreprocess"
---

# MstrPreprocess

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

> Preprocess a String Reader context.

## Syntax

```c
void MstrPreprocess(
    AIL_ID    ContextId,   //in
    AIL_INT64 ControlFlag  //in
)
```

## Description

This function prepares the specified String Reader context, font, and string model to be read.

This function must be called before the first call to [`MstrRead`](../../Reference/str/MstrRead.md). Changes to control settings, the String Reader context, one of its font or string models often require you to re-preprocess the context. To inquire if you need to preprocess or re-preprocess the context, use [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_PREPROCESSED`](../../Reference/str/MstrInquire.md).

In a typical application, [`MstrPreprocess`](../../Reference/str/MstrPreprocess.md) is called once. To preprocess a String Reader context without error, the context must have at least one font, one character, and one string model.

When you save a String Reader context, the preprocessing changes are not saved. Upon restoration, you must preprocess the context again.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the String Reader context to preprocess. The String Reader context must have been previously allocated on the system using [`MstrAlloc`](../../Reference/str/MstrAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to preprocess the String Reader context. Set this parameter to one of the following values:

*For specifying whether to preprocess the context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Preprocesses the String Reader context. |
| `M_RESET` | Un-preprocesses the String Reader context.

Un-preprocessing the String Reader context can be useful if you want to conserve system memory within an application and preserve String Reader context settings. |
