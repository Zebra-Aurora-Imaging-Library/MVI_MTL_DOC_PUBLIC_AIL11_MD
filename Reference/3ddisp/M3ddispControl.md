---
doctype: Reference
module: 3ddisp
function: M3ddispControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispControl"
---

# M3ddispControl

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

> Control a 3D display or picking context setting.

## Syntax

```c
void M3ddispControl(
    AIL_ID     Disp3dId,     //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function allows you to control the specified Aurora Imaging Library 3D display or picking context setting.

## Parameters

### `Disp3dId` *(out, AIL_ID)*

Specifies the identifier of the target 3D display or picking context, previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

### `ControlType` *(in, AIL_INT64)*

Specifies the type of 3D display or picking context setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the new value to assign to the 3D display or picking context setting specified by the [`ControlType`](../../Reference/3ddisp/M3ddispControl.md) parameter.

## Parameter Associations

### For controlling keyboard and mouse controls

The following [`ControlType`](../../Reference/3ddisp/M3ddispControl.md) and corresponding [`ControlValue`](../../Reference/3ddisp/M3ddispControl.md) parameter settings are used to enable, disable, or change keyboard and mouse bindings used to control of the 3D display. The default keyboard and mouse bindings are listed under [`M_KEYBOARD_USE`](../../Reference/3ddisp/M3ddispControl.md) and [`M_MOUSE_USE`](../../Reference/3ddisp/M3ddispControl.md).

---

### `M_ACTION_KEY_AUTO_ROTATE`

Sets the keyboard keys that you can press to start and stop auto-rotation around the auto-rotation axis (set using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md)with the identifier of a 3D line geometry object). This action is equivalent to enabling or disabling auto-rotation using [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md).

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
| `M_DEFAULT` |  |
| `M_KEY_R` *(default)* | Specifies the R key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_DOWN`

Sets the keyboard keys that you can press to orbit the viewpoint down around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md) and a negative value.

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_DOWN` *(default)* | Specifies the Down Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_LEFT`

Sets the keyboard keys that you can press to orbit the viewpoint left around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md) and a negative value.

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_LEFT` *(default)* | Specifies the Left Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_RIGHT`

Sets the keyboard keys that you can press to orbit the viewpoint right around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md) and a positive value.

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_RIGHT` *(default)* | Specifies the Right Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_UP`

Sets the keyboard keys that you can press to orbit the viewpoint up around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md) and a positive value.

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_UP` *(default)* | Specifies the Up Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_BOTTOM_TILTED`

Sets the keyboard keys that you can press to set the orientation of the viewpoint so that the view faces the bottom-front of the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_BOTTOM_TILTED`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_1` *(default)* | Specifies the 1 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_BOTTOM_VIEW`

Sets the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly below the interest point (parallel to the Z-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_BOTTOM_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_2` *(default)* | Specifies the 2 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_FRONT_VIEW`

Sets the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly in front of the interest point (parallel to the Y-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_FRONT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_5` *(default)* | Specifies the 5 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_LEFT_VIEW`

Sets the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly to the left of the interest point (parallel to the X-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_LEFT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_4` *(default)* | Specifies the 4 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_REAR_VIEW`

Sets the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly behind the interest point (parallel to the Y-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_REAR_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_3` *(default)* | Specifies the 3 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_RIGHT_VIEW`

Sets the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly to the right of the interest point (parallel to the X-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_RIGHT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_6` *(default)* | Specifies the 6 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_TOP_TILTED`

Sets the keyboard keys that you can press to set orientation of the viewpoint so that the view faces the top-front of the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_TOP_TILTED`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_7` *(default)* | Specifies the 7 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_TOP_VIEW`

