---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Adjusting_image_focus
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Adjusting image focus"
---

# Adjusting image focus

If your image is not in focus, you might want to adjust it to achieve better results when processing. Focus issues occur when the focus distance of the camera results in some or all portions of the image being out of focus, and appearing blurry. As unfocused objects in images often appear with poorly-defined edges and other features, there are a range of techniques to adjust for focus issues, both before and after the image is grabbed.

## Auto-focusing

If your lens can be adjusted using a motor, you can use [`MdigFocus`](../../Reference/dig/MdigFocus.md) to adjust the lens motor of the camera to a position that produces optimum focus in your grabbed images. This process is known as auto-focusing. Auto-focusing can be used in any imaging setup in which your lens can be adjusted by a motor, and is most useful in an imaging setup where precise measurements of objects in images are required, and frequent focus adjustments are required to grab acceptable images. In some cases, it might be impossible to have all objects in a grabbed image appear in focus, because having some objects in focus causes others to be outside the tolerated distance of the focus distance of your lens. In these cases, you can create a child buffer (or an ROI) that encompasses only the object in focus and operate only on the child buffer. Then, adjust the focus for a different area, grab, and move the child buffer to a new area in focus. Alternatively, you can try the other suggestions in this section.

For more information on auto-focusing, see [Auto-focusing](../C27_Grabbing_with_your_digitizer/S12_Autofocusing.md).

## Edge enhancement

You can improve the focus of your image by sharpening the edges present in the image. Sharpening the edges allows you to accentuate the details of the image.

For more information on edge enhancement, see [Edge enhancement](S14_Enhancing_and_detecting_edges.md).

## Extending your depth of field

At a given focus distance for a grab, the entire image is not always in focus, resulting in blurry or poorly-defined objects in the image. This is due to objects resting outside of the tolerated distance, or depth of field from that focus distance.

You can use [`MregCalculate`](../../Reference/reg/MregCalculate.md) to extend your depth of field to correct for focus issues in images; this function uses the best possible focus sections of multiple images of the same objects, taken at different focus levels. This emulates a larger depth of field, such that all objects of interest appear in focus, and is especially useful when you need to grab and process images of multiple objects at different depths.

For more information on extending the depth of field of an image, see [Extending your depth of field](../C11_Registration/S03_Extending_your_depth_of_field.md).
