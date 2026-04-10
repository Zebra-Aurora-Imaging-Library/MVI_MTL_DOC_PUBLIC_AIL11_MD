---
doctype: Reference
module: 3ddisp
function: M3ddispInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispInquire"
---

# M3ddispInquire

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

> Inquire an Aurora Imaging Library 3D display or picking context setting.

## Syntax

```c
AIL_INT64 M3ddispInquire(
    AIL_ID    Disp3dId,     //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function allows you to inquire the specified Aurora Imaging Library 3D display or picking context setting.

## Parameters

### `Disp3dId` *(in, AIL_ID)*

Specifies the identifier of the target 3D display or picking context, previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

### `InquireType` *(in, AIL_INT64)*

Specifies the type of 3D display or picking context setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For inquiring keyboard and mouse controls

You can use the following [`InquireType`](../../Reference/3ddisp/M3ddispInquire.md) to inquire about the keyboard and mouse bindings used to control of the 3D display.

---

### `M_ACTION_KEY_AUTO_ROTATE`

Inquires the keyboard keys that you can press to start and stop auto-rotation around the interest point. This action is equivalent to enabling or disabling auto-rotation using [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `M_KEY_R` *(default)* | Specifies the R key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_DOWN`

Inquires the keyboard keys that you can press to orbit the viewpoint down around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md) and a negative value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_DOWN` *(default)* | Specifies the Down Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_LEFT`

Inquires the keyboard keys that you can press to orbit the viewpoint left around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md) and a negative value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_LEFT` *(default)* | Specifies the Left Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_RIGHT`

Inquires the keyboard keys that you can press to orbit the viewpoint right around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md) and a positive value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_RIGHT` *(default)* | Specifies the Right Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORBIT_UP`

Inquires the keyboard keys that you can press to orbit the viewpoint up around the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md) and a positive value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_UP` *(default)* | Specifies the Up Arrow key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_BOTTOM_TILTED`

Inquires the keyboard keys that you can press to set the orientation of the viewpoint so that the view faces the bottom-front of the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_BOTTOM_TILTED`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_1` *(default)* | Specifies the 1 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_BOTTOM_VIEW`

Inquires the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly below the interest point (parallel to the Z-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_BOTTOM_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_2` *(default)* | Specifies the 2 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_FRONT_VIEW`

Inquires the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly in front of the interest point (parallel to the Y-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_FRONT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_5` *(default)* | Specifies the 5 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_LEFT_VIEW`

Inquires the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly to the left of the interest point (parallel to the X-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_LEFT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_4` *(default)* | Specifies the 4 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_REAR_VIEW`

Inquires the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly behind the interest point (parallel to the Y-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_REAR_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_3` *(default)* | Specifies the 3 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_RIGHT_VIEW`

Inquires the keyboard keys that you can press to set the orientation of the viewpoint so that it is directly to the right of the interest point (parallel to the X-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_RIGHT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_6` *(default)* | Specifies the 6 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_TOP_TILTED`

Inquires the keyboard keys that you can press to set orientation of the viewpoint so that the view faces the top-front of the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_TOP_TILTED`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_7` *(default)* | Specifies the 7 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ORIENTATION_TOP_VIEW`

Inquires the keyboard keys that you can press to set orientation of the viewpoint so that it is directly above the interest point (parallel to the Z-axis of the working coordinate system of the 3D display). This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_TOP_VIEW`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_8` *(default)* | Specifies the 8 key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESET`

Inquires the keyboard keys that you can press to reset the view to the default position and orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_AUTO`](../../Reference/3ddisp/M3ddispSetView.md) and [`M_TOP_TILTED`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_HOME` *(default)* | Specifies the Home key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROLL_LEFT`

Inquires the keyboard keys that you can press to roll the view left. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md)+ [`M_COMPOSE_WITH_CURRENT`](../../Reference/3ddisp/M3ddispSetView.md)and a negative value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_LEFT + M_KEY_ALT` *(default)* | Specifies the Left Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROLL_RIGHT`

