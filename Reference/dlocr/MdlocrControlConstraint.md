---
doctype: Reference
module: dlocr
function: MdlocrControlConstraint
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrControlConstraint"
---

# MdlocrControlConstraint

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

> Control a positional constraint of a Deep Learning OCR string model.

## Syntax

```c
void MdlocrControlConstraint(
    AIL_ID       ContextDlocrId,           //out
    AIL_INT64    StringModelLabelOrIndex,  //in
    AIL_INT64    PositionInString,         //in
    AIL_INT64    ControlType,              //in
    AIL_DOUBLE   ControlValue,             //in
    const void * ControlValuePtr           //in
)
```

## Description

This function allows you to control a positional constraint of a Deep Learning OCR string model. Constraints restrict the characters permitted to be read. The Deep Learning OCR module will only read strings that follow the positional constraints. This is useful, for example, when reading serial numbers that all have the same character at a given position. Setting positional constraints will also improve performance when characters are visually similar (for example, the numeral "0" and the uppercase letter "O").

You must preprocess the Deep Learning OCR context after modifying its positional constraints and before calling [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md). To know if a context needs to be preprocessed, call [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_PREPROCESSED`](../../Reference/dlocr/MdlocrInquire.md).

## Parameters

### `ContextDlocrId` *(out, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context that contains the string model to control. The context must have been previously allocated on the system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md).

### `StringModelLabelOrIndex` *(in, AIL_INT64)*

Specifies the string model to control. Set this parameter to one of the values below:

*For specifying the string model*

| Value | Description |
| --- | --- |
| `M_STRING_INDEX` | Specifies to control the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies to control the string model by indicating its label. |

### `PositionInString` *(in, AIL_INT64)*

Specifies the position of the character in the string model. Set this parameter to one of the values below:

*For specifying the position in a string model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies all positions that have not been manually constrained. |
| `Value >= 0` | Specifies the position in the string model. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the setting.

### `ControlValuePtr` *(in, *void)*

Specifies the address which contains more information about the setting's new value.

## Parameter Associations

### For controlling positional constraints in a string model

The following [`ControlType`](../../Reference/dlocr/MdlocrControlConstraint.md) and corresponding [`ControlValue`](../../Reference/dlocr/MdlocrControlConstraint.md) and [`ControlValuePtr`](../../Reference/dlocr/MdlocrControlConstraint.md) parameter settings are used to control positional constraint of a string model. Unless otherwise specified, set the [`StringModelLabelOrIndex`](../../Reference/dlocr/MdlocrControlConstraint.md) parameter to the label or index of a string model, set the [`PositionInString`](../../Reference/dlocr/MdlocrControlConstraint.md)parameter to the position that you want to constrain, and set the [`ControlValuePtr`](../../Reference/dlocr/MdlocrControlConstraint.md) parameter to [`M_NULL`](../../Reference/dlocr/MdlocrControlConstraint.md).

---

### `M_CONSTRAINT_TYPE`

Sets the type of constraint applied to the position.  > **Note:** Note, the list of characters which the following character types correspond to is influenced by [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md). For example if the character "4" is removed from the list of [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md), then [`M_DIGITS`](../../Reference/dlocr/MdlocrControlConstraint.md) will specify "012356789" rather than "0123456789". If all characters from the specified [`M_CONSTRAINT_TYPE`](../../Reference/dlocr/MdlocrControlConstraint.md) are removed from [`M_ACCEPTED_CHARS`](../../Reference/dlocr/MdlocrControl.md), an error is generated.

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies any character. |
| `M_CHAR_LIST` | Specifies any character in the list passed to the [`ControlValuePtr`](../../Reference/dlocr/MdlocrControlConstraint.md) parameter. |
| `M_DIGITS` | Specifies any digit. |
| `M_LETTERS` | Specifies any letter. |
| `M_LETTERS_LOWERCASE` | Specifies any lowercase letter. |
| `M_LETTERS_UPPERCASE` | Specifies any uppercase letter. |
| `M_SAME_AS_DEFAULT_CONSTRAINT` *(default)* | Specifies to use the default constraint for the model using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) with [`PositionInString`](../../Reference/dlocr/MdlocrControlConstraint.md) set to [`M_DEFAULT`](../../Reference/dlocr/MdlocrControlConstraint.md) and [`M_CONSTRAINT_TYPE`](../../Reference/dlocr/MdlocrControlConstraint.md) set to the required constraint. |

---

### `M_IS_OPTIONAL`

Sets whether a position in a string model is optional. You should mark a model position as optional if you do not require a character to be returned for that position in the model. If the string model has a minimum string size ([`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md)) less than its maximum size ([`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md)), optional positions can be skipped if their constraints cannot be met (up to a maximum of [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md)-[`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md) skips).  For example, if you want to read 10-digit telephone numbers in the formats '###-###-####' and '##########', you can set the minimum string size to 10, set the maximum string size to 12, and mark positions 3 and 7 as optional. Then, set the permitted character constraints to [`M_DIGITS`](../../Reference/dlocr/MdlocrControlConstraint.md) for positions 0-2, 4-6, and 8-11 and to hyphens for positions 3 and 7. If the target string is "123-456-7890", all the constraints are met, and Deep Learning OCR will return "123-456-7890". If the target string is "1234567890", positions 3 and 7 are skipped because they are optional and their constraints cannot be met, and Deep Learning OCR will return "1234567890".  Note that unless you set a model position as optional, its character constraints must be met to return the string, whereas if a character in the target image does not meet the constraints, Deep Learning OCR will ignore the character and check if the constraints are met by the next character if [`M_INTERCHAR_MAX_WIDTH`](../../Reference/dlocr/MdlocrControl.md) is set wide enough; if the distance between the previous character and the next character is less than the maximum inter-character width, the current character is ignored and the next character can be used to meet the constraints. In the example above, if you had set the model to '##########', the target string "123-456-7890" might still have been read (as "1234567890") if the maximum inter-character width was wide enough. It is always safer to include optional characters in the model and mark their positions as optional.  You can use [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dlocr/MdlocrGetResult.md) to see which positions in the model were skipped. Note that a string read with a lower number of skips will be returned as a higher priority result than a string read with a higher number of skips.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies that the model position is not optional. |
| `M_TRUE` | Specifies that the model position is optional.  > **Note:** Note, this is unavailable when [`PositionInString`](../../Reference/dlocr/MdlocrControlConstraint.md) is set to [`M_DEFAULT`](../../Reference/dlocr/MdlocrControlConstraint.md). |
