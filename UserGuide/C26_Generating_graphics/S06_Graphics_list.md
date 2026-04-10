---
doctype: UserGuide
part: "2D related information"
chapter: Generating_graphics
section: Graphics_list
module_tag: gra
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / graphics / Graphics list"
---

# 2D graphics list

A 2D graphics list is an Aurora Imaging Library object that allows you to temporarily store graphics in vector (mathematical) form for later drawing or for defining the geometry of another Aurora Imaging Library object (for example, the ROI of an image buffer).

When annotating an image buffer without a 2D graphics list, the graphic is drawn destructively in that image, and is raster-based. That is, the drawing changes the image's pixel values along the specified graphic. Once graphics are drawn in this way, they can only be modified on a pixel-by-pixel basis. However, if you draw a graphic in a 2D graphics list, the graphic is added to the list in vector (mathematical) form.

Although drawing graphics destructively into an image buffer with a graphics function (such as [`MgraLine`](../../Reference/gra/MgraLine.md)) can seem faster, in certain cases, it can be preferable to first add the graphics to a list, and then draw them, using [`MgraDraw`](../../Reference/gra/MgraDraw.md). By doing so, you can:

- Permit the graphic to be zoomed without pixelization effects and changes in line thickness when rendered on a display that is being zoomed.
- Have more efficient code by adding all the graphics to a memory efficient object (2D graphics list), and then using those graphics to annotate numerous images. If required, you can use [`MgraControlList`](../../Reference/gra/MgraControlList.md) to modify the graphics before each call to [`MgraDraw`](../../Reference/gra/MgraDraw.md).
- Add the graphics to the list once, using world units, and then annotate numerous images, each with their own camera calibration context. For example, if two cameras have taken an image in the same absolute coordinate system, and you wish to annotate both images at the same world position, you can define the graphics in world units and the graphics will be mapped, using the camera calibration context of each image, to the correct area in the pixel coordinate system.
- Add the graphics to the list in world units before you have the calibrated image.
- Create an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) region of interest (ROI) for an image buffer, which is then used by some processing functions (for example, [`McodeRead`](../../Reference/code/McodeRead.md)).
- Interactively create and modify graphics from a display.

To allocate a 2D graphics list, use [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). To add graphics to the list, use one of the [`Mgra...`](../../Reference/gra/MgraAlloc.md) graphics functions (for example, [`MgraDot`](../../Reference/gra/MgraDot.md)) or use the [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) function of a processing or analysis module; however, pass the 2D graphics list identifier instead of an image buffer identifier.

You define the graphic itself as if it were being drawn in an image. For example, a dot is always defined with an X- and Y-position, regardless of whether you are drawing it in an image or adding it to a 2D graphics list.

> **Code example:** [userguide.generating_graphics.graphic_list01](userguide.generating_graphics.graphic_list01)

For more information about how graphics are defined and drawn, see [Drawing graphics](S04_Drawing_graphics.md).

> **Note:** Some draw operations of [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) functions cannot draw in a 2D graphics list or have restrictions when drawing in a 2D graphics list. For more information, see the relevant [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) function in the Aurora Imaging Library Reference.

If required, you can also use [`MgraDraw`](../../Reference/gra/MgraDraw.md) to draw the graphics, contained in a list, destructively in an image all at once.

> **Code example:** [userguide.generating_graphics.graphic_list04](userguide.generating_graphics.graphic_list04)

When you add graphics to the list, the graphics inherit the current settings of the specified 2D graphics context; however, there is no link between the 2D graphics list and the 2D graphics context. Graphics already in the list are not affected by changes made to the context. For example, if you have a 2D graphics list that contains blue graphics, but you change the 2D graphics context so that graphics can have a red color, the graphics already in the list will retain their blue color, and only graphics added after the change to the context will be red.

Graphics can be accessed from the 2D graphics list using either an index or label value. Both values are attributed to the graphic when it is added to the list. In the case of the index value, it refers to the graphic's position in the 2D graphics list, and can change if graphics are moved or deleted. In the case of the label value, it is a label given to the graphic when added to the list and will not change, even if the order of the graphics in the list changes due to graphics being deleted or moved. You can retrieve a graphic's label using [`MgraInquireList`](../../Reference/gra/MgraInquireList.md) with [`M_LAST_LABEL`](../../Reference/gra/MgraInquireList.md) immediately after adding the graphic to the list. Note that when using multiple threads, [`M_LAST_LABEL`](../../Reference/gra/MgraInquireList.md) will return the label of the last graphic added to the list for the thread on which [`MgraInquireList`](../../Reference/gra/MgraInquireList.md) is called.

To adjust the settings of graphics in the list, use [`MgraControlList`](../../Reference/gra/MgraControlList.md). To inquire current 2D graphics list settings, use [`MgraInquireList`](../../Reference/gra/MgraInquireList.md). To remove graphics from the list, use [`MgraClear`](../../Reference/gra/MgraClear.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_DELETE`](../../Reference/gra/MgraControlList.md). To copy graphics from one 2D graphics list to another, use [`MgraCopy`](../../Reference/gra/MgraCopy.md). To move graphics in a 2D graphics list, use [`MgraCopy`](../../Reference/gra/MgraCopy.md) with [`M_MOVE`](../../Reference/gra/MgraCopy.md).

The following example demonstrates how to add graphics to a 2D graphics list, and how to change its settings.

> **Code example:** [userguide.generating_graphics.graphic_list02](userguide.generating_graphics.graphic_list02)

## Annotating the display with a 2D graphics list

Typically, the graphics in a 2D graphics list are used to annotate the display non-destructively, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md); this associates the display with the 2D graphics list. By default, modifications to the graphics within the list are immediately reflected in the non-destructive annotation of the display (refresh not required). To change this behavior, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_UPDATE_GRAPHIC_LIST`](../../Reference/disp/MdispControl.md).

> **Note:** Only one 2D graphics list can be associated with a display. If you associate a 2D graphics list with a display that already has been associated with one, the previous 2D graphics list is disassociated from the display and the new one is associated.

Since the graphics in the list are in vector form, the zooming behavior of a display with 2D graphics list annotations will be different than if you had drawn graphics destructively in the image selected to the display. For example, when zooming a display with 2D graphics list annotations, there is no loss of clarity, missing graphic elements, or changes in line thickness (which, as mentioned earlier in the chapter, is one pixel by default, and can be modified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md)). The following example shows how a 2D graphics list is used to annotate the display.

> **Code example:** [userguide.generating_graphics.graphic_list03](userguide.generating_graphics.graphic_list03)

For more information, see [Using a 2D graphics list](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md).