Inquires the keyboard keys that you can press to roll the view right. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md)+ [`M_COMPOSE_WITH_CURRENT`](../../Reference/3ddisp/M3ddispSetView.md)and a positive value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_RIGHT + M_KEY_ALT` *(default)* | Specifies the Right Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_BACKWARD`

Inquires the keyboard keys that you can press to move the view backward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_DOWN + M_KEY_ALT` *(default)* | Specifies the Down Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_DOWN`

Inquires the keyboard keys that you can press to move the view downward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_DOWN + M_KEY_SHIFT` *(default)* | Specifies the Down Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_FORWARD`

Inquires the keyboard keys that you can press to move the view forward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_UP + M_KEY_ALT` *(default)* | Specifies the Up Arrow key with the Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_LEFT`

Inquires the keyboard keys that you can press to move the view left relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_LEFT + M_KEY_SHIFT` *(default)* | Specifies the Left Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_RIGHT`

Inquires the keyboard keys that you can press to move the view right relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_RIGHT + M_KEY_SHIFT` *(default)* | Specifies the Right Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_UP`

Inquires the keyboard keys that you can press to move the view upward relative to its current orientation. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_UP + M_KEY_SHIFT` *(default)* | Specifies the Up Arrow key with the Shift key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_DOWN`

Inquires the keyboard keys that you can press to pan the view downward by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md) with a positive value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_DOWN + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Down Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_LEFT`

Inquires the keyboard keys that you can press to pan the view left by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md) with a positive value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_LEFT + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Left Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_RIGHT`

Inquires the keyboard keys that you can press to pan the view right by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md)with a negative value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_RIGHT + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Right Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TURN_UP`

Inquires the keyboard keys that you can press to pan the view upward by moving the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md)+[`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md)with a negative value.

| Value | Description |
| --- | --- |
| `M_KEY_ARROW_UP + M_KEY_SHIFT + M_KEY_ALT` *(default)* | Specifies the Up Arrow key with the Shift key and Alt key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ZOOM_IN`

Inquires the keyboard keys that you can press to move the viewpoint closer to the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)and a value greater than 1.0.

| Value | Description |
| --- | --- |
| `M_KEY_ADD` *(default)* | Specifies the Add key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ZOOM_OUT`

Inquires the keyboard keys that you can press to move the viewpoint further from the interest point. This action is equivalent to using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)and a value between 0.0 and 1.0.

| Value | Description |
| --- | --- |
| `M_KEY_SUBTRACT` *(default)* | Specifies the Subtract key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_MODIFIER_SPEED`

Inquires the keyboard key that you can use to modify the movement speed for all actions when using other keyboard commands.

| Value | Description |
| --- | --- |
| `M_KEY_CTRL` *(default)* | Specifies the CTRL key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ALTERNATE_SPEED_FACTOR`

Inquires by which factor the movement speed is multiplied when the key set using [`M_ACTION_MODIFIER_SPEED`](../../Reference/3ddisp/M3ddispInquire.md) is held.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the factor by which to multiply the movement speed. |

---

### `M_FUNCTION_KEY_CYCLE_APPEARANCES`

Inquires the keyboard keys that you can press to cycle through the given appearance modes for all point clouds currently being shown in the internal 3D graphics list of the 3D display.

| Value | Description |
| --- | --- |
| `M_KEY_CTRL + M_KEY_M` *(default)* | Specifies the Ctrl key with the M key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_FUNCTION_KEY_POINTSIZE_DECREMENT`

Inquires the keyboard keys that you can press to decrement the point size for all point clouds currently being displayed.

| Value | Description |
| --- | --- |
| `M_KEY_Z` *(default)* | Specifies the Z key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_FUNCTION_KEY_POINTSIZE_INCREMENT`

Inquires the keyboard keys that you can press to increment the point size for all point clouds currently being displayed.

