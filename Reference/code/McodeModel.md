---
doctype: Reference
module: code
function: McodeModel
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeModel"
---

# McodeModel

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

> Add, find, or delete a code model within a code context.

## Syntax

```c
AIL_ID McodeModel(
    AIL_ID    ContextCodeId,  //in
    AIL_INT64 Operation,      //in
    AIL_INT64 CodeType,       //in
    AIL_INT   Instance,       //in
    AIL_INT64 ControlFlag,    //in
    AIL_ID *  ModelCodeIdPtr  //in-out
)
```

## Description

This function adds a code model to a code context, finds a code model of a particular type in a code context, or deletes a code model from a code context.

Specify the code type of the code model when adding the model. A code context can contain multiple code models of 1D code types (excluding [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_4_STATE`](../../Reference/code/McodeModel.md)); for other code types, a code context can contain at most one code model. Although a code context can contain only one 2D code model, you can specify to perform a read, grade, or train operation on more than one occurrence of an [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) or [`M_DOTCODE`](../../Reference/code/McodeModel.md) code model (or any 1D code model that is not an [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_4_STATE`](../../Reference/code/McodeModel.md) code model), using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_NUMBER`](../../Reference/code/McodeControl.md).

Instead of individually adding models and specifying their code type, you can automatically detect the code type and encoding scheme of most 1D code occurrences in an image using [`McodeDetect`](../../Reference/code/McodeDetect.md), and automatically add a code model for each type found. To do so, set the [`Operation`](../../Reference/code/McodeModel.md)parameter to [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md), and pass the code detect result buffer to the [`ControlFlag`](../../Reference/code/McodeModel.md) parameter. This first deletes all existing code models in the code context, and then adds a code model for each detected code type. If two code occurrences with the same code type but different encoding schemes are detected, only one code model of this type is added and its encoding scheme is set to [`M_ANY`](../../Reference/code/McodeModel.md) (if supported by the code type).

Use [`McodeControl`](../../Reference/code/McodeControl.md) and [`McodeInquire`](../../Reference/code/McodeInquire.md) to configure or retrieve information about a code model or code context.

When you add a code model, it is assigned a code model identifier. To control or inquire about the code model, or to use the code model to write a symbol, you need its code model identifier. To retrieve the identifier of an existing code model in a code context (for example, in a restored code context), you can perform an [`M_FIND`](../../Reference/code/McodeModel.md) operation in one of two ways. If you know the code type, you can specify this and the instance of the code model of this type in the code context. Alternatively, you can pass [`M_ANY`](../../Reference/code/McodeModel.md) as the code type and specify the index of the code model in the code context. Aside from being associated with a code model identifier, a code model is given a sequential index number within the code context, in the order that the code model was added and starting from 0. If a code model is deleted, all code models with higher indices are shifted down one.

The added code models cannot be individually freed using [`McodeFree`](../../Reference/code/McodeFree.md). They are either automatically freed when their context is freed, or when they are removed from the context by calling [`McodeModel`](../../Reference/code/McodeModel.md) with [`M_DELETE`](../../Reference/code/McodeModel.md).

## Parameters

### `ContextCodeId` *(in, AIL_ID)*

Specifies the code context in which to add, find, or delete the code model. The code context must have been previously allocated on the required system using [`McodeAlloc`](../../Reference/code/McodeAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform within the specified code context. This parameter must be set to one of the following values:

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_ADD` | Adds a new code model to the code context. Specify the code type of the code model using the [`CodeType`](../../Reference/code/McodeModel.md) parameter. |
| `M_ADD_IF_NOT_THERE` | Adds a new code model to the code context if its type is not already present in the context. Specify the type of the code model using the [`CodeType`](../../Reference/code/McodeModel.md) parameter. In case a code model of the same type is already present, its Aurora Imaging Library identifier is returned. |
| `M_DELETE` | Deletes a code model within the code context, whose model identifier is specified by the [`ContextCodeId`](../../Reference/code/McodeModel.md) parameter.

