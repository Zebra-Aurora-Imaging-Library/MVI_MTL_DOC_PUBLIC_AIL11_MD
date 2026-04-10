---
doctype: Reference
module: 3ddisp
function: M3ddispGetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3ddisp / M3ddispGetHookInfo"
---

# M3ddispGetHookInfo

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

> Get information about a 3D display hook event.

## Syntax

```c
AIL_INT M3ddispGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function allows you to get information about the event that caused the hook-handler function to be called. [`M3ddispGetHookInfo`](../../Reference/3ddisp/M3ddispGetHookInfo.md) should only be called within the scope of a 3D display hook-handler function (see [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the 3D display event identifier received by the hook-handler function (see [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md)).

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information about the event to return.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For retrieving information about a general 3D display hook event

Regardless of the 3D display event that caused the hook-handler function to be called, the [`InfoType`](../../Reference/3ddisp/M3ddispGetHookInfo.md) parameter can be set to the value below.

---

### `M_DISPLAY`

Retrieves the Aurora Imaging Library identifier of the 3D display that generated the event.

| Value | Description |
| --- | --- |
| `3D display identifier` | Specifies the Aurora Imaging Library identifier of the 3D display that generated the event. |

### For retrieving information regarding mouse events

If the hook-handler function was called due to an [`M_MOUSE_...`](../../Reference/3ddisp/M3ddispHookFunction.md) event type, the [`InfoType`](../../Reference/3ddisp/M3ddispGetHookInfo.md) parameter can be set to one of the values below.

---

### `M_MOUSE_POSITION_X`

Retrieves the X-position of the cursor, in display coordinates.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the X-position of the cursor, in display coordinates. |

---

### `M_MOUSE_POSITION_Y`

Retrieves the Y-position of the cursor, in display coordinates.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the Y-position of the cursor, in display coordinates. |

---

### `M_MOUSE_WHEEL_VALUE`

Retrieves the value for the mouse wheel's rotation. Note, this value can only be retrieved if the hook-handler function was called due to the [`M_MOUSE_WHEEL`](../../Reference/3ddisp/M3ddispHookFunction.md) event.

| Value | Description |
| --- | --- |
| `Value` | Specifies the value for the mouse wheel's rotation. A positive value indicates that the wheel was rotated forward, away from the user; a negative value indicates that the wheel was rotate backward, toward the user. |

### For retrieving information regarding keyboard events

If the hook-handler function was called due to an [`M_KEY_...`](../../Reference/3ddisp/M3ddispHookFunction.md) event type, the [`InfoType`](../../Reference/3ddisp/M3ddispGetHookInfo.md) parameter can be set to one of the values below.

---

### `M_AIL_KEY_VALUE`

Retrieves the Aurora Imaging Library operating system-independent key code for the keyboard key that triggered the event. Note, this value can only be retrieved if the hook-handler function was called due to the[`M_KEY_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md), [`M_KEY_UP`](../../Reference/3ddisp/M3ddispHookFunction.md), or [`M_KEY_CHAR`](../../Reference/3ddisp/M3ddispHookFunction.md) event.

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |

---

### `M_KEY_VALUE`

Retrieves the operating system's key code for the keyboard key that triggered the event.

| Value | Description |
| --- | --- |
| `Value` | Specifies the key code. |

### For retrieving information regarding either a mouse or keyboard event

If the hook-handler function was called due to either an [`M_KEY_...`](../../Reference/3ddisp/M3ddispHookFunction.md) or [`M_MOUSE_...`](../../Reference/3ddisp/M3ddispHookFunction.md) event type, the [`InfoType`](../../Reference/3ddisp/M3ddispGetHookInfo.md) parameter can be set to the value below.

---

### `M_COMBINATION_KEYS`

Retrieves which combination keys or mouse buttons triggered the event. Note that a combination of the values below can be returned. Bitwise operators must be used to establish the specific keys or buttons pressed.

| Value | Description |
| --- | --- |
| `M_KEY_ALT` | Specifies the Alt key. |
| `M_KEY_CTRL` | Specifies the Ctrl key. |
| `M_KEY_SHIFT` | Specifies the Shift key. |
| `M_KEY_WIN` | Specifies the Windows key. |
| `M_MOUSE_LEFT_BUTTON` | Specifies the left mouse button. |
| `M_MOUSE_MIDDLE_BUTTON` | Specifies the middle mouse button. |
| `M_MOUSE_RIGHT_BUTTON` | Specifies the right mouse button. |

### For retrieving information regarding frametime events

If the hook-handler function was called due to an [`M_FRAMETIME_...`](../../Reference/3ddisp/M3ddispHookFunction.md) event type, the [`InfoType`](../../Reference/3ddisp/M3ddispGetHookInfo.md) parameter can be set to one of the values below.  > **Note:** Note, the frametime is the time it took to render a frame.

---

### `M_FRAMETIME_AVERAGE`

Retrieves the average frametime in seconds, over the number of frames specified by[`M_FRAMETIME_AVERAGE_SMOOTHING`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the frametime. |

---

### `M_FRAMETIME_AVERAGE_SMOOTHING`

Retrieves the number of frames used when calculating [`M_FRAMETIME_AVERAGE`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of frames. |

---

### `M_FRAMETIME_LAST`

Retrieves the frametime of the last frame in seconds.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the frametime. |

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (\![`M_NULL`](../../Reference/3ddisp/M3ddispGetHookInfo.md)) value is returned.
