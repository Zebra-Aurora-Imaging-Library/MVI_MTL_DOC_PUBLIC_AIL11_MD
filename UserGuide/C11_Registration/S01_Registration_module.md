---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Registration
section: Registration_module
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / registration / Registration module"
---

# Aurora Imaging Library Registration module

The Aurora Imaging Library Registration module can perform four different types of registration operations. The first type is referred to as correlation-stitching registration, and can find the positional correlation between images in a common coordinate system. This information can be used for mosaicing and super-resolution. The second type, referred to as extended depth of field registration, or fusion, finds and uses the focused objects in images of the same scene to produce one image that combines all of the objects in focus. The third type is referred to as depth-from-focus registration; it computes an index map of a scene whereby each index value corresponds to a specific depth based on images of the same scene taken at different focus distances. The fourth type is referred to as photometric stereo registration; it uses multiple lighting orientations to enhance details, and detect defects from images of objects with reflective surfaces.

This chapter discusses the last three types of registration operations. For information on performing a correlation-stitching registration or mosaicing operation, see [Correlation stitching registration and mosaicing](../C12_Correlation_stitching_registration/ChapterInformation.md).

Extended depth of field operations, depth-from-focus operations, and photometric stereo operations require 1-band images; if images containing more than 1 band are used for these operations, an error occurs.

The Registration module does not take into account the camera calibration of images.

## Extended depth of field registration

A camera setup with a given depth of field does not always capture all the information in focus. You can instead take multiple images of the scene at different focus distances, and use the Aurora Imaging Library Registration module to perform an extended depth of field registration to create a single image with the objects in focus. This simulates a camera setup with an extended depth of field, and can be very useful for overcoming camera and lens limitations. Microscopy, for example, is one area where the possible depth of field is very shallow.

The images in an extended depth of field registration must overlap completely; they should be taken with exactly the same camera setup with different focus distances.

## Depth-from-focus registration

When grabbing an image of a scene, you might need to perform 3D analysis of the objects grabbed in the scene. The Aurora Imaging Library Registration module can generate an image that conveys depth information, from a set of two-dimensional images of the same scene taken at different focus distances. Essentially, each pixel in the resulting image corresponds to the index of the image in the set where the corresponding pixel appears most in focus. The resulting image is referred to as an index map and the operation is referred to as a depth-from-focus registration operation.

The source images used in a depth-from-focus registration must overlap completely; they should be taken with exactly the same camera setup, with only the focus distance differing.

## Photometric stereo registration

An image taken with a camera setup using a single light source does not always correctly capture all the details or defects on an object's surface. You can instead take multiple images of the same scene using different lighting orientations, and use the Aurora Imaging Library Registration module to create a single enhanced image that reveals raised and recessed features, surface reflectance changes, and scratches or other flaws that are not apparent with an image taken with only one light source. Photometric stereo registration operations use material reflectance properties and the surface curvature of objects when calculating the resulting enhanced image. Operations available include computing the albedo image, Gaussian curvature image, mean curvature image, local shape image, local contrast image, and local texture image.

The following image illustrates the process of photometric stereo registration.

*[Image: Photometrics_Overview_threestatic.png]*

The images in a photometric stereo registration must overlap completely; they should be taken with exactly the same camera setup, with different lighting directions.
