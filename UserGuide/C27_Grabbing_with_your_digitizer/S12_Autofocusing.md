---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Autofocusing
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Autofocusing"
---

# Auto-focusing

You can use [`MdigFocus`](../../Reference/dig/MdigFocus.md) to automatically adjust the lens motor of your camera to a position that produces optimum focus in your images. This function is primarily useful when the quality of the focus in your images is unacceptable and physically moving the grabbed object is not possible.

[`MdigFocus`](../../Reference/dig/MdigFocus.md) determines the optimum focus position by calling a user-defined function to move the lens motor to a specified initial position, grabbing an image, calculates the focus indicator of the grabbed image, and based on the focus indicator and the specified optimum focus search strategy, calculating a new position for the lens motor. The process repeats, calling the user-defined function to move the lens motor to the newly calculated position and comparing the new focus indicator with the old. The process continues repeating until the optimum focus position is found.

The focus quality of an image (known as its focus indicator) is measured by analyzing its edges. An image with good focus quality (a high focus indicator) has well-defined edges, that is, has a sharp difference in gray-levels between its object edges and its background.

By default, [`MdigFocus`](../../Reference/dig/MdigFocus.md) subsamples and filters each grabbed image before analyzing it. This makes it easier to analyze the image. If necessary, you can specify that the subsampling and/or filtering operations be skipped. Skipping these operations will result in a more accurate analysis of the image's focus quality. It is primarily useful to skip these operations when your images contain fine details since subsampling or filtering can remove these details. Note that subsampling the grabbed images increases the speed of [`MdigFocus`](../../Reference/dig/MdigFocus.md); filtering the grabbed images slows down [`MdigFocus`](../../Reference/dig/MdigFocus.md).

If necessary, you can specify that only a sub-region of the image be analyzed, by passing a child buffer to the function. This is primarily useful if there are objects at different distances within the camera's field of view. In such a case, each object will have a different optimum focus position, so you need to use a child buffer to specify the object on which to focus.

## Optimum focus search strategies

When you perform [`MdigFocus`](../../Reference/dig/MdigFocus.md), you have to specify the minimum, maximum, and starting position of the lens motor. Given these parameters, different strategies can be used to find the optimum focus position. These strategies determine how the position is updated (in which direction and by how much) between grabs. They can affect the speed and accuracy of the operation. The default search strategy used by Aurora Imaging Library is the smart-scan strategy.

### Bisection strategy

The bisection strategy ([`M_BISECTION`](../../Reference/dig/MdigFocus.md)) breaks down the given positional range, step-by-step, until it finds the optimum focus position.

*[Image: bisection.png]*

In general, the bisection strategy processes the fewest amount of images. However, it is most sensitive to noise and requires that the lens motor travel the greatest distance.

### Refocus strategy

The refocus strategy ([`M_REFOCUS`](../../Reference/dig/MdigFocus.md)) scans upward or downward until it finds the optimum focus position or until it reaches the minimum or maximum position. While scanning in one direction, if the focus indicator decreases continuously (indicating an out-of-focus condition), the focus position is returned to its starting point and scanning is started in the opposite direction. By default, if a peak in focus indicator values is found, the next two positions are scanned to make sure the peak is truly the optimum. If necessary, you can change the number of positions used to verify a peak.

*[Image: refocus.png]*

The refocus strategy is the best strategy to use when the current focus position is close to optimum.

### Scan-All strategy

The scan-all strategy ([`M_SCAN_ALL`](../../Reference/dig/MdigFocus.md)) scans each positions between the minimum and maximum and returns the position which produced the highest focus indicator.

*[Image: scan_all.png]*

The scan-all strategy is the slowest but most accurate.

### Smart-scan strategy

The smart-scan strategy ([`M_SMART_SCAN`](../../Reference/dig/MdigFocus.md)) performs three refocus searches, each with a smaller positional increment. You specify the initial positional increment; the subsequent increments are factors of the initial one. As with the refocus strategy, the default number of positions used to verify a peak is 2 but can be changed.

*[Image: smart_scanning.png]*

The smart-scan strategy is a compromise between the speed of a bisection and the accuracy of a scan-all.

## Evaluate the focus indicator

Rather than determine the optimum focus position, [`MdigFocus`](../../Reference/dig/MdigFocus.md) can be used to simply return the focus indicator value for a given image or for the image grabbed at the current lens position. To do so, use [`M_EVALUATE`](../../Reference/dig/MdigFocus.md). This focus indicator value varies according to the number of edges present and their sharpness. An image without any edges has a focus indicator value of 0. To determine the best focus position, compare the focus indicator values found with multiple calls to [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) on images of the same scene, taken with different focus distances.

Larger focus indicator values indicate that the image is more in focus. If the image is brought out of focus, the focus indicator decreases accordingly. It is important not to have saturation in the images being analyzed, because saturation can create strong artificial edges that move when the focus changes.