Sets the keyboard keys that you can press to set orientation of the viewpoint so that it is directly above the interest point (parallel to the Z-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_TOP_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_8` *(default)* | Specifies the 8 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESET`

Sets the keyboard keys that you can press to reset the view to the current default position and orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_AUTO`](../../Reference/3ddisp/M3ddispSetView.md) and [`M_TOP_TILTED`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_HOME` *(default)* | Specifies the Home key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROLL_LEFT`

Sets the keyboard keys that you can press to roll the view left. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md)+ [`M_COMPOSE_WITH_CURRENT`](../../Reference/3ddisp/M3ddispSetView.md)and a negative value.

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_LEFT + M_KEY_ALT` *(default)* | Specifies the Left Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROLL_RIGHT`

Sets the keyboard keys that you can press to roll the view right. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md)+ [`M_COMPOSE_WITH_CURRENT`](../../Reference/3ddisp/M3ddispSetView.md)and a positive value.

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_RIGHT + M_KEY_ALT` *(default)* | Specifies the Right Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_BACKWARD`

Sets the keyboard keys that you can press to move the view backward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_DOWN + M_KEY_ALT` *(default)* | Specifies the Down Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_DOWN`

Sets the keyboard keys that you can press to move the view downward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_DOWN + M_KEY_SHIFT` *(default)* | Specifies the Down Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_FORWARD`

Sets the keyboard keys that you can press to move the view forward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_UP + M_KEY_ALT` *(default)* | Specifies the Up Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_LEFT`

Sets the keyboard keys that you can press to move the view left relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_LEFT + M_KEY_SHIFT` *(default)* | Specifies the Left Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_RIGHT`

Sets the keyboard keys that you can press to move the view right relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_RIGHT + M_KEY_SHIFT` *(default)* | Specifies the Right Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_UP`

Sets the keyboard keys that you can press to move the view upward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_UP + M_KEY_SHIFT` *(default)* | Specifies the Up Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_DOWN`

Sets the keyboard keys that you can press to pan the view downward by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md) and a positive value.

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_DOWN + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Down Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_LEFT`

Sets the keyboard keys that you can press to pan the view left by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md) and a positive value.

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_LEFT + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Left Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_RIGHT`

Sets the keyboard keys that you can press to pan the view right by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md)and a negative value.

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_RIGHT + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Right Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_UP`

Sets the keyboard keys that you can press to pan the view upward by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md)and a negative value.

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
| `M_DEFAULT` |  |
| `M_KEY_ARROW_UP + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Up Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ZOOM_IN`

Sets the keyboard keys that you can press to move the viewpoint closer to the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)and a value greater than 1.0.

| Value | Description |
| --- | --- |
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
| `M_DEFAULT` |  |
| `M_KEY_ADD` *(default)* | Specifies the Add key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ZOOM_OUT`

Sets the keyboard keys that you can press to move the viewpoint further from the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)and a value between 0.0 and 1.0.

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
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` |  |
| `M_KEY_SUBTRACT` *(default)* | Specifies the Subtract key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_MODIFIER_SPEED`

Sets the keyboard key that you can use to modify the movement speed for all actions when using other keyboard commands. You can set the factor by which the speed will be modified using [`M_ALTERNATE_SPEED_FACTOR`](../../Reference/3ddisp/M3ddispControl.md).

| Value | Description |
| --- | --- |
| *(see `AdditionalKey`)* |  |
| `M_DEFAULT` |  |
| `M_KEY_CTRL` *(default)* | Specifies the CTRL key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ALTERNATE_SPEED_FACTOR`

Sets by which factor to multiply the movement speed when the key set using [`M_ACTION_MODIFIER_SPEED`](../../Reference/3ddisp/M3ddispControl.md) is held.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the factor by which to multiply the movement speed. |

---

### `M_FUNCTION_KEY_CYCLE_APPEARANCES`

Sets the keyboard keys that you can press to cycle through the given appearance modes for all point clouds currently being shown in the internal 3D graphics list of the 3D display. Note, for the appearance change to be visible, the point cloud must have a mesh component.

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
| `M_DEFAULT` |  |
| `M_KEY_CTRL + M_KEY_M` *(default)* | Specifies the Ctrl key with the M key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_FUNCTION_KEY_POINTSIZE_DECREMENT`

Sets the keyboard keys that you can press to decrement the point size ([`M_THICKNESS`](../../Reference/3dgra/M3dgraControl.md)) for all point clouds currently being displayed.  This only affects point clouds which have [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md), or which do not have a mesh. For each point cloud, [`M_THICKNESS`](../../Reference/3dgra/M3dgraControl.md) is decremented independently based on the current value for that point cloud.

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
| `M_DEFAULT` |  |
| `M_KEY_Z` *(default)* | Specifies the Z key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_FUNCTION_KEY_POINTSIZE_INCREMENT`

Sets the keyboard keys that you can press to increment the point size ([`M_THICKNESS`](../../Reference/3dgra/M3dgraControl.md)) for all point clouds currently being displayed.  This only affects point clouds which have [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md), or which do not have a mesh. For each point cloud, [`M_THICKNESS`](../../Reference/3dgra/M3dgraControl.md) is incremented independently based on the current value for that point cloud.

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
| `M_DEFAULT` |  |
| `M_KEY_X` *(default)* | Specifies the X key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_FUNCTION_KEY_TOGGLE_LOD`

Sets the keyboard keys that you can press to cycle through the given level of detail (LoD) modes for all point clouds currently being shown in the internal 3D graphics list of the 3D display.  The LoD mode will cycle through the three modes ([`M_DISABLE`](../../Reference/3ddisp/M3ddispControl.md), [`M_ENABLE_ON_ACTION`](../../Reference/3ddisp/M3ddispControl.md), and [`M_ENABLE_ALWAYS`](../../Reference/3ddisp/M3ddispControl.md)).

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
| `M_DEFAULT` |  |
| `M_KEY_L` *(default)* | Specifies the L key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_KEYBOARD_USE`

