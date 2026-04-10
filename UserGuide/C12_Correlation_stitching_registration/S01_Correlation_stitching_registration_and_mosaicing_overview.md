---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Correlation_stitching_registration_and_mosaicing_overview
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Correlation stitching registration and mosaicing overview"
---

# Correlation-stitching registration and mosaicing overview

Besides the registration operations discussed in the previous chapter, the Aurora Imaging Library Registration module can also perform correlation-stitching registration, and can find the positional correlation between images in a common coordinate system. This information can be used for mosaicing and super-resolution.

Although the correlation-stitching registration operation accepts color images, only band 0 is used. In contrast, color images are fully supported with the mosaicing operation.

## Correlation-stitching registration

The Aurora Imaging Library Registration module allows you to find the transformations that optimally position images in a common coordinate system. The common coordinate system is referred to as the global pixel coordinate system. Registering images is necessary, for example, in industries that need to combine small images and analyze the larger one, recover the geometry of a scene, or superimpose and align two images to see the differences between them.

The following animation illustrates the process of correlation-stitching registration.

*[Image: ImageRegistration]*

The operation determines exactly how images overlap and fit together, based on the pixels in the images and some preliminary information that you supply. This information includes the rough location of all the images and the type of transformation to use to optimize their alignment. Using this information with advanced algorithms, the Aurora Imaging Library Registration module finds the best match between the overlapping regions of the images. It then calculates the transformation that needs to be applied to each image to obtain the optimal match in the overlapping regions.

## Mosaicing and super-resolution

The correlation-stitching registration operation can be used to perform mosaicing operations, which combine images to form one larger image or to create an image with improved resolution (super-resolution). It also supports converting coordinates between two of the following coordinate systems: the global pixel coordinate system, any image's pixel coordinate system, and the mosaic's coordinate system.

Using the mosaicing operation, the registration module physically combines the images used in a correlation registration, and can draw the outlines of how they fit together. This is often useful for taking images of small components that do not completely fit inside of a single image taken by your camera setup at a given resolution. This also allows you to take multiple images of large objects and add them together for a larger image combination.

Super-resolution is a process which combines images to enhance the resolution of an image. This is implemented by supplying the Registration module with multiple images of the same scene, which can be at varying zoom levels. These are used to average out noise and produce a better quality image. Super-resolution can be applied on camera setups to enhance the effective resolution of an imaging setup and produce images that can be scaled with less loss of information. You will need to first perform a correlation-stitching registration with high accuracy.
