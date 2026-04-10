---
doctype: Reference
module: dlocr
function: MdlocrAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrAlloc"
---

# MdlocrAlloc

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

> Allocate a Deep Learning OCR context.

## Syntax

```c
AIL_ID MdlocrAlloc(
    AIL_ID    SysId,             //in
    AIL_INT64 ContextType,       //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  ContextDlocrIdPtr  //out
)
```

## Description

This function allocates a Deep Learning OCR read context, or a Deep Learning OCR fine-tuning context on the specified system. A Deep Learning OCR read context contains information required to perform an [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) operation, which allows you to read strings of text without defining a font. A Deep Learning OCR fine-tuning context is used to perform an [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md) operation on a Deep Learning OCR read context with a dataset, allocated using [`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md). This allows you to train a predefined read context on images from your use-case.

By default, a Deep Learning OCR read context is set to read all text in an image within the default character height bounds. If you want to restrict the string that is found and read, add a string model; string models are optional. [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md) allows you to define the string model so that attributes are automatically set from a resulting string found using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md). If you only want to restrict the number of characters in the string, you can use [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md). Regardless of how you add a model, you can always constrain the characters that appear at certain positions using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) and adjust general string model settings, such as, character height and number of characters using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md).

A Deep Learning OCR context internally handles characters in Unicode.

When allocating the read context, you should specify whether to use an algorithm that favors robustness or speed should be used, depending on the complexity of your images and your processing power restrictions.

Global settings and string model-specific settings and positional constraints are all held in a Deep Learning OCR read context; use [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md), [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md), and [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) to set these up, respectively.

When the Deep Learning OCR context is no longer required, release it using[`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md)unless [`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrAlloc.md) was specified, the smart identifier manages the Deep Learning OCR context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the Deep Learning OCR context. Set this parameter to one of the values below:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of context to allocate. Set this parameter to the value below:

*For specifying the type of read context to allocate*

| Value | Description |
| --- | --- |
| `M_OCR_NET1_BALANCED_V1` | Specifies to allocate a Deep Learning OCR read context with a balanced deep learning model based on the NET1 architecture. The performance and accuracy of this context falls between [`M_OCR_NET1_FAST_V1`](../../Reference/dlocr/MdlocrAlloc.md) and [`M_OCR_NET1_EXTENDED_V1`](../../Reference/dlocr/MdlocrAlloc.md). This context should be used on average complexity images and when processing power restrictions are not too high. |
| `M_OCR_NET1_EXTENDED_V1` | Specifies to allocate a Deep Learning OCR read context with the most robust deep learning model based on the NET1 architecture. Maximum accuracy over [`M_OCR_NET1_FAST_V1`](../../Reference/dlocr/MdlocrAlloc.md) and [`M_OCR_NET1_BALANCED_V1`](../../Reference/dlocr/MdlocrAlloc.md) comes at the cost of speed. This context should be used on high complexity images when processing power restrictions are low. |
| `M_OCR_NET1_FAST_V1` | Specifies to allocate a Deep Learning OCR read context with the fastest deep learning model based on the NET1 architecture. Faster performance over [`M_OCR_NET1_EXTENDED_V1`](../../Reference/dlocr/MdlocrAlloc.md) and [`M_OCR_NET1_BALANCED_V1`](../../Reference/dlocr/MdlocrAlloc.md) comes at the cost of lower accuracy. This context should be used on simpler images and when processing power is limited. |
| `M_OCR_NET2_TINY_V1` | Specifies to allocate a Deep Learning OCR read context with the tiny deep learning model based on the NET2 architecture. This context can be fine-tuned, using [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md) with a Deep Learning OCR dataset ([`M_OCR_NET2_DATASET`](../../Reference/dlocr/MdlocrAllocDataset.md)) and a Deep Learning OCR fine-tuning context ([`M_OCR_NET2_FINETUNE_CONTEXT`](../../Reference/dlocr/MdlocrAlloc.md)). |

*For specifying the type of fine-tuning context to allocate*

| Value | Description |
| --- | --- |
| `M_OCR_NET2_FINETUNE_CONTEXT` | Specifies to allocate a Deep Learning OCR fine-tuning context. This can be used with Deep Learning OCR read contexts based on the NET2 architecture ([`M_OCR_NET2_TINY_V1`](../../Reference/dlocr/MdlocrAlloc.md)). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextDlocrIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Deep Learning OCR context identifier or specifies the data type that the function should use to return the Deep Learning OCR context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Deep Learning OCR
                              context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Deep Learning OCR
                              context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_DLOCR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Deep Learning OCR
                              context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the fine-tuning identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Deep Learning OCR fine-tuning context.

If allocation fails, [`M_NULL`](../../Reference/dlocr/MdlocrAlloc.md) is written as the identifier. |
| `Address in which to write the read context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Deep Learning OCR read context.

If allocation fails, [`M_NULL`](../../Reference/dlocr/MdlocrAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Deep Learning OCR context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_DLOCR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrAlloc.md) was specified).
