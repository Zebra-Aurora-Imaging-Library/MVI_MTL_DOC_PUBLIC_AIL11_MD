---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Registration
section: Basic_concepts
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / registration / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Registration module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Registration module are:

- **Albedo image**. An image that represents a measure of the reflectance of an object's surface.
- **Confidence map**. An image where the gray value of each pixel represents how reliable the index map is at that location.
- **Depth of field**. The tolerated distance from the focus distance of a camera setup in which objects are considered in focus.
- **Index map**. An image where the gray value of each pixel represents the index of an image, at which the pixel is most in focus.
- **Intensity map**. An image where the gray value of each pixel is the intensity of the corresponding pixel in the image where that pixel is found to be most in focus.
- **Mosaicing**. The process of combining many smaller images to form one larger image, using the results of the registration calculation.
- **Registration**. The registration of images involves the fusion of multiple images of a common scene to obtain some resulting image.
- **Registration element**. A data structure that stores the settings used to control the registration of a specific image. Registration elements are located in the registration context.
- **Registration element's image**. The image whose index in the image array is the same as the index of a particular registration element. The registration of an image is controlled by the contents of its associated registration element.
- **Registration context**. An Aurora Imaging Library object that stores the registration information and each individual registration element.
