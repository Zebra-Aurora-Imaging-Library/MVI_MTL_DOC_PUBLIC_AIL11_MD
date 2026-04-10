---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Masking_the_model
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Masking the model"
---

# Masking the model

Often your model contains regions that you need Aurora Imaging Library to ignore when searching for the model in the target image. These regions might be noise pixels or simply regions that have nothing to do with what you are searching.

For instance, in a machine guidance application, a mechanical device might need to know where mounting holes are located on a circuit board so that screws can be properly inserted. In this case, a mounting hole would be the model and the circuit board would be the target image.

*[Image: target.png]*

In the above image, the model contains too much of the actual board; it might not match holes in different areas of the board.

In such cases, parts of the model can be masked by setting pixel values in certain regions to "don't care" pixels. Aurora Imaging Library then ignores these regions when searching for occurrences of the model.

In our example, you would need to mask the edges of the model, as follows:

*[Image: mask1.png]*

The unmasked region of the model now more closely resembles the pattern for which to search; it is more circular in shape and contains little of the actual board.

To create a mask:

1. Allocate an image that is of the same size as the model image. This new image is the **mask image**.
2. Clear the mask image to zero.
3. Set the "don't care" pixels in the mask image to a non-zero value. There are numerous ways of doing this. For example, you can use one of the [`Mgra...`](../../Reference/gra/MgraAlloc.md) functions.
4. Call [`MpatMask`](../../Reference/pat/MpatMask.md), specifying the mask image. This function sets the model's "don't care" pixels.
5. View the mask using [`MpatDraw`](../../Reference/pat/MpatDraw.md) with [`M_DRAW_DONT_CARE`](../../Reference/pat/MpatDraw.md).

When you change the "don't care" pixels of a model, you should preprocess the search model again.
