---
doctype: Reference
module: dmr
function: MdmrControlStringModel
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrControlStringModel"
---

# MdmrControlStringModel

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

> Control a global setting of a SureDotOCR string model, or control a position (constraint) in a string model.

## Syntax

```c
void MdmrControlStringModel(
    AIL_ID       ContextDmrId,             //out
    AIL_INT64    StringModelLabelOrIndex,  //in
    AIL_INT64    Position,                 //in
    AIL_INT64    ControlType,              //in
    AIL_DOUBLE   ControlValue1,            //in
    AIL_DOUBLE   ControlValue2,            //in
    const void * ControlValuePtr           //in
)
```

## Description

This function allows you to control a global setting of a SureDotOCR string model, or to control a position in a string model. This includes setting constraints for the different positions. Constraints restrict the characters permitted to be read, according to a character type and font. String models, along with their constraints, represent the dot-matrix strings to read from a target image. Initially, any character from any font can be read at every string model position.

String models are held in a SureDotOCR context. To add string models to a context, use [`MdmrControl`](../../Reference/dmr/MdmrControl.md). To inquire about string models, use [`MdmrInquireStringModel`](../../Reference/dmr/MdmrInquireStringModel.md).

You can set default constraints for all the positions in the string model, and override the constraints for specific positions. If you override the constraints for a specific position, the position is said to be explicitly constrained; otherwise, it is said to be implicitly constrained. To set the default constraints for the positions in the string model, call this function and pass [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md) to the [`Position`](../../Reference/dmr/MdmrControlStringModel.md) parameter; to set explicit constraints for a position, pass **M_POSITION_IN_STRING(n)** instead, where _n_ is the position to constrain.

If you previously constrained a position, you can further constrain it by specifying its exact position in the string model, or by indicating the order in which it was originally constrained. For example, if you explicitly constrained positions 14 and 22, you can further constrain position 22 using either **M_POSITION_IN_STRING(n)**, where _n_ is 22, or **M_POSITION_CONSTRAINED_ORDER(n)**, where _n_ is 1.

Once you have explicitly constrained a position, it is considered to be explicitly constrained until you call this function with [`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md) for that position, even if you have manually set the position's constraints back to the defaults of the string model. Note that the constrained order of a position changes if you reset previously constrained positions to be implicitly constrained ([`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md)).

