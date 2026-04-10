---
doctype: Reference
module: str
function: MstrSetConstraint
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrSetConstraint"
---

# MstrSetConstraint

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

> Set character constraints.

## Syntax

```c
void MstrSetConstraint(
    AIL_ID       ContextId,       //out
    AIL_INT      StringIndex,     //in
    AIL_INT      CharPos,         //in
    AIL_INT64    ConstraintType,  //in
    const void * CharListPtr      //in
)
```

## Description

This function specifies the constraint to apply to a specified character of a specified string or of all strings.

Each constraint must include at least one character existing in one of the selected fonts. For example, if the constraint is [`M_DIGIT`](../../Reference/str/MstrSetConstraint.md) then at least one of the fonts must contain a digit.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the String Reader context with which to associate the constraint. The String Reader context must have been previously allocated on the required system using [`MstrAlloc`](../../Reference/str/MstrAlloc.md).

### `StringIndex` *(in, AIL_INT)*

Specifies the string(s) in the String Reader context to affect.

*For specifying the string index*

| Value | Description |
| --- | --- |
| `M_STRING_INDEX` | Specifies that the constraint will be applied to a string or to all strings. |

### `CharPos` *(in, AIL_INT)*

Specifies the character position in the string for which to set the constraint.

*For specifying the character position*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Sets the default constraint of the string model.

The default constraint of the string model is used when no specific constraint is set to a given position. |
| `0 <= Value <= 255` | Sets the character position in the string to which to apply the constraint. |

### `ConstraintType` *(in, AIL_INT64)*

Specifies the type of constraint to set.

*For specifying the constraint*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Sets the constraint to the default constraint.

For a specific character position (the [`CharPos`](../../Reference/str/MstrSetConstraint.md) parameter is set to an individual value), the default is the same as the default constraint of the string model.

For the default character position (the [`CharPos`](../../Reference/str/MstrSetConstraint.md) parameter is set to [`M_DEFAULT`](../../Reference/str/MstrSetConstraint.md)), the default is the same as [`M_ANY`](../../Reference/str/MstrSetConstraint.md). |
| `M_ANY` | Sets the constraint to any character. |
| `M_DIGIT` | Sets the constraint to any digit (0 to 9). |
| `M_DIGIT + M_LETTER` | Sets the constraint to any digit (0 to 9) and any letter (a to z and A to Z). |
| `M_LETTER` | Sets the constraint to any letter (a to z and A to Z).

Note that [`M_LETTER`](../../Reference/str/MstrSetConstraint.md) does not differentiate between upper and lower cases. |

*For specifying the letter case of the constraint*

| Value | Description |
| --- | --- |
| `M_LOWERCASE` | Sets the constraint to any lowercase letter (a to z). |
| `M_UPPERCASE` | Sets the constraint to any uppercase letter (A to Z). |

*For restricting the constraint*

| Value | Description |
| --- | --- |
| `M_FONT_INDEX` | Specifies that the constraint will be restricted to a specific font.

Note that this value is only available for a font-based context. |

### `CharListPtr` *(in, *void)*

Specifies an explicit list of valid characters, at the specified position. This is an optional, null-terminated string.