| Value | Description |
| --- | --- |
| `M_KEY_X` *(default)* | Specifies the X key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_FUNCTION_KEY_TOGGLE_LOD`

Inquires the keyboard keys that you can press to cycle through the given level of detail (LoD) modes for all point clouds currently being shown in the internal 3D graphics list of the 3D display.  The LoD mode will cycle through the three modes ([`M_DISABLE`](../../Reference/3ddisp/M3ddispInquire.md), [`M_ENABLE_ON_ACTION`](../../Reference/3ddisp/M3ddispInquire.md), and [`M_ENABLE_ALWAYS`](../../Reference/3ddisp/M3ddispInquire.md)).

| Value | Description |
| --- | --- |
| `M_KEY_L` *(default)* | Specifies the L key. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_KEYBOARD_USE`

Inquires whether the user can interactively rotate, orbit, and scroll the view for the 3D display using the keyboard.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the keyboard to control the view is disabled. |
| `M_ENABLE` *(default)* | Specifies that using the keyboard to control the view is enabled. |

---

### `M_MOUSE_ROLL`

Inquires whether the user can interactively roll the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the mouse to roll the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to roll the view is enabled. |

---

### `M_MOUSE_ROTATION`

Inquires whether the user can interactively rotate the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the mouse to rotate the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to rotate the view is enabled. |

---

### `M_MOUSE_TRANSLATION`

Inquires whether the user can interactively translate the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the mouse to translate the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to translate the view is enabled. |

---

### `M_MOUSE_TRANSLATION_X`

Inquires whether the user can interactively translate the view for the 3D display in the X-direction using the mouse.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the mouse to translate the view in the X-direction is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to translate the view in the X-direction is enabled. |

---

### `M_MOUSE_TRANSLATION_Y`

Inquires whether the user can interactively translate the view for the 3D display in the Y-direction using the mouse.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the mouse to translate the view in the Y-direction is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to translate the view in the Y-direction is enabled. |

---

### `M_MOUSE_USE`

Inquires whether the user can interactively rotate, orbit, and scroll the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the mouse to control the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to control the view is enabled. |

---

### `M_MOUSE_ZOOM`

Inquires whether the user can interactively zoom the view for the 3D display using the mouse.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that using the mouse to zoom the view is not enabled. |
| `M_ENABLE` *(default)* | Specifies that using the mouse to zoom the view is enabled. |

### Combination Constants — For specifying additional keyboard keys

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify an additional keyboard key.

| Value | Description |
| --- | --- |
| `M_KEY_ALT` | Specifies the Alt key. |
| `M_KEY_CTRL` | Specifies the Ctrl key. |
| `M_KEY_SHIFT` | Specifies the Shift key. |

### For 3D displays

The following [`InquireType`](../../Reference/3ddisp/M3ddispInquire.md) and corresponding [`UserVarPtr`](../../Reference/3ddisp/M3ddispInquire.md) parameter settings are available for 3D displays.

---

### `M_3D_GRAPHIC_LIST_ID`

Inquires the Aurora Imaging Library identifier of the internal 3D graphics list associated with this 3D display.  > **Note:** Note that the 3D display is always associated with the same internal 3D graphics list, which is automatically allocated and freed at the same time as the 3D display. You should not attempt to free the internal graphics list. To display the contents of another 3D graphics list on a 3D display, you can copy the contents of the source 3D graphics list into the 3D display's internal 3D graphics list using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md). You can also use [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) with [`M_ADD`](../../Reference/3ddisp/M3ddispSelect.md) to add a 3D graphic to the internal graphics list or to add a graphics list to the 3D display context.

---

### `M_ASSOCIATED_GRAPHIC_LIST_ID`

Inquires the 2D graphics list to associate with the 3D display. When a list is associated it must have no graphics set in world units and it cannot have any graphics with calibration set.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies that no 2D graphics list is associated with the 3D display. |
| `2D graphics list identifier` | Specifies the identifier of the 2D graphics list to associate with the 3D display. The 2D graphics list must have been previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). |

---

### `M_AUTO_ROTATE`

Inquires whether to automatically rotate the view (both the viewpoint and interest point) around the auto-rotation axis. This gives the impression that the scene is rotating.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to not auto-rotate the view. |
| `M_ENABLE` | Specifies to auto-rotate the view. |

---

### `M_BACKGROUND_COLOR`

