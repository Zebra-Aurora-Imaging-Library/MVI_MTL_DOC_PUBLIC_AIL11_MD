---
doctype: Reference
module: gra
function: MgraGetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraGetHookInfo"
---

# MgraGetHookInfo

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

> Get information about a 2D graphics list event.

## Syntax

```c
AIL_INT MgraGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function allows you to get information about the event that caused the hook-handler function to be called. [`MgraGetHookInfo`](../../Reference/gra/MgraGetHookInfo.md) should only be called within the scope of a 2D graphics list hook-handler function (see [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the 2D graphics list event identifier received by the hook-handler function (see [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md)). The event will be of the type passed to the [`HookType`](../../Reference/gra/MgraHookFunction.md) parameter of the hook-handler function.

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information to get.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For getting information about a 2D graphics list event

---

### `An M_GRAPHIC_LIST_MODIFIED event ID`

Specifies that the event that called the hook-handler function was due to the 2D graphics list being modified, either by an Aurora Imaging Library function or by interactive manipulations.

#### `M_GRAPHIC_LIST_ID`

Retrieves the Aurora Imaging Library identifier of the 2D graphics list that triggered the event.

| Value | Description |
| --- | --- |
| `Graphics list identifier` | Retrieves the Aurora Imaging Library identifier of the 2D graphics list containing the graphic that triggered the event. |

---

### `An M_GRAPHIC_MODIFIED event ID`

Specifies that the event that called the hook-handler function was due to a graphic in the 2D graphics list being modified, either by an Aurora Imaging Library function or by interactive manipulations.  If a graphic's selection ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_GRAPHIC_SELECTED`](../../Reference/gra/MgraControlList.md)) is changed either programmatically or interactively, this event will not be triggered; the [`M_GRAPHIC_SELECTION_MODIFIED`](../../Reference/gra/MgraHookFunction.md) event will be triggered instead.

#### `M_GRAPHIC_CONTROL_TYPE`

Inquires the type of modification made to the graphics in the list that triggered the event.

| Value | Description |
| --- | --- |
| *(see [`ControlType`](Reference/gra/MgraControlList.md))* |  |
| `M_DELETE` | Specifies that one of the following has been called on the 2D graphics list, resulting in some graphics being removed from the 2D graphics list: [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_DELETE`](../../Reference/gra/MgraControlList.md), [`MgraClear`](../../Reference/gra/MgraClear.md), or when [`MgraCopy`](../../Reference/gra/MgraCopy.md) with [`M_MOVE`](../../Reference/gra/MgraCopy.md) deletes graphics from a source 2D graphics list. |
| `M_DRAW_DIRECTION` | Specifies that the option of how an arrow, pointing in the direction with which a graphic was defined is rendered (drawn), was modified. |
| `M_EASY_SELECTION` | Specifies that the option of whether Aurora Imaging Library allows the easy selection of graphics on the display was modified. |
| `M_GRAPHIC_CREATE` | Specifies that a new graphic has been added to the 2D graphics list, either using an Aurora Imaging Library function or through interactive manipulations. |
| `M_GRAPHIC_INTERACTIVE` | Specifies that interactive manipulations, such as translating, rotating, and resizing, have changed the properties of a graphic in the 2D graphics list. |
| `M_GRAPHIC_SOURCE_CALIBRATION` | Specifies that the identifier of the camera calibration information, used to interpret positioning and dimensioning information of a graphic defined in world units, was modified. |
| `M_MOVE` | Specifies that a call to [`MgraCopy`](../../Reference/gra/MgraCopy.md), with an in-place move ([`M_MOVE`](../../Reference/gra/MgraCopy.md)) operation, changed the order of the graphics inside the 2D graphics list. |
| `M_MULTIPLE_SELECTION` | Specifies that the option of whether to permit interactive multiple selection using the Ctrl key was modified. |
| `M_MULTIPLE_SELECTION_KEY` | Specifies that the keyboard key that you can press to select multiple graphics with your mouse on the display was modified. |
| `M_RESIZE_HEIGHT` | Specifies that the graphic's height was increased or decreased. |
| `M_RESIZE_WIDTH` | Specifies that the graphic's width was increased or decreased. |
| `M_ROTATE` | Specifies that a rotation operation was performed on the graphic, or one of its sub-elements. |

#### `M_GRAPHIC_LABEL_VALUE`

Retrieves the label of the modified graphic that triggered the event.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that all graphics in the 2D graphics list were modified using [`MgraClear`](../../Reference/gra/MgraClear.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_ALL`](../../Reference/gra/MgraControlList.md). |
| `M_ALL_SELECTED` | Specifies that all selected graphics in the 2D graphics list were modified using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_ALL_SELECTED`](../../Reference/gra/MgraControlList.md). |
| `M_LIST` | Specifies that a 2D graphics list's setting was modified using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_LIST`](../../Reference/gra/MgraControlList.md). |
| `M_MULTIPLE_LABELS` | Specifies that more than one graphic was modified. This can happen when calling [`MgraCopy`](../../Reference/gra/MgraCopy.md) and with interactive manipulations (for example, translating many graphics). |
| `Value` | Specifies the label of the graphic. |

#### `M_GRAPHIC_LIST_ID`

Retrieves the Aurora Imaging Library identifier of the 2D graphics list containing the graphic that triggered the event.

| Value | Description |
| --- | --- |
| `Graphics list identifier` | Retrieves the Aurora Imaging Library identifier of the 2D graphics list containing the graphic that triggered the event. |

