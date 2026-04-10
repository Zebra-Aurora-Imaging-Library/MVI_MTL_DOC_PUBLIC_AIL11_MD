---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Guidelines_for_choosing_models
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Guidelines for choosing models"
---

# Guidelines for choosing models

While finding models based on geometric features is a robust, reliable technique, there are a few pitfalls which you should be aware of when allocating your models so that you choose the best possible model.

## Make sure your images have enough contrast

Contrast is necessary for identifying edges in your model source and target image with subpixel accuracy. For this reason, it is recommended that you avoid models that contain only slow gradations in grayscale values.

## Avoid poor geometric models

Poor geometric models suffer from a lack of clearly defined geometric characteristics, or from geometric characteristics that do not distinguish themselves sufficiently from other image features. These models can produce unreliable results.

*[Image: PoorGeoModel.png]*

## Be aware of ambiguous models

Certain types of geometric models provide non-unique, or ambiguous, results in terms of position, angle, or scale.

Models that are ambiguous in position are usually composed of one or more sets of parallel lines only. Such models make it impossible to establish a unique position for them. A large number of matches can be found since the actual number of line segments in any particular line is theoretically limitless.

*[Image: Screwdriver.png]*

Models that are ambiguous in scale are usually composed only of straight lines which pass through the same point; some spirals are also ambiguous in scale. Models that consist of small portions of objects should be tested to verify that they are not ambiguous in scale.

*[Image: ScaleAmbiguous.png]*

For example, a model of an isolated corner is ambiguous in terms of scale because it consists of only two straight lines that pass through the same point.

*[Image: BadScale.png]*

Symmetric models are often ambiguous in angle due to their similarity in features. For example, circles are completely ambiguous in terms of angle. Other simple symmetric models, such as squares and triangles, are ambiguous with respect to certain angles:

*[Image: TriAngle.png]*

## Nearly ambiguous models

When the major part of a model contains ambiguous features, false matches can occur because the percentage of the occurrence's edges involved in the ambiguous features is great enough to be considered a match (see [Determining what is a match](S09_Determining_what_is_a_match.md)). To avoid this, make sure that your models have enough distinct features to be found among other target features. This will ensure that only correct matches are returned as results. For example, the following model can produce false matches since the greater proportion of active edges in the model is composed of parallel straight lines rather than distinguishing curves.

*[Image: NearlyAmbiguous.png]*