Inquires the primary background color for the 3D display, used when [`M_BACKGROUND_MODE`](../../Reference/3ddisp/M3ddispInquire.md)is set to [`M_SINGLE_COLOR`](../../Reference/3ddisp/M3ddispInquire.md) or[`M_GRADIENT_VERTICAL`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_BACKGROUND_COLOR`](Reference/3ddisp/M3ddispControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_BACKGROUND_COLOR_GRADIENT`

Inquires the secondary background color for the 3D display, used when [`M_BACKGROUND_MODE`](../../Reference/3ddisp/M3ddispInquire.md)specifies that the background is a gradient.

| Value | Description |
| --- | --- |
| *(see [`M_BACKGROUND_COLOR_GRADIENT`](Reference/3ddisp/M3ddispControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_BACKGROUND_IMAGE_SIZE_BAND`

Inquires the number of color bands of the background image buffer. This inquire type is only available if the 3D display has a background image buffer, set using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_BACKGROUND_IMAGE`](../../Reference/3ddisp/M3ddispCopy.md).

| Value | Description |
| --- | --- |
| `1` | Specifies the buffer has one band. |
| `3` | Specifies the buffer has three bands. |

---

### `M_BACKGROUND_IMAGE_SIZE_X`

Inquires the width of the background image buffer. This inquire type is only available if the 3D display has a background image buffer, set using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_BACKGROUND_IMAGE`](../../Reference/3ddisp/M3ddispCopy.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the buffer, in pixels. |

---

### `M_BACKGROUND_IMAGE_SIZE_Y`

Inquires the height of the background image buffer. This inquire type is only available if the 3D display has a background image buffer, set using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_BACKGROUND_IMAGE`](../../Reference/3ddisp/M3ddispCopy.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the buffer, in pixels. |

---

### `M_BACKGROUND_MODE`

Inquires whether the background of the 3D display is a solid color or a gradient.

| Value | Description |
| --- | --- |
| `M_BACKGROUND_IMAGE` | Specifies that the background is an image, set using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_BACKGROUND_IMAGE`](../../Reference/3ddisp/M3ddispCopy.md). |
| `M_GRADIENT_VERTICAL` *(default)* | Specifies that the background is a gradient. |
| `M_SINGLE_COLOR` | Specifies that the background is a solid color, specified by [`M_BACKGROUND_COLOR`](../../Reference/3ddisp/M3ddispInquire.md). |

---

### `M_COLOR_LUT_ID`

Inquires the identifier of the current LUT buffer associated with the 3D display's internal 3D graphics list. If the current LUT is predefined (default [`M_COLORMAP_TURBO`](../../Reference/3dgra/M3dgraCopy.md)), Aurora Imaging Library returns the type of predefined colormap to which the LUT is set ([`M_COLORMAP_...`](../../Reference/3dgra/M3dgraCopy.md)). If the current LUT is user-defined, the identifier of an internal copy of the user-defined LUT is returned.  For a user-defined LUT that has an internal Aurora Imaging Library identifier, you must not free the associated buffer. You can, however, modify the buffer so that newly added point cloud graphics receive the modified LUT. Note that each point cloud graphic gets its own copy of the LUT associated with the 3D graphics list when it is created.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LUT_ID`](Reference/3dgra/M3dgraInquire.md))* |  |

---

### `M_DISPLAY_TYPE`