For an [`M_DELETE`](../../Reference/code/McodeModel.md) operation, the [`CodeType`](../../Reference/code/McodeModel.md) and [`Instance`](../../Reference/code/McodeModel.md) parameters must be passed [`M_NULL`](../../Reference/code/McodeModel.md). |
| `M_DELETE_ALL` | Deletes all code models from the context. For this operation, the [`CodeType`](../../Reference/code/McodeModel.md) and [`Instance`](../../Reference/code/McodeModel.md) parameters must be passed[`M_NULL`](../../Reference/code/McodeModel.md). |
| `M_FIND` | Finds a code model within the code context, whose code type is specified by the [`CodeType`](../../Reference/code/McodeModel.md) parameter. If there are many code models having the same specified code type, you must specify which instance of the specified type using the [`Instance`](../../Reference/code/McodeModel.md) parameter. |
| `M_RESET_FROM_DETECTED_RESULTS` | Resets the code context and adds new code models to the context based on the results of an [`McodeDetect`](../../Reference/code/McodeDetect.md)operation. A code model is added for each code type that was detected by the [`McodeDetect`](../../Reference/code/McodeDetect.md) operation; if it detected two code occurrences with the same code type but different encoding schemes, only one code model of this type is added and its encoding scheme is set to [`M_ANY`](../../Reference/code/McodeModel.md) (if supported by the code type). All existing code models in the context are deleted.

When resetting the code context, [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) is set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeControl.md). For all code models added, [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) is set to [`M_HIGH`](../../Reference/code/McodeControl.md), and [`M_CODE_SEARCH_MODE`](../../Reference/code/McodeControl.md) is set to [`M_BEST`](../../Reference/code/McodeControl.md).

You must pass the code detect result buffer to the [`ControlFlag`](../../Reference/code/McodeModel.md) parameter. |

*For inquiring whether a code type can be added to the code context*

| Value | Description |
| --- | --- |
| `M_SUPPORTED` | Inquires whether the code type can be added to the code context, given the code type(s) that already exists in the context. |

### `CodeType` *(in, AIL_INT64)*

Specifies the code type of the code model to add or find in the code context. Note that if the [`Operation`](../../Reference/code/McodeModel.md) parameter is set to [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md) or [`M_DELETE`](../../Reference/code/McodeModel.md), set this parameter to `M_NULL`. Possible values for [`CodeType`](../../Reference/code/McodeModel.md) are listed below.

*For finding code models of any type*

| Value | Description |
| --- | --- |
| `M_ANY` | Matches all code types. This value can only be used when the [`Operation`](../../Reference/code/McodeModel.md) parameter is set to [`M_FIND`](../../Reference/code/McodeModel.md). |

*For specifying a 1D code type*

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type.

[`M_INTERLEAVED25`](../../Reference/code/McodeModel.md) is best used in a fixed-length application, with all reading equipment programmed to accept messages only of the correct length. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |

*For specifying a 2D code type*

| Value | Description |
| --- | --- |
| `M_AZTEC` | Specifies an Aztec code type. This is a matrix code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. This is a matrix code type.

Note that this code type includes the Extended Rectangular Data Matrix (DMRE) type. |
| `M_DOTCODE` | Specifies a DotCode code type. This is a matrix code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. This is a matrix code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. This is a cross-row code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. This is a matrix code type. |
| `M_PDF417` | Specifies a PDF417 code type. This is a cross-row code type. |
| `M_QRCODE` | Specifies a QR code type. This is a matrix code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. This is a cross-row code type. |

*For specifying a composite code type*

| Value | Description |
| --- | --- |
| `M_COMPOSITECODE` | Specifies a composite code type. The code type is a composite of a 1D and a 2D code type; the particular combination of code types is determined by the specified encoding scheme. |

### `Instance` *(in, AIL_INT)*