Sets whether the user can interactively rotate, orbit, and scroll the view or change the point size and appearance mode for the 3D display using the keyboard.  The default key usage is:  |   |   | | --- | --- | | Up arrow | Orbits the viewpoint up around the interest point (relative to the roll of the view). | | Down arrow | Orbits the viewpoint down around the interest point (relative to the roll of the view). | | Left arrow | Orbits the viewpoint left around the interest point (relative to the roll of the view). | | Right arrow | Orbits the viewpoint right around the interest point (relative to the roll of the view). | | Alt-Up arrow | Moves the view forward by moving both the viewpoint and interest point. | | Alt-Down arrow | Moves the view backward by moving both the viewpoint and interest point. | | Alt-Left arrow | Rolls the view left. | | Alt-Right arrow | Rolls the view right. | | Shift-Up arrow | Scrolls the view up by moving the viewpoint and interest point. | | Shift-Down Arrow | Scrolls the view down by moving the viewpoint and interest point. | | Shift-Left arrow | Scrolls the view left by moving the viewpoint and interest point. | | Shift-Right arrow | Scrolls the view right by moving the viewpoint and interest point. | | Alt-Shift-Up arrow | Rotates the view up by moving the interest point. | | Alt-Shift-Down arrow | Rotates the view down by moving the interest point. | | Alt-Shift-Left arrow | Rotates the view left by moving the interest point. | | Alt-Shift-Right arrow | Rotates the view right by moving the interest point. | | + | Moves the viewpoint towards the interest point (zoom in). | | - | Moves the viewpoint away from the interest point (zoom out). | | Home | Resets the view to the default position and orientation. | | r | Enables or disables auto-rotation. | | Z | Decrement the point size in the display. | | X | Increment the point size in the display. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the keyboard to control the view is disabled. |
| `M_ENABLE` *(default)* | Specifies that using the keyboard to control the view is enabled. |

---

### `M_MOUSE_ROLL`

Sets whether the user can interactively roll the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the mouse to roll the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to roll the view is enabled. |

---

### `M_MOUSE_ROTATION`

Sets whether the user can interactively rotate the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the mouse to rotate the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to rotate the view is enabled. |

---

### `M_MOUSE_TRANSLATION`

Sets whether the user can interactively translate the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the mouse to translate the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to translate the view is enabled. |

---

### `M_MOUSE_TRANSLATION_X`

Sets whether the user can interactively translate the view for the 3D display in the X-direction using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the mouse to translate the view in the X-direction is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to translate the view in the X-direction is enabled. |

---

### `M_MOUSE_TRANSLATION_Y`

Sets whether the user can interactively translate the view for the 3D display in the Y-direction using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the mouse to translate the view in the Y-direction is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to translate the view in the Y-direction is enabled. |

---

### `M_MOUSE_USE`

Sets whether the user can interactively rotate, orbit, and scroll the view for the 3D display using the mouse.  The default mouse usage is:  |   |   | | --- | --- | | Left mouse button + move the mouse | Orbits the viewpoint around the interest point. | | Right mouse button + move the mouse | Scrolls the view by moving the viewpoint and interest point. | | Middle mouse button + move the mouse | Rolls the view. | | Scroll wheel up | Moves the viewpoint towards the interest point. | | Scroll wheel down | Moves the viewpoint away from interest point. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the mouse to control the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to control the view is enabled. |

---

### `M_MOUSE_ZOOM`

Sets whether the user can interactively zoom the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that using the mouse to zoom the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to zoom the view is enabled. |

### For general 3D display settings

The following [`ControlType`](../../Reference/3ddisp/M3ddispControl.md) and corresponding [`ControlValue`](../../Reference/3ddisp/M3ddispControl.md) parameter settings are available for 3D displays.

---

### `M_ASSOCIATED_GRAPHIC_LIST_ID`

Sets the 2D graphics list to associate with the 3D display. When a list is associated it must have no graphics set in world units and it cannot have any graphics with calibration set.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that no 2D graphics list is associated with the 3D display. |
| `2D graphics list identifier` | Specifies the identifier of the 2D graphics list to associate with the 3D display. The 2D graphics list must have been previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). |

---

### `M_AUTO_ROTATE`