Inquires how the 3D display is presented.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EXCLUSIVE` | Specifies to present the 3D display full-screen, without a windowed border, in one of the Windows desktop screens. |
| `M_WEB` | Specifies to allocate a 3D display that is presented upon selection in one or more instances of an Aurora Imaging Web Library client application (for example, running in a compatible web browser). |
| `M_WINDOWED` *(default)* | Specifies to allocate a 3D display that is presented upon selection in its own window on the Windows desktop screen(s). |

---

### `M_FOV_HORIZONTAL_ANGLE`

Inquires the horizontal field of view of the 3D display. This is the angle which defines how much of the scene is shown within the width of the 3D display.

| Value | Description |
| --- | --- |
| `0.0 < Value < 180.0` | Specifies the horizontal field of view, in degrees. |

---

### `M_FOV_VERTICAL_ANGLE`

Inquires the vertical field of view of the 3D display. This is the angle which defines how much of the scene is shown within the height of the 3D display.

| Value | Description |
| --- | --- |
| `0.0 < Value < 180.0` *(default)* | Specifies the vertical field of view, in degrees. |

---

### `M_FRAMETIME_AVERAGE`

Inquires the average frametime in seconds, over the number of frames specified by [`M_FRAMETIME_AVERAGE_SMOOTHING`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the frametime in seconds. |

---

### `M_FRAMETIME_AVERAGE_SMOOTHING`

Inquires how many frames to use when calculating the average frametime returned using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md)with [`M_FRAMETIME_AVERAGE`](../../Reference/3ddisp/M3ddispInquire.md) or [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md)with [`M_FRAMETIME_HIGH`](../../Reference/3ddisp/M3ddispHookFunction.md) or [`M_FRAMETIME_LOW`](../../Reference/3ddisp/M3ddispHookFunction.md).

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the number of frames. |

---

### `M_FRAMETIME_LAST`

Inquires the frametime of the last frame, in seconds; this is the time it took to render the frame.  A low frametime indicates good GPU performance. A high frametime indicates poor GPU performance. If the frametime is too low, you either need to use a better GPU or reduce the complexity of your scene.  For the 3D display to be usable interactively, you will typically want to target at least a frametime of 0.06 sec (equivalent to 15 fps). A frametime of 0.03 sec (equivalent to 30 fps) is preferable, and less than 0.016 sec (equivalent to 60 fps) is ideal.  When grabbing continuously, if the frametime is higher than the time between grabs, some grabs will never be shown in the display.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the frametime, in seconds. |

---

### `M_FRAMETIME_THRESHOLD_HIGH`

Inquires the frametime above which to generate an [`M_FRAMETIME_HIGH`](../../Reference/3ddisp/M3ddispHookFunction.md) event.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the frametime, in seconds. |

---

### `M_FRAMETIME_THRESHOLD_LOW`

Inquires the frametime below which to generate an [`M_FRAMETIME_LOW`](../../Reference/3ddisp/M3ddispHookFunction.md) event.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the frametime, in seconds. |

---

### `M_GRAPHIC_LIST_OPACITY`

Inquires the opacity level of annotations generated from the 2D graphics list associated with the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_LIST_OPACITY`](Reference/3ddisp/M3ddispControl.md))* |  |

---

### `M_INTERACTIVE_INPUT_MODE`

Inquires how the 3D display will handle user inputs.

