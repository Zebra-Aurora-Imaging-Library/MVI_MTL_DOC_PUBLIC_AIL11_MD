---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Basic_concepts_when_performing_correlation_stitching
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Basic concepts when performing correlation stitching"
---

# Basic concepts for correlation-stitching registration

The basic concepts and vocabulary conventions for the Aurora Imaging Library Registration module are:

- **Global pixel coordinate system**. The coordinate system in which the transformations, resulting from the registration calculation, position the images.
- **Mosaicing**. The process of combining many smaller images to form one larger image, using the results of the correlation-stitching registration calculation.
- **Overlapping region**. The region in images that share the same coordinates in a common coordinate system.
- **Reference image**. The image with respect to whose coordinate system you set the rough location of another image. Each image must be positioned with respect to either a reference image's pixel coordinate system or the global pixel coordinate system.
- **Registration**. The registration of images involves the fusion of multiple images of a common scene to obtain some resulting image.
- **Registration element**. A data structure that stores the settings used to control the registration of a specific image. Registration elements are located in the registration context.
- **Registration element's image**. The image whose index in the image array is the same as the index of a particular registration element. The registration of an image is controlled by the contents of its associated registration element.
- **Registration context**. An Aurora Imaging Library object that stores the registration information and each individual registration element.
- **Rough location**. Approximate location of an image in its reference image's pixel coordinate system. The registration calculation finds the transformation that will convert the image's rough location into its optimal location in the global pixel coordinate system.
- **Transformation matrix**. A matrix that maps a set of coordinates from a source coordinate system into a destination coordinate system.