Specifies the instance or the index of the code model, depending on the operation and whether a specific code type is specified. Note that if the [`Operation`](../../Reference/code/McodeModel.md) parameter is set to [`M_ADD`](../../Reference/code/McodeModel.md) or [`M_DELETE`](../../Reference/code/McodeModel.md), set this parameter to `M_NULL`.

*For specifying the instance of a code model*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to add a code model for each code type in the code detect result buffer, when performing an [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md)operation. |
| `Value >= 0` | Specifies the instance or index of the individual code model on which to perform the operation when performing an [`M_FIND`](../../Reference/code/McodeModel.md)operation. When performing an [`M_FIND`](../../Reference/code/McodeModel.md)operation and a specific code type is specified using the [`CodeType`](../../Reference/code/McodeModel.md) parameter, the [`Instance`](../../Reference/code/McodeModel.md) parameter must indicate the instance, starting at zero, of that particular code type. If [`CodeType`](../../Reference/code/McodeModel.md) is set to [`M_ANY`](../../Reference/code/McodeModel.md), the [`Instance`](../../Reference/code/McodeModel.md) parameter must specfy the index of the code model. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies additional information required to perform the operation. When not performing an[`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md) operation, set this parameter to [`M_DEFAULT`](../../Reference/code/McodeModel.md).

*For specifying additional information required to perform the operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies no additional information is required. |
| `DetectResultCodeId` | Specifies the identifier of the code detect result buffer to use to add models, when performing an [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md) operation. The code detect result buffer must have been previously allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_DETECT_RESULT`](../../Reference/code/McodeAllocResult.md) and store the results of an [`McodeDetect`](../../Reference/code/McodeDetect.md) operation. |

### `ModelCodeIdPtr` *(in-out, *AIL_ID)*

Specifies the address in which to return or from which to read the code model identifier, depending on operation. For [`M_ADD`](../../Reference/code/McodeModel.md), [`M_ADD_IF_NOT_THERE`](../../Reference/code/McodeModel.md), [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md), and [`M_FIND`](../../Reference/code/McodeModel.md) operations, you can set this parameter to [`M_NULL`](../../Reference/code/McodeModel.md), since this function also returns the code model identifier.

*For specifying/retrieving the code model identifier*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to ignore this parameter. |
| `Address containing code model identifer to delete` | Specifies the address containing the identifier of the code model to delete from the context, when performing an [`M_DELETE`](../../Reference/code/McodeModel.md) operation. |
| `Address in which to write the identifier of code model added` | Specifies the address in which to write the identifier of the code model added, when performing an [`M_ADD`](../../Reference/code/McodeModel.md), [`M_ADD_IF_NOT_THERE`](../../Reference/code/McodeModel.md), or [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md) operation. In the latter case, if multiple code models are added, only the identifier of the first code model is returned; to retrieve the identifier of the other code models, perform an [`M_FIND`](../../Reference/code/McodeModel.md)operation for each additional code model, specifying its index and an [`M_ANY`](../../Reference/code/McodeModel.md) code type. |
| `Address in which to write the identifier of code model found` | Specifies address in which to write the identifier of the code model found, when performing an [`M_FIND`](../../Reference/code/McodeModel.md)operation. |

## Return Value

**Type:** `AIL_ID`

The returned value is the identifier of the (first) code model for a successful [`M_ADD`](../../Reference/code/McodeModel.md), [`M_ADD_IF_NOT_THERE`](../../Reference/code/McodeModel.md), [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md), or [`M_FIND`](../../Reference/code/McodeModel.md) operation; if the model could not be added/found, `M_NULL` is returned, unless performing an [`M_ADD_IF_NOT_THERE`](../../Reference/code/McodeModel.md) operation. In the latter case, if the model already exists, the Aurora Imaging Library identifier of the existing model will be returned. The returned value is [`M_NULL`](../../Reference/code/McodeModel.md) for an [`M_DELETE`](../../Reference/code/McodeModel.md) or an[`M_DELETE_ALL`](../../Reference/code/McodeModel.md) operation.