For a successful read operation, you must specify an explicit value for the global string model controls that set the maximum and minimum number of characters in the string ([`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) and [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md), or [`M_STRING_SIZE_MIN_MAX`](../../Reference/dmr/MdmrControlStringModel.md)).

You must preprocess the SureDotOCR context after you have finished modifying its string models and before calling [`MdmrRead`](../../Reference/dmr/MdmrRead.md). To know if a context needs to be preprocessed, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) with [`M_PREPROCESSED`](../../Reference/dmr/MdmrInquire.md).

## Parameters

### `ContextDmrId` *(out, AIL_ID)*

Specifies the identifier of the SureDotOCR context that contains the string model to control. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `StringModelLabelOrIndex` *(in, AIL_INT64)*

Specifies the string model (one or all) to control. Set this parameter to one of the values below:

*For specifying the string model*

| Value | Description |
| --- | --- |
| `M_STRING_INDEX` | Specifies to control the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies to control the string model by indicating its label. |
| `M_ALL` | Specifies to control all string models. |

### `Position` *(in, AIL_INT64)*

Specifies how to control the string model. Set this parameter to one of the values below:

*For specifying how to control to the string model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to control a global setting of a string model. |
| `M_POSITION_CONSTRAINED_ORDER` | Specifies to control an explicitly constrained position in the string model by indicating the order in which the position was explicitly constrained. |
| `M_POSITION_IN_STRING` | Specifies to control a position in the string model. |
| `M_ALL_CONSTRAINED_POSITIONS` | Specifies to control all explicitly constrained positions in the string model. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue1` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

### `ControlValue2` *(in, AIL_DOUBLE)*

Specifies the second required value for the control.

### `ControlValuePtr` *(in, *void)*

Specifies the address which contains more information about the setting's new value.

## Parameter Associations

### For controlling a global setting of a string model

The following [`ControlType`](../../Reference/dmr/MdmrControlStringModel.md) and corresponding[`ControlValue1`](../../Reference/dmr/MdmrControlStringModel.md), [`ControlValue2`](../../Reference/dmr/MdmrControlStringModel.md), and [`ControlValuePtr`](../../Reference/dmr/MdmrControlStringModel.md) parameter settings are used to control a global setting of a string model. In this case, set the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrControlStringModel.md) parameter to the label or index of a string model (one or all, unless otherwise specified), and set the [`Position`](../../Reference/dmr/MdmrControlStringModel.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md).

---

### `M_CHAR_ACCEPTANCE`

Sets the acceptance level for the score of the string's characters. This score quantifies the similarity between the characters in the target string and the corresponding characters in the font that the string model uses.  For the string to be read, the score of every character must be greater than or equal to the specified character acceptance. To retrieve character scores, call [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_CHAR_SCORE`](../../Reference/dmr/MdmrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level for the character score, as a percentage. 100.0% indicates that the character in the target string perfectly resembles the corresponding character in the font (perfection is generally hard to obtain). |

---

### `M_STRING_ACCEPTANCE`

Sets the acceptance level for the string's score. This score quantifies the similarity between the target string and the string model. For the string to be read, its score must be greater than or equal to the specified acceptance.  The string's score is the average score of all its characters. Even if the string's score is above the string's acceptance, the string might not be read if one or more if its characters is below the character's acceptance ([`M_CHAR_ACCEPTANCE`](../../Reference/dmr/MdmrControlStringModel.md)). In this case, consider lowering the character's acceptance. To retrieve the score of the string or its characters, call [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_STRING_SCORE`](../../Reference/dmr/MdmrGetResult.md) or [`M_CHAR_SCORE`](../../Reference/dmr/MdmrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level for the string's score, as a percentage. 100.0% indicates that the string in the target image perfectly resembles the string model (perfection is generally hard to obtain). |

---

### `M_STRING_CERTAINTY`

Sets the certainty level for the string's score. This score quantifies the similarity between the target string and the string model. If the string's score is equal to or above the certainty, the string is immediately read as a valid string result, without processing the rest of the target image for strings with higher scores (provided the specified number of strings to read has been read).  The string's score is the average score of all its characters. To retrieve the score of the string or its characters, call [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_STRING_SCORE`](../../Reference/dmr/MdmrGetResult.md) or [`M_CHAR_SCORE`](../../Reference/dmr/MdmrGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level for the string score, as a percentage. 100.0% indicates that the string in the target image perfectly resembles the string model (perfection is generally hard to obtain). |

---

### `M_STRING_LABEL_VALUE`

Modifies the label value of the string model. The [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrControlStringModel.md) parameter must specify a specific string model (not all).

---

### `M_STRING_RANK`

Sets the order in which to read a string, relative to the other strings to read. Strings with lower ranks are read before those with higher ones. Do not modify rank unless your context has multiple string models.  SureDotOCR reads strings from left to right and from top to bottom, relative to the angle of the string's location. If a string model's rank is 0 ([`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md)), SureDotOCR tries to read that string first; accounting for angle, it will be the top left-most string. If multiple string models have identical ranks, SureDotOCR reads just one string, depending on which is more similar to its string model.  When specifying ranks, you must include rank 0, and you must not skip successive values. For example, you will get an error if a context has four string models with their ranks set to [1, 2, 3, 4] or [0, 1, 2, 4]. Strings with different ranks must be on separate lines.  > **Note:** SureDotOCR uses the rank of the string models in a context to establish the required number of strings to read for that context: `_TotalNumberOfStringsToRead_ = _HighestRankValue_ + 1`. For example, if you have four string models with ranks set to [0, 0, 1, 2], SureDotOCR must read three strings. If 3 strings cannot be read, no results are returned. Since the default rank for every string model is 0, SureDotOCR will try to read 1 string, regardless of how many string models are in your context, unless you modify rank.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the order in which to read a string. Explicitly indicating rank can make the read operation faster and more accurate. |

---

### `M_STRING_SIZE_MAX`

Sets the maximum number of characters in the string. This number includes spaces, if they correspond to a space permitted character constraint ([`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)).  If this number is not equal to[`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md), use [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dmr/MdmrGetResult.md) to see which characters in the model were skipped.

| Value | Description |
| --- | --- |
| `1 <= Value <= 256` | Specifies the maximum number of characters. |

---

### `M_STRING_SIZE_MIN`

Sets the minimum number of characters in the string. This number includes spaces, if they correspond to a space permitted character constraint ([`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)).  If this number is not equal to[`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md), use [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dmr/MdmrGetResult.md) to see which characters in the model were skipped.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 256` *(default)* | Specifies the minimum number of characters. |

---

### `M_STRING_SIZE_MIN_MAX`

Sets the minimum and maximum number of characters in the string. These numbers include spaces, if they correspond to a space permitted character constraint ([`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)).  If min and max are not equal, use [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dmr/MdmrGetResult.md) to see which characters in the model were skipped.  [`M_STRING_SIZE_MIN_MAX`](../../Reference/dmr/MdmrControlStringModel.md) is the same as calling this function twice, the first with [`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) and the second with [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 256` *(default)* | Specifies the minimum number of characters. |
| `1 <= Value <= 256` | Specifies the maximum number of characters. |

### For setting constraints to the different positions in the string model

The following [`ControlType`](../../Reference/dmr/MdmrControlStringModel.md) and corresponding [`ControlValue1`](../../Reference/dmr/MdmrControlStringModel.md), [`ControlValue2`](../../Reference/dmr/MdmrControlStringModel.md), and [`ControlValuePtr`](../../Reference/dmr/MdmrControlStringModel.md) parameter settings are used to set constraints for the different positions in the string model. In this case, set the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrControlStringModel.md) parameter to the label or index of a string model (one or all).  To set the default constraints for the positions in the string model, set the [`Position`](../../Reference/dmr/MdmrControlStringModel.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md), unless otherwise specified. To set explicit constraints for a position, set the [`Position`](../../Reference/dmr/MdmrControlStringModel.md)parameter to the required position (one or all). Note that positions have zero-based indexing.

---

### `M_ADD_PERMITTED_CHARS_ENTRY`

Sets a constraint based on permitted characters. This restricts the characters that can be read, according to a font ([`ControlValue1`](../../Reference/dmr/MdmrControlStringModel.md)) and character type ([`ControlValue2`](../../Reference/dmr/MdmrControlStringModel.md)). For example, you can specify that at position 0, in string model 0, you will only permit the letter 'O', as represented in font _OhZero_, to be read.  Every position in every string model can initially read any character from any font in the context, unless you specify otherwise. Spaces can also be read, depending on the space controls, which you can specify by calling [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md) and [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md).  Unless otherwise specified, permitted character constraints are cumulative. Each time you specify permitted characters, they are added to the previously specified permitted characters, if they exist.

| Value | Description |
| --- | --- |
| `M_FONT_INDEX` | Specifies the font by indicating its index, or specifies any font.  When you specify a font's index, SureDotOCR internally refers to the font by its corresponding label. If the font's index subsequently changes, which can happen when other fonts are added or deleted, the font you referred to remains the same. |
| `M_FONT_LABEL` | Specifies the font by indicating its label, or specifies any font. |
| `M_ANY` | Specifies to read all characters present in the font. |
| `M_CHAR_LIST` | Specifies to read an explicit list of characters. To specify them, use the [`ControlValuePtr`](../../Reference/dmr/MdmrControlStringModel.md) parameter. |
| `M_DIGITS` | Specifies to read characters '0' to '9'. |
| `M_LETTERS` | Specifies to read characters 'A' to 'Z' and 'a' to 'z'. |
| `M_LETTERS_LOWERCASE` | Specifies to read characters 'a' to 'z'. |
| `M_LETTERS_UPPERCASE` | Specifies to read characters 'A' to 'Z'. |
| `M_SPACE` | Specifies to read a space.  Unlike other permitted characters, fonts cannot represent a space. To establish it, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md) and [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md). When specifying [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md), set the [`ControlValue1`](../../Reference/dmr/MdmrControlStringModel.md) parameter to the index or label of any font (**M_FONT_INDEX()**).  To use [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md), the [`Position`](../../Reference/dmr/MdmrControlStringModel.md) parameter must be set to a specific position that is not the first or last position in a string model. That position cannot have another character constraint nor can you have consecutive positions with a space constraint. If these conditions are not followed, you will get an error.  Reading an [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md) character does not necessarily mean that the corresponding space position in the target string is blank. SureDotOCR attempts to read the best string possible, and can use the space constraint as an area to ignore, even if that area is not empty. For example, if you have an icon at a specific position within a target string, you can specify a space at that position. |

### For generally controlling the different positions in the string model

The following [`ControlType`](../../Reference/dmr/MdmrControlStringModel.md) and corresponding [`ControlValue1`](../../Reference/dmr/MdmrControlStringModel.md), [`ControlValue2`](../../Reference/dmr/MdmrControlStringModel.md), and [`ControlValuePtr`](../../Reference/dmr/MdmrControlStringModel.md) parameter settings are used to generally control the different positions in the string model(s). In this case, set the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrControlStringModel.md) parameter to the label or index of a string model (one or all).  Depending on the control type selected, you can control the default constraints for the positions in the string model (set the [`Position`](../../Reference/dmr/MdmrControlStringModel.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md)), or you can control the explicit constraints for a position (set the [`Position`](../../Reference/dmr/MdmrControlStringModel.md)parameter to one or all positions). Note that positions have zero-based indexing.

---

### `M_CLONE_CONSTRAINTS_FROM`

Clones the explicit constraints from the source to the destination. The string model position specified with the [`ControlValue1`](../../Reference/dmr/MdmrControlStringModel.md) and [`ControlValue2`](../../Reference/dmr/MdmrControlStringModel.md) parameters represents the source. The string model position specified with the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrControlStringModel.md) and [`Position`](../../Reference/dmr/MdmrControlStringModel.md) parameters represents the destination.  If the destination is a specific position (one or all), it will be explicitly constrained according to the explicit constraints of the source. Explicit constraints already specified for the destination will be overwritten. Only the cloned ones will remain.  If the destination refers to the default constraints for the positions in the string model, all its implicitly constrained positions will be identical to the explicitly constrained position identified by the source. The clone operation does not override the state of the positions in the source; they remain implicitly constrained.

| Value | Description |
| --- | --- |
| `M_STRING_INDEX` | Specifies the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies the string model by indicating its label. |
| `M_POSITION_CONSTRAINED_ORDER` | Specifies an explicitly constrained position in the string model by indicating the order in which the position was explicitly constrained. |
| `M_POSITION_IN_STRING` | Specifies a position in the string model. |

---

### `M_DELETE_PERMITTED_CHARS_ENTRY`

Deletes a character constraint at a specific index.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to delete all character constraints. The permitted character constraints revert to those of the default constraint. |
| `0<=Value<M_NUMBER_OF_PERMITTED_CHARS_ENTRIES` | Specifies to delete the character constraint at the specified index. |

---

### `M_IS_OPTIONAL`

Sets whether a position in a string model is optional. You should mark a model position as optional if you do not require a character to be returned for that position in the model. If the string model has a minimum string size ([`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md)) less than its maximum size ([`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md)), optional positions can be skipped if their constraints cannot be met (up to a maximum of [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md)-[`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) skips).  For example, if you want to read 10-digit telephone numbers in the formats '###-###-####' and '##########', you can set the minimum string size to 10, set the maximum string size to 12, and mark positions 3 and 7 as optional. Then, set the permitted character constraints to [`M_DIGITS`](../../Reference/dmr/MdmrControlStringModel.md) for positions 0-2, 4-6, and 8-11 and to hyphens for positions 3 and 7. If the target string is "123-456-7890", all the constraints are met, and SureDotOCR will return "123-456-7890". If the target string is "1234567890", positions 3 and 7 are skipped because they are optional and their constraints cannot be met, and SureDotOCR will return "1234567890".  Note that unless you set a model position as optional, its character constraints must be met to return the string.  Regardless of whether a model position is optional, if the target image does not meet the constraints and [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md) is set wide enough, SureDotOCR will ignore the character and check if the constraints are met by the next character. If the distance between the previous character and the next character is less than the minimum space width, the current character is ignored and the next character can be used to meet the constraints. In the example above, if you had set the model to '##########', the target string "123-456-7890" might still have been read (as "1234567890") if the minimum space width was wide enough. It is always safer to include optional characters in the model and mark their positions as optional.  You can use [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md) with [`M_SKIPPED_POSITIONS`](../../Reference/dmr/MdmrGetResult.md) to see which positions in the model were skipped. Note that a string read with a lower number of skips will be returned as a higher priority result than a string read with a higher number of skips.  Note that [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md) will not accept optional constraints if [`M_STRING_PARTIAL_MODE`](../../Reference/dmr/MdmrControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies that the model position is not optional. |
| `M_TRUE` | Specifies that the model position is optional. |

---

### `M_RESET_IMPLICIT_CONSTRAINTS`

Resets the implicit (default) constraints for the positions in the string model back to their initial value. You can only use [`M_RESET_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md) if the [`Position`](../../Reference/dmr/MdmrControlStringModel.md)parameter is set to [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md).  Initially, any character from any font can be read at every string model position.

---

### `M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`

Resets the explicitly constrained position(s) to the implicit (default) constraints for the string model. These positions are then considered implicitly constrained. You can only use [`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md) if the [`Position`](../../Reference/dmr/MdmrControlStringModel.md)parameter is not set to [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md).  The constrained order (**M_POSITION_CONSTRAINED_ORDER()**) of a position changes when you use [`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md).
