---
doctype: Reference
module: func
function: MfuncControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / func / MfuncControl"
---

# MfuncControl

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

> Control a user-defined Aurora Imaging Library function context.

## Syntax

```c
void MfuncControl(
    AIL_ID     ContextFuncId,  //out
    AIL_INT64  ControlType,    //in
    AIL_DOUBLE ControlValue    //in
)
```

## Description

This function allows you to control the specified user-defined Aurora Imaging Library function context.

## Parameters

### `ContextFuncId` *(out, AIL_ID)*

Specifies the identifier of the user-defined Aurora Imaging Library function context. Depending on the control type, the context must have been allocated using a specific Aurora Imaging Library allocation function.

### `ControlType` *(in, AIL_INT64)*

Specifies the user-defined Aurora Imaging Library function context setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For script-based user-defined Aurora Imaging Library functions

The following [`ControlType`](../../Reference/func/MfuncControl.md) and [`ControlValue`](../../Reference/func/MfuncControl.md) parameter settings can be specified when the function context being controlled is a script-based user-defined Aurora Imaging Library function context allocated using [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md).

---

### `M_ADD_SCRIPT_REFERENCE`

Adds the specified DLL to the list of DLLs that are referred to during runtime.  > **Note:** This control type is not relevant for Python.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the path and file name of the DLL your script/function requires to run. |

---

### `M_COMPILE`

Sets when the script is reloaded from disk and recompiled.

| Value | Description |
| --- | --- |
| `M_MODIFIED` *(default)* | Specifies that the script is recompiled only if the file on disk has changed. |
| `M_ONCE` | Specifies that the script is only compiled once. |

---

### `M_DEBUG_INFORMATION`

Sets if the interpreter interface DLL must generate debug information for the script.  > **Note:** This control type is not relevant for Python.

| Value | Description |
| --- | --- |
| `M_NO` *(default)* | Specifies that the script is compiled in release mode, using optimization flags. |
| `M_YES` | Specifies that the script is compiled in debug mode, and that the necessary debug information is generated. When choosing this control value, you must specifiy the directory in which to save the debug information using the [`M_DEBUG_INFORMATION_PATH`](../../Reference/func/MfuncControl.md) control type. |

---

### `M_DEBUG_INFORMATION_PATH`

Sets the path of the directory in which the compiled version of the script and other files are created, if they are relevant for the scripting language.  > **Note:** This control type is not relevant for Python.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the path of the directory in which the debug information will be stored. This is also the directory in which the compiled version of the script is created.  > **Note:** The compiled version of the script (assembly file) will only be created in the specified directory if you choose to generate debug information using [`M_DEBUG_INFORMATION`](../../Reference/func/MfuncControl.md). |