| Value | Description |
| --- | --- |
| `M_ASSOCIATED_GRAPHIC_LIST_2D` | Specifies to use the mouse movements and keystrokes to draw graphics into the associated 2D graphics list ([`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)). |
| `M_NORMAL_3D` *(default)* | Specifies to use the mouse movements and keystrokes to manipulate the view of the 3D display. |

---

### `M_LOD_DEGRADATION_LEVEL`

Inquires the point/pixel density at which the display will choose a lower level of detail (LoD) for a given point cloud. A lower value will reduce the LoD more aggressively, improving performance at the cost of visible quality reduction. For example, a value below 1 will show lower LoDs even when the view is zoomed in, and values over 4 will rarely show lower LoDs.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the point/pixel density. |

---

### `M_LOD_DEGRADE_MODE`

Inquires which level of detail (LoD) degradation mode to use.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the LoD change. |
| `M_ENABLE` | Same as [`M_ENABLE_ON_ACTION`](../../Reference/3ddisp/M3ddispInquire.md). |
| `M_ENABLE_ALWAYS` | Specifies to always show a degraded LoD. |
| `M_ENABLE_ON_ACTION` | Specifies to use one LoD lower when the user is interacting with the display. |

---

### `M_LOD_TIME_TO_SHOW_DEGRADED`

Inquires how long to wait after an automatically degraded render, before rendering at the normally selected LoD.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies how long to wait, in seconds. |

---

### `M_PARALLEL_SCALE`

Inquires the parallel scale when using the [`M_PARALLEL_PRECISE`](../../Reference/3ddisp/M3ddispInquire.md) projection mode.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the parallel scale. |

---

### `M_PROJECTION_MODE`

Inquires the mode of projection, which affects the appearance of 3D graphics shown in the 3D display.

| Value | Description |
| --- | --- |
| `M_PARALLEL` | Specifies to use parallel projection. |
| `M_PARALLEL_PRECISE` | Specifies to use parallel projection, with a defined ratio between world units and pixel size. |
| `M_PERSPECTIVE` *(default)* | Specifies to use perspective projection. |

---

### `M_ROTATION_INDICATOR`

Inquires whether to show the rotation indicator in the center of the view.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies to not show the rotation indicator. |
| `M_ENABLE` | Specifies to always show the rotation indicator. |
| `M_ENABLE_ON_MOUSE_CLICK` *(default)* | Specifies to show the rotation indicator when the left mouse button is held down. |

---

### `M_ROTATION_SPEED`

Inquires how fast, and in what direction, the view rotates when [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispInquire.md) is set to [`M_ENABLE`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the auto-rotation speed, in degrees/sec. |

---

### `M_SHOWN_LISTS`

Inquires the identifiers of the 3D graphics lists currently shown in the display.

| Value | Description |
| --- | --- |
| `3D graphics list identifiers` | Specifies the identifiers of the 3D graphics lists currently shown in the display. |

---

### `M_SIZE_X`

Inquires the horizontal resolution of the window.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the horizontal resolution, in pixels. |

---

### `M_SIZE_Y`

Inquires the vertical resolution of the window.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the vertical resolution, in pixels. |

---

### `M_TITLE`

Inquires the 3D display's title.

| Value | Description |
| --- | --- |
| `"Title"` | Specifies the title of the 3D display. |

---

### `M_TRANSPARENCY_SORT_MODE`

Inquires which depth sorting method to use when semi-opaque (semi-transparent) 3D graphics are shown in the 3D display.

| Value | Description |
| --- | --- |
| `M_DEPTH_PEELING` *(default)* | Specifies to use depth peeling. |
| `M_FAST` | Specifies to use simple depth sorting. |

---

### `M_UPDATE`

Inquires whether Aurora Imaging Library will update the 3D display.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to update the 3D display. |
| `M_ENABLE` *(default)* | Specifies to update the 3D display. |

---

### `M_UPDATE_GRAPHIC_LIST`

Inquires whether Aurora Imaging Library should update the 3D display when modifications are made to the display's associated 2D graphics list. To associate a 2D graphics list to the 3D display, use [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the 3D display is not updated when the associated 2D graphics list is modified. |
| `M_ENABLE` *(default)* | Specifies that the 3D display is immediately updated when the associated 2D graphics list is modified (refresh not required). |

---

### `M_VIEW_BOX_NODE`

Inquires to which 3D graphic the view will be reset when you use[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_AUTO`](../../Reference/3ddisp/M3ddispSetView.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the label of a 3D graphic in the 3D display's 3D graphics list. |

---

### `M_WINDOW_INITIAL_POSITION_X`

Inquires the initial left-most X-coordinate of the window.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the X-coordinate, in pixels. |

---

### `M_WINDOW_INITIAL_POSITION_Y`

Inquires the initial top-most Y-coordinate of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixels. |

---

### `M_WINDOW_MAXBUTTON`

Inquires whether the window's maximize button is visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MINBUTTON`

Inquires whether the window's minimize button is visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MOVE`

Inquires whether window movement is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window movement. |
| `M_ENABLE` *(default)* | Specifies to allow window movement. |

---

### `M_WINDOW_OVERLAP`

Inquires whether the window can be overlapped by another.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow the window to be overlapped by another (keep window on top). |
| `M_ENABLE` *(default)* | Specifies to allow the window to be overlapped by another. |

---

### `M_WINDOW_RESIZE`

Inquires whether window resizing is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window resizing. |
| `M_ENABLE` *(default)* | Specifies to allow window resizing. |

---

### `M_WINDOW_SHOW`

Inquires whether the window should be shown or hidden.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the window should be hidden. |
| `M_ENABLE` *(default)* | Specifies that the window should be shown. |

---

### `M_WINDOW_SYSBUTTON`

Inquires whether the window's system buttons are visible. The system buttons refer to the collection of the window's minimize, maximize, and close buttons.  To individually set whether the minimize and maximize buttons are visible, change the [`M_WINDOW_MAXBUTTON`](../../Reference/3ddisp/M3ddispInquire.md) or [`M_WINDOW_MINBUTTON`](../../Reference/3ddisp/M3ddispInquire.md) control type settings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the buttons are not visible. |
| `M_ENABLE` *(default)* | Specifies that the buttons are visible. |

---

### `M_WINDOW_TITLE_BAR`

Inquires whether the window's title bar is visible.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the title bar is not visible. |
| `M_ENABLE` *(default)* | Specifies that the title bar is visible. |

### For picking contexts

The following [`InquireType`](../../Reference/3ddisp/M3ddispInquire.md) and corresponding [`UserVarPtr`](../../Reference/3ddisp/M3ddispInquire.md) parameter settings are available for picking contexts.

---

### `M_BLOCKABLE`

Inquires whether 3D graphics are blocked from being picked if the closest 3D graphic at a given pixel is one that has been filtered out.  > **Note:** Note, this setting is only available for point picking contexts.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to ignore filtered out 3D graphics entirely. |
| `M_ENABLE` | Specifies to acknowledge filtered out 3D graphics. |

---

### `M_FACES_AND_LINES`

Inquires whether faces and lines can be picked.  This inquire type only applies to meshed point cloud graphics, arc graphics, line graphics, lines graphics, filled graphics, and graphics with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md)set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that faces and lines are not pickable. |
| `M_ENABLE` *(default)* | Specifies that faces and lines are pickable. |

---

### `M_GRAPHIC_TYPE`

Inquires which type of 3D graphic can be detected by the pick operation.

| Value | Description |
| --- | --- |
| `M_ANY` *(default)* | Specifies any type of 3D graphic. |

---

### `M_LABEL`

Inquires which label is required for a 3D graphic to be detected by the pick operation.

| Value | Description |
| --- | --- |
| `M_NO_LABEL` *(default)* | Specifies to not filter out 3D graphics according to their label. |
| `Value >= 1` | Specifies the label of the 3D graphic that is pickable. |

---

### `M_OPACITY_THRESHOLD`

Inquires the minimum opacity required for a 3D graphic to be detected by the pick operation.

| Value | Description |
| --- | --- |
| `0.0 < Value <= 100.0` *(default)* | Specifies the opacity threshold. |

---

### `M_POINTS`

Inquires whether points can be picked.  This inquire type only applies to point cloud graphics, dots graphics, and graphics with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md)set to [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that points are not pickable. |
| `M_ENABLE` *(default)* | Specifies that points are pickable. |

---

### `M_RENDER_LAYER_MASK`

Inquires on which layers a 3D graphic can be detected by the pick operation. Graphics on higher levels are always detected (and drawn before) those on lower layers.

| Value | Description |
| --- | --- |
| `M_LAYER_MASK` | Specifies to only detect 3D graphics on the specified layer. |
| `M_LAYER_UNMASK` | Specifies to detect 3D graphics on all but the specified layer. |
| `M_ANY` *(default)* | Specifies to detect 3D graphics on any layer. |
| `0b0000000000 <= Value <= 0b1111111111` | Specifies a bit mask, where the status of bits from 0 to 9 indicate which layers to mask; only 3D graphics on the layers that correspond to bits with non-zero values will be detected. |

---

### `M_SELECTION_RADIUS`

Inquires the approximate radius around the specified position for the pick operation to search.  > **Note:** Note, this setting is only available for point picking contexts.

| Value | Description |
| --- | --- |
| `Value >= 1.0` *(default)* | Specifies the selection radius, in pixels. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For inquiring about the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get information about the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

#### `M_IS_SET_TO_DEFAULT`

Inquires whether the specified inquire type is set to its default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not set to its default value. |
| `M_TRUE` | Specifies that the inquire type is set to its default value. |

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