#### `M_GRAPHIC_SUB_INDEX`

Retrieves the index of the sub-element of the graphic that triggered the event.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the event affected the whole graphic. |
| `Value` | Specifies the index of the sub-element of the graphic that triggered the event. |

---

### `An M_GRAPHIC_SELECTION_MODIFIED event ID`

Specifies that the event that called the hook-handler function was due to a graphic being selected or deselected.

#### `M_GRAPHIC_LABEL_VALUE`

Retrieves the label of the graphic that triggered the event.

| Value | Description |
| --- | --- |
| `M_MULTIPLE_LABELS` | Specifies that more than one graphic was selected. This can happen when calling [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_GRAPHIC_SELECTED`](../../Reference/gra/MgraControlList.md) and with interactive manipulations (for example, Ctrl-clicking many graphics). |
| `M_NO_LABEL` | Specifies a deselection triggered the event and no graphic was selected. |
| `Value` | Specifies the label of the graphic that was selected. |

#### `M_GRAPHIC_LABEL_VALUE_DESELECTED`

Retrieves the label of the graphic that was deselected when the event was triggered.

| Value | Description |
| --- | --- |
| `M_MULTIPLE_LABELS` | Specifies that more than one graphic was deselected. |
| `M_NO_LABEL` | Specifies that no graphic was deselected. |
| `Value` | Specifies the label of the graphic that was deselected. |

#### `M_GRAPHIC_LIST_ID`

Retrieves the Aurora Imaging Library identifier of the 2D graphics list containing the graphic that triggered the event.

| Value | Description |
| --- | --- |
| `Graphics list identifier` | Retrieves the Aurora Imaging Library identifier of the 2D graphics list containing the graphic that triggered the event. |

---

### `An M_INTERACTIVE_GRAPHIC_STATE_MODIFIED event ID`

Specifies that the event that called the hook-handler function was due to the 2D graphics list's interactive state being modified, either by an Aurora Imaging Library function or by interactive manipulations.

#### `M_GRAPHIC_LABEL_VALUE`

Retrieves the label of the graphic that triggered the event. For example, it can return the label of the graphic being dragged when the state of interactivity ([`M_INTERACTIVE_GRAPHIC_STATE`](../../Reference/gra/MgraGetHookInfo.md)) is [`M_STATE_GRAPHIC_DRAGGED`](../../Reference/gra/MgraGetHookInfo.md).

| Value | Description |
| --- | --- |
| `M_NO_LABEL` | Specifies that an Aurora Imaging Library function triggered the event. |
| `Value` | Specifies the label of the graphic that triggered the event. |

#### `M_GRAPHIC_LIST_ID`

Retrieves the Aurora Imaging Library identifier of the 2D graphics list that triggered the event.

| Value | Description |
| --- | --- |
| `Graphics list identifier` | Retrieves the Aurora Imaging Library identifier of the 2D graphics list containing the graphic that triggered the event. |

#### `M_INTERACTIVE_GRAPHIC_PREVIOUS_STATE`

Inquires the state of interactivity of the 2D graphics list before the event occurred. This always returns a different value than [`M_INTERACTIVE_GRAPHIC_STATE`](../../Reference/gra/MgraGetHookInfo.md).

| Value | Description |
| --- | --- |
| `M_STATE_BEING_CREATED` | Specifies that a new graphic was being created. |
| `M_STATE_GRAPHIC_DRAGGED` | Specifies that a graphic was being dragged using the mouse. |
| `M_STATE_GRAPHIC_HOVERED` | Specifies that the cursor was hovering over a graphic. |
| `M_STATE_HANDLE_DRAGGED` | Specifies that a graphic's handle was being dragged using the mouse. |
| `M_STATE_HANDLE_HOVERED` | Specifies that the cursor was hovering over a graphic's handle. |
| `M_STATE_IDLE` | Specifies that the cursor was not hovering over anything and that no graphic was queued for creation. |
| `M_STATE_WAITING_FOR_CREATION` | Specifies that a graphic was queued for creation. |

#### `M_INTERACTIVE_GRAPHIC_STATE`

Inquires the current state of interactivity of the 2D graphics list.

| Value | Description |
| --- | --- |
| `M_STATE_BEING_CREATED` | Specifies that a new graphic is being created. |
| `M_STATE_GRAPHIC_DRAGGED` | Specifies that a graphic is being dragged using the mouse. |
| `M_STATE_GRAPHIC_HOVERED` | Specifies that the cursor is currently hovering over a graphic. |
| `M_STATE_HANDLE_DRAGGED` | Specifies that a graphic's handle is being dragged using the mouse. |
| `M_STATE_HANDLE_HOVERED` | Specifies that the cursor is currently hovering over a graphic's handle. |
| `M_STATE_IDLE` | Specifies that the cursor is not hovering over anything and that no graphic is queued for creation. |
| `M_STATE_WAITING_FOR_CREATION` | Specifies that a graphic is queued for creation. |

### Combination Constants — Returns whether a rectangle's location in the image will not change

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that a rectangle's location in the image will not change, regardless of how Aurora Imaging Library interprets its reference position.

| Value | Description |
| --- | --- |
| `M_SAME_LOCATION` | Specifies that, when you set [`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md), Aurora Imaging Library also adjusts the rectangle's actual position in the image so it remains at the same location. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/gra/MgraInquireList.md) should only be used with [`M_GRAPHIC_LIST_ID`](../../Reference/gra/MgraGetHookInfo.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (!`M_NULL`) value is returned.