Sets whether to automatically rotate the view (both the viewpoint and interest point) around the auto-rotation axis. This gives the impression that the scene is rotating.  By default, the auto-rotation axis is that which is parallel to the Z-axis and intersects the default position of the interest point (typically the center of the first 3D graphic added to the internal 3D graphics list of the 3D display). You can change the auto-rotation axis using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_ROTATION_AXIS`](../../Reference/3ddisp/M3ddispCopy.md)and the identifier of a line 3D geometry object.  > **Note:** Typically, you should only use an auto-rotation axis that intersects the interest point. Some users might be disoriented by the view rotating around a different axis, especially when the rotation indicator is enabled (using [`M_ROTATION_INDICATOR`](../../Reference/3ddisp/M3ddispControl.md)).  Instead of this control type, you can also use [`M_ACTION_KEY_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md)to enable/disable rotation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not auto-rotate the view. |
| `M_ENABLE` | Specifies to auto-rotate the view. |

---

### `M_BACKGROUND_COLOR`

Sets the primary background color for the 3D display, used when [`M_BACKGROUND_MODE`](../../Reference/3ddisp/M3ddispControl.md)is set to [`M_SINGLE_COLOR`](../../Reference/3ddisp/M3ddispControl.md) or[`M_GRADIENT_VERTICAL`](../../Reference/3ddisp/M3ddispControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the background color will be set. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies a dark gray. This is equivalent to**M_RGB888()**with the values (0,35,40). |

---

### `M_BACKGROUND_COLOR_GRADIENT`

Sets the secondary background color for the 3D display, used when [`M_BACKGROUND_MODE`](../../Reference/3ddisp/M3ddispControl.md)specifies that the background is a gradient.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the background color will be set. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies a light gray. This is equivalent to **M_RGB888()**with the values (90,110,130). |

---

### `M_BACKGROUND_MODE`

Sets whether the background of the 3D display is a solid color or a gradient.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BACKGROUND_IMAGE` | Specifies that the background is an image, set using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_BACKGROUND_IMAGE`](../../Reference/3ddisp/M3ddispCopy.md). If no background image is set, the background is shown as though this value were set to [`M_GRADIENT_VERTICAL`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_GRADIENT_VERTICAL` *(default)* | Specifies that the background is a gradient. The top of the window is the color specified by [`M_BACKGROUND_COLOR`](../../Reference/3ddisp/M3ddispControl.md) and the bottom of the window is the color specified by [`M_BACKGROUND_COLOR_GRADIENT`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_SINGLE_COLOR` | Specifies that the background is a solid color, specified by [`M_BACKGROUND_COLOR`](../../Reference/3ddisp/M3ddispControl.md). |

---

### `M_FOV_HORIZONTAL_ANGLE`

Sets the horizontal field of view of the 3D display as an angle. This is the angle that defines how much of the scene is shown within the width of the 3D display.  The default value is whichever value will give the field of view the same aspect ratio as the 3D display's window size, given the default vertical field of view.

| Value | Description |
| --- | --- |
| `0.0 < Value < 180.0` | Specifies the horizontal field of view, in degrees. |

---

### `M_FOV_VERTICAL_ANGLE`

Sets the vertical field of view of the 3D display as an angle. This is the angle that defines how much of the scene is shown within the height of the 3D display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 180.0` *(default)* | Specifies the vertical field of view, in degrees. |

---

### `M_FRAMETIME_AVERAGE_SMOOTHING`

Sets how many frames to use when calculating the average frametime for [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md)with [`M_FRAMETIME_AVERAGE`](../../Reference/3ddisp/M3ddispInquire.md) or [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md)with [`M_FRAMETIME_HIGH`](../../Reference/3ddisp/M3ddispHookFunction.md) or [`M_FRAMETIME_LOW`](../../Reference/3ddisp/M3ddispHookFunction.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of frames. |

---

### `M_FRAMETIME_THRESHOLD_HIGH`

Sets the frametime above which to generate an [`M_FRAMETIME_HIGH`](../../Reference/3ddisp/M3ddispHookFunction.md) event. If you try to set this lower than the current value of [`M_FRAMETIME_THRESHOLD_LOW`](../../Reference/3ddisp/M3ddispControl.md), it will instead be set to the same value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the frametime, in seconds. |

---

### `M_FRAMETIME_THRESHOLD_LOW`

Sets the frametime below which to generate an [`M_FRAMETIME_LOW`](../../Reference/3ddisp/M3ddispHookFunction.md) event. If you try to set this higher than the current value of [`M_FRAMETIME_THRESHOLD_HIGH`](../../Reference/3ddisp/M3ddispControl.md), it will instead be set to the same value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the frametime, in seconds. |

---

### `M_GRAPHIC_LIST_OPACITY`

Sets the opacity level of annotations generated from the 2D graphics list associated with the 3D display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use partial opacity. This is equivalent to setting an opacity of 100. |
| `0 <= Value <= 100` | Specifies the opacity (as a percentage), where 0 is completely transparent and 100 is completely opaque. |

---

### `M_INTERACTIVE_INPUT_MODE`

Sets how the 3D display will handle user inputs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASSOCIATED_GRAPHIC_LIST_2D` | Specifies to use the mouse movements and keystrokes to draw graphics into the associated 2D graphics list ([`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispControl.md)). |
| `M_NORMAL_3D` *(default)* | Specifies to use the mouse movements and keystrokes to manipulate the view of the 3D display. |

---

### `M_LOD_DEGRADATION_LEVEL`

Sets the point/pixel density at which the display will choose a lower level of detail (LoD) for a given point cloud. A lower value will reduce the LoD more aggressively, improving performance at the cost of visible quality reduction. For example, a value below 1 will show lower LoDs even when the view is zoomed in, and values over 4 will rarely show lower LoDs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the point/pixel density. |

---

### `M_LOD_DEGRADE_MODE`

Sets which level of detail (LoD) degradation mode to use.  > **Note:** Note, this setting only applies when [`M_VIEW_BASED_LOD`](../../Reference/3dgra/M3dgraControl.md) is set to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md)

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the LoD change. |
| `M_ENABLE` | Same as [`M_ENABLE_ON_ACTION`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_ENABLE_ALWAYS` | Specifies to always show a degraded LoD. |
| `M_ENABLE_ON_ACTION` | Specifies to use one LoD lower when the user is interacting with the display. Manipulating the view with the mouse, keyboard, or an Aurora Imaging Library function, manipulating a graphics handle, and automatic rotations are all considered interactions with the display.  The display will wait for at least as many seconds specified by [`M_LOD_TIME_TO_SHOW_DEGRADED`](../../Reference/3ddisp/M3ddispControl.md) before rendering a full detail view. |

---

### `M_LOD_TIME_TO_SHOW_DEGRADED`

Sets how long to wait after an automatically degraded render, before rendering at the normally selected LoD.  > **Note:** Note, this has no effect if [`M_LOD_DEGRADE_MODE`](../../Reference/3ddisp/M3ddispControl.md) is set to [`M_DISABLE`](../../Reference/3ddisp/M3ddispControl.md)

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies how long to wait, in seconds. |

---

### `M_PARALLEL_SCALE`

Sets the parallel scale when using the [`M_PARALLEL_PRECISE`](../../Reference/3ddisp/M3ddispControl.md) projection mode.  The parallel scale is the ratio between world units and pixels. For example, if the parallel scale is 2, a plane with a size of 200x200 units will appear as a square of 100x100 pixels (assuming it is viewed directly from the top). As parallel scale increases, the display will zoom out.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the parallel scale. |

---

### `M_PROJECTION_MODE`

Sets the mode of projection, which affects the appearance of 3D graphics shown in the 3D display. The perspective projection mode accounts for perspective to provide information about depth; this is similar to how humans see 3D objects. Parallel projection modes ignore perspective to allow for more accurate measurements.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PARALLEL` | Specifies to use parallel projection.  In this mode, parallel lines remain parallel and the 3D display gives a "blueprint" view. This is useful when trying to determine how two points are aligned with one another, particularly when used with a view that is aligned with the object (for example, top-down or side-on). Unlike [`M_PARALLEL_PRECISE`](../../Reference/3ddisp/M3ddispControl.md), you do not specify a parallel scale. The parallel scale is automatically calculated so that the plane defined by the view direction and interest point has approximately the same size in parallel projection as it would in perspective projection, given the current window size and [`M_FOV_HORIZONTAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md)/[`M_FOV_VERTICAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_PARALLEL_PRECISE` | Specifies to use parallel projection, with a defined ratio between world units and pixel size.  In this mode, parallel lines remain parallel and the 3D display gives a "blueprint" view. This is useful when trying to determine how two points are aligned with one another, particularly when used with a view that is aligned with the object (for example, top-down or side-on). You can specify the ratio between world units and pixels using [`M_PARALLEL_SCALE`](../../Reference/3ddisp/M3ddispControl.md). Note that when this mode is specified, the keyboard and mouse wheel control the parallel scale instead of the view distance. The distance of a point from the viewpoint is irrelevant; it will always be shown at the same position in the 3D display. |
| `M_PERSPECTIVE` *(default)* | Specifies to use perspective projection.  In this mode, the 3D display has perspective distortion. Parallel lines appear to converge and points that are further away appear smaller and closer to the center of the view. Perspective projection does not preserve relative proportions, but it works in a similar way to the human visual system and provides a more natural view than parallel projection.  When using this mode, you can specify an angle to define what is visible ([`M_FOV_..._ANGLE`](../../Reference/3ddisp/M3ddispControl.md)). This is analogous to the focal length of a camera. |

---

### `M_ROTATION_INDICATOR`

Sets whether to show the rotation indicator in the center of the view. The rotation indicator is always positioned at the interest point, and axis-aligned with the working coordinate system of the 3D display. This is useful for showing the current orientation of the view.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to not show the rotation indicator. |
| `M_ENABLE` | Specifies to always show the rotation indicator. |
| `M_ENABLE_ON_MOUSE_CLICK` *(default)* | Specifies to show the rotation indicator when the left mouse button is held down. |

---

### `M_ROTATION_SPEED`

Sets how fast, and in what direction, the view rotates when [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md) is set to [`M_ENABLE`](../../Reference/3ddisp/M3ddispControl.md). This is the number of degrees the view rotates per second.  If this value is set to a positive number, the view will auto-rotate to the right. Otherwise, it will auto-rotate to the left.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the auto-rotation speed, in degrees/sec. |

---

### `M_SIZE_X`

Sets the horizontal resolution of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the horizontal resolution, in pixels. Only integer values are accepted. |

---

### `M_SIZE_Y`

Sets the vertical resolution of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the vertical resolution, in pixels. Only integer values are accepted. |

---

### `M_SYNCHRONIZE`

Synchronizes this thread with in an internal thread by forcing the thread to block until all previously requested operations on the display have been processed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_TITLE`

Sets the display's title to the specified string. If no string is specified, the window is given a default title ("Aurora Imaging Library Display #x").

| Value | Description |
| --- | --- |
| `"WindowTitle"` | Specifies the name of the window title (or display's title). |

---

### `M_TRANSPARENCY_SORT_MODE`

Sets which depth sorting method to use when semi-opaque (semi-transparent) 3D graphics are shown in the 3D display. More advanced modes can resolve visual errors where semi-opaque graphics are always shown in front of other graphics, regardless of which graphic is closer to the viewpoint.  > **Note:** Note that this setting can alter the appearance of both opaque and semi-opaque co-planar surfaces (for example, if you create two plane graphics with the same position and normal). Graphics on higher layers will always be rendered completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DEPTH_PEELING` *(default)* | Specifies to use depth peeling. This mode resolves most transparency sorting errors, at the cost of increased GPU usage when there are multiple semi-opaque 3D graphics in the 3D graphics list of the 3D display. Typically, this mode does not affect performance if all graphics in the graphics list are fully opaque. |
| `M_FAST` | Specifies to use simple depth sorting. This mode offers maximum performance, but can exhibit significant visual errors when there are multiple semi-opaque 3D graphics in the 3D graphics list, particularly if they intersect with other graphics. |

---

### `M_UPDATE`

Sets whether Aurora Imaging Library will update the 3D display. This control type can be used to temporarily disable display updates when the internal 3D graphics list (or a buffer/container associated with a 3D graphic in the 3D graphics list) is being modified by more than one operation; the 3D display can then be updated at the end of all operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to update the 3D display. |
| `M_ENABLE` *(default)* | Specifies to update the 3D display. Also, the 3D display is forced to update immediately. |
| `M_NOW` | Specifies to force an immediate update of the 3D display. In this case, the display is updated even if [`M_UPDATE`](../../Reference/3ddisp/M3ddispControl.md) is set to [`M_DISABLE`](../../Reference/3ddisp/M3ddispControl.md). |

---

### `M_UPDATE_GRAPHIC_LIST`

Sets whether Aurora Imaging Library should update the 3D display when modifications are made to the 3D display's associated 2D graphics list. To associate a 2D graphics list to the 3D display, use [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispControl.md).  With [`M_UPDATE_GRAPHIC_LIST`](../../Reference/3ddisp/M3ddispControl.md), you can temporarily disable display updates ([`M_DISABLE`](../../Reference/3ddisp/M3ddispControl.md)) when the 2D graphics list is being modified by multiple operations; you can then enable display updates ([`M_ENABLE`](../../Reference/3ddisp/M3ddispControl.md)) at the end of all operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the 3D display is not updated when the associated 2D graphics list is modified. |
| `M_ENABLE` *(default)* | Specifies that the 3D display is immediately updated when the associated 2D graphics list is modified (refresh not required). |

---

### `M_VIEW_BOX_NODE`

Sets on which 3D graphic the view will be focused when you use[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_AUTO`](../../Reference/3ddisp/M3ddispSetView.md).  > **Note:** If the specified 3D graphic is later removed from the 3D graphics list, the default setting is automatically restored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0 (the label of the root node). |
| `Value >= 0` | Specifies the label of a 3D graphic in the 3D display's 3D graphics list. |

---

### `M_WINDOW_INITIAL_POSITION_X`

Sets the initial left-most X-coordinate of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in pixels. Only integer values are accepted. |

---

### `M_WINDOW_INITIAL_POSITION_Y`

Sets the initial top-most Y-coordinate of the window. Only integer values are accepted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixels. Only integer values are accepted. |

---

### `M_WINDOW_MAXBUTTON`

Sets whether the window's maximize button is visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MINBUTTON`

Sets whether the window's minimize button is visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MOVE`

Sets whether window movement is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window movement. |
| `M_ENABLE` *(default)* | Specifies to allow window movement. |

---

### `M_WINDOW_OVERLAP`

Sets whether the window can be overlapped by another.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow the window to be overlapped by another (keep window on top). |
| `M_ENABLE` *(default)* | Specifies to allow the window to be overlapped by another. |

---

### `M_WINDOW_RESIZE`

Sets whether window resizing is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window resizing. |
| `M_ENABLE` *(default)* | Specifies to allow window resizing. |

---

### `M_WINDOW_SHOW`

Sets whether the window should be shown or hidden.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the window should be hidden. |
| `M_ENABLE` *(default)* | Specifies that the window should be shown. |

---

### `M_WINDOW_SYSBUTTON`

Sets whether the window's system buttons are visible. The system buttons refer to the collection of the window's minimize, maximize, and close buttons.  To individually set whether the minimize and maximize buttons are visible, change the [`M_WINDOW_MAXBUTTON`](../../Reference/3ddisp/M3ddispControl.md) or [`M_WINDOW_MINBUTTON`](../../Reference/3ddisp/M3ddispControl.md) control type settings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the buttons are not visible. |
| `M_ENABLE` *(default)* | Specifies that the buttons are visible. |

---

### `M_WINDOW_TITLE_BAR`

Sets whether the window's title bar is visible.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the title bar is not visible. |
| `M_ENABLE` *(default)* | Specifies that the title bar is visible. |

### For controlling a picking context

The following [`ControlType`](../../Reference/3ddisp/M3ddispControl.md) and corresponding [`ControlValue`](../../Reference/3ddisp/M3ddispControl.md) parameter settings are available for picking contexts.

---

### `M_BLOCKABLE`

Sets whether 3D graphics are blocked from being picked if the closest 3D graphic at a given pixel is one that has been filtered out. For example, if you are only looking for boxes ([`M_GRAPHIC_TYPE`](../../Reference/3ddisp/M3ddispControl.md) set to [`M_GRAPHIC_TYPE_BOX`](../../Reference/3ddisp/M3ddispControl.md)), and there is a box and a line at a given pixel, but the line is in front of the box, enabling this control type will result in no detected 3D graphic, since the line blocks the box. Disabling this control type will cause the pick operation to ignore the line and detect the box.  Note that enabling this control type might slow down the pick operation and negate the performance benefits of filtering out 3D graphics. When this control type is enabled, Aurora Imaging Library checks all 3D graphics (including those that have been filtered out) on a given layer to determine which ones were hit by the ray, and then identifies the one closest to the viewpoint. When this control type is disabled, Aurora Imaging Library will only check whether the ray hit any 3D graphics that have not been filtered out, resulting in a faster pick operation.  > **Note:** Note, this setting is only available for point picking contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to ignore filtered out 3D graphics entirely. Aurora Imaging Library will not check whether the ray intersected with 3D graphics that have been filtered out. |
| `M_ENABLE` | Specifies to acknowledge filtered out 3D graphics. Aurora Imaging Library will check every 3D graphic to determine with which ones the ray intersected. The pick operation will not report a detected 3D graphic if the closest intersected 3D graphic on the highest layer has been filtered out. |

---

### `M_FACES_AND_LINES`

Sets whether faces and lines can be picked.  Note that the faces of a 3D graphic with a wireframe appearance still exist and can be detected by the pick operation when this control is enabled. If you want to detect 3D graphics inside a wireframe box, you must ignore its faces by disabling this control (since its faces are still valid raycast targets, even though they are not visible).  This control type only applies to meshed point cloud graphics, arc graphics, line graphics, lines graphics, filled graphics, and graphics with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md)set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  To control whether points can be picked, use[`M_POINTS`](../../Reference/3ddisp/M3ddispControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that faces and lines are not pickable. |
| `M_ENABLE` *(default)* | Specifies that faces and lines are pickable. |

---

### `M_GRAPHIC_TYPE`

Sets which type of 3D graphic can be detected by the pick operation. All other types of 3D graphics are filtered out and not pickable.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_TYPE`](Reference/3dgra/M3dgraInquire.md))* |  |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies any type of 3D graphic. |

---

### `M_LABEL`

Sets which label is required for a 3D graphic to be detected by the pick operation. All other 3D graphics are filtered out and not pickable.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NO_LABEL` *(default)* | Specifies to not filter out 3D graphics according to their label. The pick operation searches for all 3D graphics of the specified type, regardless of their label. |
| `Value >= 1` | Specifies the label of the 3D graphic that is pickable. |

---

### `M_OPACITY_THRESHOLD`

Sets the minimum opacity required for a 3D graphic to be detected by the pick operation. 3D graphics with an opacity lower than the specified threshold are filtered out and are not pickable.  Note that 3D graphics with an opacity of 0.0 and 3D graphics that are invisible are never pickable.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the opacity threshold. 3D graphics with an opacity lower than the specified value are not pickable. |

---

### `M_POINTS`

Sets whether points can be picked.  This control type only applies to point cloud graphics, dots graphics, and graphics with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md)set to [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md).  To control whether faces and lines can be picked, use[`M_FACES_AND_LINES`](../../Reference/3ddisp/M3ddispControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that points are not pickable. |
| `M_ENABLE` *(default)* | Specifies that points are pickable. |

---

### `M_RENDER_LAYER_MASK`

Sets on which layers a 3D graphic can be detected by the pick operation. All 3D graphics on different layers are filtered out and not pickable. Graphics on higher layers are always detected before (and drawn in front of) those on lower layers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LAYER_MASK` | Specifies to only detect 3D graphics on the specified layer. |
| `M_LAYER_UNMASK` | Specifies to detect 3D graphics on all but the specified layer. |
| `M_ANY` *(default)* | Specifies to detect 3D graphics on any layer. |
| `0b0000000000 <= Value <= 0b1111111111` | Specifies a bit mask, where the status of bits from 0 to 9 indicate which layers to mask; only 3D graphics on the layers that correspond to bits with non-zero values will be detected. For example, you can set this value to 0b0010000010 (or `(1 &lt;&lt; 1) + (1 &lt;&lt; 7)`) to only detect 3D graphics on layers 1 and 7. |

---

### `M_SELECTION_RADIUS`

Sets the approximate radius around the specified position for the pick operation to search.  > **Note:** Note, this setting is only available for point picking contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1.0` *(default)* | Specifies the selection radius, in pixels. |

### Combination Constants — For specifying additional keyboard keys

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to specify an additional keyboard key.

| Value | Description |
| --- | --- |
| `M_KEY_ALT` | Specifies the Alt key. |
| `M_KEY_CTRL` | Specifies the Ctrl key. |
| `M_KEY_SHIFT` | Specifies the Shift key. |

> **Note:** Note that the display's [`M_FOV_HORIZONTAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md) and [`M_FOV_VERTICAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md)must always have the same aspect ratio as [`M_SIZE_X`](../../Reference/3ddisp/M3ddispControl.md) and [`M_SIZE_Y`](../../Reference/3ddisp/M3ddispControl.md). Any time one of these values is changed, [`M_FOV_HORIZONTAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md) and/or [`M_FOV_VERTICAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md)will be automatically updated to maintain this coherence. For example, if you increase [`M_SIZE_Y`](../../Reference/3ddisp/M3ddispControl.md), [`M_FOV_VERTICAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md)will also be increased by an appropriate amount to maintain the aspect ratio. It is possible for either FoV angle to exceed 180 degrees, either because the window was resized or because the shorter window axis is set to a high FoV angle. If this happens, the FoV angles are clamped such that the ratio is maintained, but neither FoV angle exceeds 179.999 degrees. Once the window size is reduced, the actual FoV angle that was requested will again be used. The "temporary" angle will be used when returning the FoV angle for either axis; if the vertical angle is clamped, inquiring either the vertical or horizontal FoV angle will return values which reflect what is currently shown on-screen, not the values that you would get without the clamping.

If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/3ddisp/M3ddispControl.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/3ddisp/M3ddispControl.md).

This setting only has an effect if you enable [`M_KEYBOARD_USE`](../../Reference/3ddisp/M3ddispControl.md).
