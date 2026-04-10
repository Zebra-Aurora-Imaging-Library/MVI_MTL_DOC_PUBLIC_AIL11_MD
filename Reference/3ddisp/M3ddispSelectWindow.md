---
doctype: Reference
module: 3ddisp
function: M3ddispSelectWindow
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispSelectWindow"
---

# M3ddispSelectWindow

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

> Add a point cloud graphic, generated from a point cloud container or depth map image buffer/container, or add a graphics list to a 3D display, and associate the 3D display with a user-defined window. The function can also remove them from the 3D display.

## Syntax

```c
AIL_INT64 M3ddispSelectWindow(
    AIL_ID            Disp3dId,           //out
    AIL_ID            AilObjectId,        //in
    AIL_INT64         Option,             //in
    AIL_INT64         ControlFlag,        //in
    AIL_WINDOW_HANDLE ClientWindowHandle  //in
)
```

## Description

This function can add a point cloud graphic, generated from a point cloud container or depth map image buffer/container, to a 3D display's internal graphics list or can add a graphics list to the 3D display. It can associate and show the 3D display in a user-defined window. This function can also remove a specified point cloud graphic or graphics list previously added to the 3D display.

> **Note:** This function never affects the user-defined window's open or closed status; the function only affects the 3D display's association with it.

The specified container or image buffer is linked to the point cloud graphic, which is added to the 3D display's internal 3D graphics list.

