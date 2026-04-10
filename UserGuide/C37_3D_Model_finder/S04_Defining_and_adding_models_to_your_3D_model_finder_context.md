---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Defining_and_adding_models_to_your_3D_model_finder_context
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Defining and adding models to your 3D model finder context"
---

# Defining and adding models to your 3D model finder context

Once you have allocated a find 3D model finder context using[`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md), you can then add a model to it using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md). The model that you add must match your 3D model finder context; for example, you cannot add a sphere model to a find cylinder 3D model finder context. Note that you can add a surface model to a find surface or planar surface 3D model finder context. You can define one model per 3D model finder context. Use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) to specify individual model search settings, as well as general settings of a 3D model finder context. Note that when you add or delete a model or change context/model settings, the 3D model finder context must be preprocessed, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md), before you can perform a search.

## Geometric-type models

You can add a geometric model (for example, [`M_BOX`](../../Reference/3dmod/M3dmodDefine.md)) to a find 3D model finder context using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_ADD`](../../Reference/3dmod/M3dmodDefine.md) or [`M_ADD_FROM_GEOMETRY`](../../Reference/3dmod/M3dmodDefine.md). The model must match the type of context; for example, you can only add a box model to a find box 3D model finder context. In addition, there are two types of geometric models: nominal and range-type.

### Nominal versus range-type geometric models

A model can be defined either nominally or using a range of accepted values. Nominal models should be used when the expected occurrences are all approximately the same size; define models as ranges when you want to accept a variety of occurrences at different scales. Note that for range-type models, the proportions of the occurrences don't have to be the same as that of the model, as long as they are in the valid range. When using a range-type model, you should define the model's range to be close to the expected range of occurrences. Defining a model with a very wide range can increase the search time.

*[Image: 3dmod_nominal-range-models.png]*

To define models nominally, call [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_BOX`](../../Reference/3dmod/M3dmodDefine.md), [`M_CYLINDER`](../../Reference/3dmod/M3dmodDefine.md), [`M_RECTANGLE`](../../Reference/3dmod/M3dmodDefine.md), or [`M_SPHERE`](../../Reference/3dmod/M3dmodDefine.md). To define a range-type model, call [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_BOX_RANGE`](../../Reference/3dmod/M3dmodDefine.md), [`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodDefine.md), [`M_RECTANGLE_RANGE`](../../Reference/3dmod/M3dmodDefine.md), or [`M_SPHERE_RANGE`](../../Reference/3dmod/M3dmodDefine.md).

### Ways to define a geometric model

There are two different ways that you can define and add a model to your 3D model finder context:

- You can define models from specified numeric constraints.
- You can define models from specified 3D geometries.

### Models defined from specified numeric constraints

A model defined from specified numeric constraints is one whose dimensions, like its radius or length, are set by numeric values you pass to [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md). You can define a model from specified numeric constraints and add it to your 3D model finder context using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_ADD`](../../Reference/3dmod/M3dmodDefine.md). For example, if you want to define a nominal sphere model ([`M_SPHERE`](../../Reference/3dmod/M3dmodDefine.md)) from specified numeric constraints, you can specify the nominal radius and a tolerance for the radius; occurrences must then have a radius within the specified tolerance of the specified nominal radius. Alternatively, if you want to define a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodDefine.md)) from specified numeric constraints, for example, you can specify the minimum and maximum length of the cylinder model; occurrences must then have lengths within this range.

### Models defined from specified 3D geometries

A model defined from specified 3D geometries is one whose dimensions, like its radius or length, are set by the dimensions of 3D geometry objects that were previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). To add a model to your 3D model finder context from specified 3D geometries, use [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_ADD_FROM_GEOMETRY`](../../Reference/3dmod/M3dmodDefine.md). For example, if you want to define a nominal sphere model ([`M_SPHERE`](../../Reference/3dmod/M3dmodDefine.md)) from specified 3D geometries, you can specify the identifier of a 3D sphere geometry object. Your model will have the same radius as that of the specified 3D geometry object. Alternatively, if you want to define a range-type cylinder model ([`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodDefine.md)) from specified 3D geometries, for example, you can specify the identifiers of two 3D cylinder geometry objects. The minimum radius and length of your model will be equal to the radius and length of the first 3D cylinder geometry object, and the maximum radius and length of your model will be equal to the radius and length of the second 3D cylinder geometry object.

