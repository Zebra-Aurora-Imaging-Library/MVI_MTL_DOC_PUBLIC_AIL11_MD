---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: Steps_to_display_3D_data
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / Steps to display 3D data"
---

# Steps to display 3D data

The following steps provide a typical methodology to show 3D data in an Aurora Imaging Library 3D display:

1. Allocate a 3D display on the same system as your 3D-displayable point cloud container or depth map image buffer/container, using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).
2. Select the 3D-displayable point cloud container or depth map image buffer/container to the 3D display, using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md). If you select a point cloud or depth map container that is not natively 3D-displayable, Aurora Imaging Library will compensate and internally convert it to a 3D-displayable format if possible. For more information about what makes a container 3D-displayable, see [Preparing a container for display or processing](../C41_3D_Containers/S04_Preparing_a_container_for_display_or_processing.md). If the point cloud container ceases to be 3D-displayable (for example, because its range component is removed), it will not be shown in the display until it becomes 3D-displayable again.
   > **Note:** You can also select an empty container with the [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)attribute (in which you plan to grab 3D data) to the display.
3. Repeat step 2 for all point cloud containers or depth map image buffers/containers that you want to display. In this case, call [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md) with [`M_ADD`](../../Reference/3ddisp/M3ddispSelect.md) to prevent the containers or image buffers you have already added to the 3D display's 3D graphics list from being removed.
4. Optionally, change the view of the display either using the keyboard/mouse controls or using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md).
5. If you want to annotate the 3D display, inquire the identifier of its internal 3D graphics list, using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md). Then, add graphic objects to the 3D graphics list using the functions in the [`M3dgra...`](../../Reference/3dgra/M3dgraBox.md) module. Alternatively, you can add a graphics list to the display, using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md). This is useful, for example, to add the same annotations to multiple displays; changes made to this graphics list are reflected in every display in which it is shown.
6. When you no longer want to show a point cloud container or depth map image buffer/container, remove it from the 3D display's internal 3D graphics list using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md) with [`M_REMOVE`](../../Reference/3ddisp/M3ddispSelect.md).
7. When you no longer want to show the 3D display, close it using[`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md) with [`M_CLOSE`](../../Reference/3ddisp/M3ddispSelect.md).
8. When the 3D display is no longer required, free it using [`M3ddispFree`](../../Reference/3ddisp/M3ddispFree.md), unless [`M_UNIQUE_ID`](../../Reference/3ddisp/M3ddispAlloc.md) was specified during allocation.