> **Note:** Any subsequent changes made to the image buffer or components of the container will be shown in the 3D display. The point cloud container or depth map image buffer/container must be allocated for display ([`M_DISP`](../../Reference/buf/MbufAllocContainer.md)); otherwise, no point cloud graphic will be shown. You can determine whether a container is currently 3D-displayable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with[`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md).

By default, this function removes any point cloud graphics already selected to the 3D display. To show multiple point cloud graphics in the 3D display simultaneously, you can either specify the[`M_ADD`](../../Reference/3ddisp/M3ddispSelectWindow.md)option, or use [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md). To remove only a single container or image buffer from the 3D display, specify the[`M_REMOVE`](../../Reference/3ddisp/M3ddispSelectWindow.md)option, or use [`M3dgraRemove`](../../Reference/3dgra/M3dgraRemove.md). Note that you can use [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) with [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md) to display an unlinked point cloud graphic; in this case, the 3D display is not updated dynamically.

If you want to annotate the 3D display, inquire either the identifier of its internal 3D graphics list or the identifier of a 3D graphics list previously added to the display, using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md). Then, add graphic objects to the 3D graphics list using the functions in the [`M3dgra...`](../../Reference/3dgra/M3dgraBox.md) module.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call functions in this module concurrently from multiple threads on the same Aurora Imaging Library 3D display ([`Disp3dId`](../../Reference/3ddisp/M3ddispSelectWindow.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data. Point cloud containers and depth map image buffers/containers are **not thread-safe** and must only be accessed by one thread at a time. You must ensure the source point cloud container or depth map image buffer/container is not modified from another thread while the add is being performed. Note that this does not impact subsequent modifications to the point cloud container or depth map image buffer/container as long as they are modified from one thread at a time.

## Parameters

### `Disp3dId` *(out, AIL_ID)*

Specifies the identifier of the 3D display, previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

### `AilObjectId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library identifier of the 3D-displayable container, fully corrected depth map image buffer, or graphics list to add to the display.

### `Option` *(in, AIL_INT64)*

Specifies the action to perform on the specified 3D display.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ClientWindowHandle` *(in, AIL_WINDOW_HANDLE)*

Specifies the window handle of the user-defined window or child window. This window must have a window handle of type _HWND_ or X11. For example, the Windows API functions can create a window with an _HWND_ handle, and third-party functions (like Qt) can create an X11 window handle. If this parameter is set to zero, this function behaves like [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md).

## Parameter Associations

### For specifying the container, image buffer, or 3D graphics list and what operation to perform

---

### `M_NULL`

Specifies no container, image buffer, or 3D graphics list.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLOSE` | Specifies to stop showing the 3D display and to disassociate it from the user-defined window.  > **Note:** This setting is not available for [`M_WEB`](../../Reference/3ddisp/M3ddispAlloc.md) displays. |
| `M_OPEN` | Specifies to associate the 3D display with the user-defined window and to show the 3D display when the window is open.  > **Note:** This setting is not available for [`M_WEB`](../../Reference/3ddisp/M3ddispAlloc.md) displays. |
| `M_REMOVE` | Specifies to remove all point cloud graphics from the 3D display's internal 3D graphics list. |
| `M_SELECT` *(default)* | Specifies to remove all point cloud graphics from the 3D display's internal 3D graphics list and to disassociate the 3D display from the user-defined window. |

---

### `3D-displayable container identifier`

Specifies the Aurora Imaging Library identifier of the 3D-displayable point cloud or depth map container, for adding or removing its linked point cloud graphic to/from the specified 3D display's internal 3D graphics list. You can inquire whether a container is 3D-displayable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md).  > **Note:** If the container is only 3D-displayable with compensation, Aurora Imaging Library will perform an internal conversion each time the container is modified. For maximum performance, you should convert the container to a format that is natively 3D-displayable (using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)with a destination container that has the attribute [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)). [`M3dimFix`](../../Reference/3dim/M3dimFix.md) with [`M_RANGE_FINITE`](../../Reference/3dim/M3dimFix.md)+[`M_NORMALS_FINITE`](../../Reference/3dim/M3dimFix.md)+[`M_MESH_VALID_POINTS`](../../Reference/3dim/M3dimFix.md) is internally performed for point cloud graphics. Just like automatic compensation, this does not modify the source point cloud.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADD` | Specifies to add the point cloud graphic, linked to the specified container or depth map image buffer, to the 3D display's internal 3D graphics list. If the specified container or image buffer is already linked to a point cloud graphic in the 3D graphics list, no error is generated. The function will return the label of the existing point cloud graphic instead. If, however, the existing point cloud graphic is unlinked (that is, it was added using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) with [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md)), using this option adds a duplicate point cloud to the 3D graphics list. This does not affect the association of the 3D display with the user-defined window. Note that to add multiple linked point cloud graphics, use[`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md). |
| `M_REMOVE` | Specifies to remove, from the display's internal graphics list, all point cloud graphic labels corresponding to the specified container. An error is generated if the specified container is not linked to a point cloud graphic in the internal 3D graphics list. You can remove unlinked point cloud graphics from the display's internal graphics list using [`M3dgraRemove`](../../Reference/3dgra/M3dgraRemove.md). |
| `M_SELECT` *(default)* | Specifies to add the specified container's point cloud graphic to the 3D display's internal 3D graphics list, remove all other point cloud graphics from the internal 3D graphics list, and associate the 3D display with the user-defined window. Note, an error will occur if a user-added graphics list has already been added to the display. |

---

### `3D graphics list identifier`

Specifies the Aurora Imaging Library identifier of the 3D graphics list to add to or remove from the specified 3D display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADD` | Specifies to add the 3D graphics list to the display. This does not affect the association of the 3D display with the user-defined window. |
| `M_REMOVE` | Specifies to remove the 3D graphics list from the display. Note, the default graphics list cannot be removed. If the specified 3D graphics list is not associated with the display, an error will occur. |
| `M_SELECT` *(default)* | Specifies to remove all 3D graphics lists (excluding the default graphics list) and add the specified graphics list to the display. This does not affect the association of the 3D display with the user-defined window. Note, if the default graphics list is not empty, an error will occur. |

---

### `Empty container identifier`

Specifies the Aurora Imaging Library identifier of an empty container with the [`M_DISP`](../../Reference/buf/MbufAllocContainer.md) attribute. This is useful for specifying a container into which you will subsequently grab 3D data.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADD` | Specifies to add the point cloud graphic, linked to the specified container or depth map image buffer, to the 3D display's internal 3D graphics list. If the specified container is already linked to a point cloud graphic in the 3D graphics list, no error is generated. The function will return the label of the existing point cloud graphic instead. If, however, the existing point cloud graphic is unlinked (that is, it was added using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) with [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md)), using this option adds a duplicate point cloud to the 3D graphics list. Since the specified container doesn't initially contain a point cloud or depth map, a point cloud graphic without any points is added. This does not affect the association of the 3D display with the user-defined window. |
| `M_REMOVE` | Specifies to remove the point cloud graphic, linked to the specified container, from the 3D display's internal 3D graphics list. An error is generated if the specified container is not linked to a point cloud graphic in the internal 3D graphics list. |
| `M_SELECT` *(default)* | Specifies to add the specified container's point cloud graphic to the 3D display's internal 3D graphics list, remove all other point cloud graphics from the internal 3D graphics list, and associate the 3D display with the user-defined window. Since the specified container doesn't initially contain a point cloud or depth map, a point cloud graphic without any points is added. |

---

### `Image buffer identifier`

Specifies the Aurora Imaging Library identifier of an image buffer, with the [`M_DISP`](../../Reference/buf/MbufAlloc2d.md)attribute, for adding or removing its linked point cloud graphic to/from the specified 3D display's internal 3D graphics list. This image buffer will be displayed as a point cloud. The image buffer must be a fully-corrected depth map ([`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md) returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADD` | Specifies to add the point cloud graphic, linked to the specified depth map image buffer, to the 3D display's internal 3D graphics list. If the specified image buffer is already linked to a point cloud graphic in the 3D graphics list, no error is generated. The function will return the label of the existing point cloud graphic instead. If, however, the existing point cloud graphic is unlinked (that is, it was added using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) with [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md)), using this option adds a duplicate point cloud to the 3D graphics list. This does not affect the association of the 3D display with the user-defined window. |
| `M_REMOVE` | Specifies to remove the point cloud graphic, linked to the specified depth map image buffer, from the 3D display's internal 3D graphics list. An error is generated if the specified image buffer is not linked to a point cloud graphic in the internal 3D graphics list. |
| `M_SELECT` *(default)* | Specifies to add the specified image buffer's point cloud graphic to the 3D display's internal 3D graphics list, remove all other point cloud graphics from the internal 3D graphics list, and associate the 3D display with the user-defined window. |

## Return Value

**Type:** `AIL_INT64`

Returns the label of the point cloud graphic added to the 3D graphics list of the 3D display.
