---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Image_quality
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Image quality"
---

# Image quality

Prior to manipulating and extracting information from an image, many applications require that you obtain the best possible digital representation of it. Several factors affect the quality of an image. These include:

- **Random noise**. There are two main types of random noise:
  - **Gaussian noise**. When this type of noise is present, the exact value of any given pixel is different for each grabbed image; this type of noise adds to or subtracts from the actual pixel value.
  - **Salt-and-pepper noise (also known as impulse or shot noise)**. This type of noise introduces pixels of arbitrary values (usually high-frequency values) that are generally noticeable because they are completely unrelated to the neighboring pixels.
  Random noise can be caused, for example, by the camera or digitizer because electronic devices tend to generate a certain amount of noise. If the images were transmitted, the distance between the sending and the receiving devices also magnifies the random noise problem because of interference.
- **Systematic noise**. Unlike random noise, this type of noise can be predicted, appearing as a group of pixels that should not be part of the actual image. This can be caused, for example, by the camera or digitizer or by uneven lighting. If the image was magnified, microscopic dust particles, on either the object or a camera lens, can appear to be part of the image.
- **Distortions**. Distortions appear as geometric transforms of the actual image. These can be caused, for example, by the position of the camera relative to the object (not perpendicular), the curvature in the optical lenses, or a non-unity aspect ratio of an acquisition device.
- **Focus**. Focus issues result in objects or features of objects appearing blurry in the image. These issues can be caused, for example, by grabbing the image such that the objects are at different focus depth levels, given the proximity of the camera. An image with focus issues can be more difficult to process with accurate results, since the edges and other features present within the image appear less well-defined.

## Techniques to improve images

Most interference problems cannot be adjusted very easily at the source; therefore, preprocessing will probably be required to improve the image as much as possible, without affecting the information that you are seeking. There are several techniques that you can use to improve your image:

- Grab the object of interest several times, averaging each image frame with the previous. This technique is generally effective on Gaussian random noise.
- Apply a low-pass spatial filter to your image to reduce Gaussian random noise and systematic noise with small scale variations. This technique replaces each pixel with a weighted sum of its neighborhood.
- Apply a median filter to your image to reduce salt-and-pepper noise. This technique replaces each pixel with the median pixel value of its neighborhood.
- Perform a morphological opening operation to remove small particles and break isthmuses between objects in your image.
- Perform a morphological closing operation to remove small holes in objects.
- Make sure that your camera and digitizer grab the image with square pixels (that is, a 1:1 aspect ratio), to reduce object-shape distortions. If this is not possible or does not correct the problem, you can resize the image, using [`MimResize`](../../Reference/im/MimResize.md); alternatively, you can calibrate your camera setup and then associate this camera calibration with your image, using the Aurora Imaging Library Camera Calibration module.
- Apply a dead-pixel correction to your image to replace dead-pixels with valid image data based on neighboring pixels.
- Apply a gain and offset correction to remove electrical bias, thermal agitation, and sensitivity variations from your camera's charge-coupled device (CCD).

> **Note:** Note that, some of these techniques are not possible using Aurora Imaging Library Lite.