The following image shows two 3D cylinder geometry objects. One cylinder has a smaller radius and length than the other. If you pass the identifiers of two 3D cylinder geometry objects to [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_ADD_FROM_GEOMETRY`](../../Reference/3dmod/M3dmodDefine.md) and [`M_CYLINDER_RANGE`](../../Reference/3dmod/M3dmodDefine.md), [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) will only return occurrences whose radii and lengths fall in between those of the two 3D cylinder geometries.

*[Image: 3dmod_2cyl.png]*

## Surface models

You can define and add surface models to a find surface 3D model finder context or a find planar surface 3D model finder context, using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_ADD_FROM_POINT_CLOUD`](../../Reference/3dmod/M3dmodDefine.md) and the source point cloud. Find planar surface 3D model finder contexts use a specialized algorithm to locate instances of planar surfaces. If your surface model has one or more planar regions, you should use a find planar surface 3D model finder context for the best accuracy. If your surface model is non-planar, you should use a find surface 3D model finder context.

The surface of the entire point cloud is used as the model. You can use a point cloud restored from a CAD file (using [`MbufImport`](../../Reference/buf/MbufImport.md)/[`MbufRestore`](../../Reference/buf/MbufRestore.md)). Note that the CAD file must be in the PLY or STL file format. Alternatively, you can grab a point cloud using a 3D sensor and remove background objects from the scan (for example, using [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) to crop points and/or [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) to remove a background plane from the point cloud). Typically, a CAD model contains all surfaces of the object. However, a surface model obtained from a 3D sensor might require multiple scans to fully represent all sides. For example, if the top surface of an object differs from the bottom surface, you must scan twice to capture each opposing side. Resolution compensation might be required if the model and target scene don't have the same scanning conditions. For more information, see [Model and target point cloud resolution](S08_Search_settings_specific_to_surface_models.md).

*[Image: 3dmod_surface_model_point_cloud.png]*

A surface occurrence's found position is determined by the model's reference axis; position results return the coordinates of the model's reference axis origin transformed at the model occurrence. By default, the origin of the model's reference axis is at the origin of the working coordinate system of the original point cloud from which the surface model was defined. This does not necessarily correspond to the center of the model. When adding the model using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md), you can move the origin of the working coordinate system to the required position. If you want to change the position of the reference axis origin later, you can do so using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SET_POSITION_...`](../../Reference/3dmod/M3dmodControl.md).

When adding the model using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) (or later using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REMOVE_OUTLIERS`](../../Reference/3dmod/M3dmodControl.md)), you can also specify that outliers are removed. When enabled, Aurora Imaging Library removes outliers such as clusters of points around the perimeter of the model. Enable outlier removal if the model contains stray points, especially if refine registration ([`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodCopy.md)) is enabled. The default for this control type is set when adding the surface model to the context using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_SURFACE`](../../Reference/3dmod/M3dmodDefine.md).

Note that you should not enable outlier removal if your model was defined from a CAD-sourced point cloud (using [`MbufImport`](../../Reference/buf/MbufImport.md)/[`MbufRestore`](../../Reference/buf/MbufRestore.md)) since parts of the model can be removed, which can cause the search to fail/return false positives. Also note that you can use [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) to remove outlier points from the source point cloud before adding the model to the find surface or planar surface 3D model finder context.

*[Image: 3dmod_surface_model_outliers_removed.png]*

Aurora Imaging Library uses the features of the surface model and the target point cloud to find occurrences, but by default, it doesn't do any fitting. To get more accurate score and pose information, you should enable refine registration ([`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodCopy.md)). When enabled, 3D registration is internally performed between the model and each of the found occurrences. See [Fit refinement](S08_Search_settings_specific_to_surface_models.md) to learn more.
