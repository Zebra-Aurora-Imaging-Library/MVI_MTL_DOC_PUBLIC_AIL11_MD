---
doctype: Reference
module: ocr
function: MocrRestoreFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrRestoreFont"
---

# MocrRestoreFont

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

> Restore an OCR font context, font constraint data, or control data from file.

## Syntax

```c
AIL_ID MocrRestoreFont(
    AIL_CONST_TEXT_PTR FileName,            //in
    AIL_INT64          Operation,           //in
    AIL_ID             SysId,               //in
    AIL_ID *           FontContextOcrIdPtr  //in-out
)
```

## Description

This function restores an OCR font context that was previously saved to a file using [`MocrSaveFont`](../../Reference/ocr/MocrSaveFont.md).

This function can also restore a font context from a predefined font context file. Predefined context files are typically distributed with Aurora Imaging Library and can be found in the installed Contexts folder (for example, _C:\Program Files\Aurora Imaging Library\11\Contexts\_ _Semi_M12-92.mfo_). Alternatively, this function can load only font constraint or control data from the file into the specified OCR font context.

When an OCR font context is no longer required, it should be freed with [`MocrFree`](../../Reference/ocr/MocrFree.md).

After restoring the OCR font context, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the OCR font context identifier returned is not [`M_NULL`](../../Reference/ocr/MocrRestoreFont.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/ocr/MocrRestoreFont.md) was specified).

When the restored OCR font context is no longer required, release it using[`MocrFree`](../../Reference/ocr/MocrFree.md)unless [`M_UNIQUE_ID`](../../Reference/ocr/MocrRestoreFont.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/ocr/MocrRestoreFont.md) was specified, the smart identifier manages the restored OCR font context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the path and name of the file from which to restore the Aurora Imaging Library OCR font context (for example, _"C:\mydirectory\myfile"_). The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"Filename.mfo"` | Specifies to restore an OCR user-defined font. |
| `"MICR_CMC_7.mfo"` | Specifies to restore an OCR font context typically used with the MICR CMC7 standard. |
| `"MICR_E_13B.mfo"` | Specifies to restore an OCR font context typically used with the MICR E-13b standard. |
| `"OCR_A_BT.mfo"` | Specifies to restore an OCR font context that uses the OCR-A BT font. |
| `"OCR_B_BT.mfo"` | Specifies to restore an OCR font context that uses the OCR-B BT font. |
| `"SEMI_M12-92.mfo"` | Specifies to restore an OCR font context following the SEMI M12 standard. |
| `"SEMI_M13-88.mfo"` | Specifies to restore an OCR font context following the SEMI M13 standard. |
| `"SEMI.mfo"` | Specifies to restore an OCR font context typically used with the SEMI conductor standard. |

### `Operation` *(in, AIL_INT64)*

Specifies which type of OCR font context data to restore. It can be set to one of the following values:

*For specifying the type of data to restore*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_RESTORE`](../../Reference/ocr/MocrRestoreFont.md). |
| `M_LOAD_CONSTRAINT` | Loads only font constraint data into the specified OCR font context. |
| `M_LOAD_CONTROL` | Loads only font control data into the specified OCR font context. |
| `M_RESTORE` | Allocates a new OCR font context on the specified system, and initializes it with the font, control, and constraint data, restored from file. |

### `SysId` *(in, AIL_ID)*

Specifies the system on which the OCR font context is allocated.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `FontContextOcrIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write the OCR font context identifier or specifies the data type that the function should use to return the OCR font context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the restored OCR font context (if [`M_RESTORE`](../../Reference/ocr/MocrRestoreFont.md)was specified); in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the OCR font context (if [`M_RESTORE`](../../Reference/ocr/MocrRestoreFont.md)was specified); in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_OCR_ID_is returned instead of a standard Aurora Imaging Library identifier.

> **Note:** This setting is only available when using C++11 (or later).

An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the OCR font context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of the Aurora Imaging Library identifier the OCR font context in which to store the loaded data (if [`M_LOAD_CONSTRAINT`](../../Reference/ocr/MocrRestoreFont.md) or [`M_LOAD_CONTROL`](../../Reference/ocr/MocrRestoreFont.md)was specified).

If allocation fails, [`M_NULL`](../../Reference/ocr/MocrRestoreFont.md) is written as the identifier. |
| `Address of the OCR font context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored OCR font context (if [`M_RESTORE`](../../Reference/ocr/MocrRestoreFont.md)was specified). |

## Return Value

**Type:** `AIL_ID`

The returned value is the OCR font context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_OCR_ID_). If restoration fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/ocr/MocrRestoreFont.md) was specified).
