---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: 3D_Model_finder_overview
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / 3D Model finder overview"
---

# Aurora Imaging Library 3D Model Finder module

The 3D Model Finder module allows you to find occurrences of box, cylinder, rectangular plane, sphere, or surface models in a point cloud.

*[Image: 3dmod_simple_cylinder.png]*

Box, cylinder, rectangular plane, and sphere models are collectively referred to as geometric models because of their basic geometric shape. You can define geometric models from specified numeric constraints, or from one or two specified 3D geometries (except for rectangular plane models). There are two types of geometric models: nominal models and range-type models. You should define your model as a nominal model when the expected occurrences are all approximately the same size; you should define your model as a range-type model when you want to accept a variety of occurrences at different scales.

Surface models are models added from a point cloud. You can search for occurrences in any orientation, but they need to be at approximately the same scale.

*[Image: 3dmod_simple_surface.png]*

The 3D Model Finder module allows you to tailor your search to fit the requirements of your application. For example, if you know how many occurrences are present in the point cloud, you can set the maximum number of occurrences for which to search, which can decrease the search time.

Once the search is complete, you can retrieve results for each found occurrence, such as its score (which is based on its model coverage) and its center point. You can specify the sorting key to use to return results of occurrences; this is useful, for example, to retrieve the results of the highest occurrence first (the one with the highest Z-coordinate), instead of retrieving results of the occurrence with the highest score first. You can copy a group of results from the 3D model finder result buffer into an image buffer or a transformation matrix object. For geometric models, you can also copy the geometry of the occurrence into a 3D geometry object.

The module also allows you to restore a 3D model finder context from a file or memory stream, or to save a 3D model finder context to a file or memory stream.
