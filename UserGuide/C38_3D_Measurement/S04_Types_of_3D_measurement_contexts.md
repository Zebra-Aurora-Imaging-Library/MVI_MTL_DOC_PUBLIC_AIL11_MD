---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: Types_of_3D_measurement_contexts
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / Types of 3D measurement contexts"
---

# Types of 3D measurement contexts

Using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md), you can allocate one of three types of 3D measurement contexts to find markers: a path 3D measurement context ([`M_FIND_MARKER_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md)), a template 3D measurement context ([`M_FIND_MARKER_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md)), or a profile 3D measurement context ([`M_FIND_MARKER_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md)). You can also allocate a fit 3D measurement context ([`M_FIT_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md)) to fit a 3D geometry to markers found in a template's perpendicular profiles.

## Path 3D measurement context

You can use a path 3D measurement context to find a specified number of markers along a path. This type of context is useful when you want to find an object but you don't know its exact location. You must provide the path along which to extract a profile, using [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md) with a line geometry object. The path should cross through the possible locations of your object. After extracting the profile along the path, the 3D Measurement module will find the specified number of markers in the profile.

## Template 3D measurement context

You can use a template 3D measurement context to find the markers that can form the specified template. A template represents the location of an expected edge or ridge in the depth map. This type of context is useful when you want to verify the location or shape of an object. For example, if you want to verify that the edge or ridge of an object is at a particular location and angle, you can use a template 3D measurement context and define a template with the expected pose, using [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md) with a line geometry object. The 3D Measurement module will extract profiles at regular intervals perpendicular to the template and find a single marker in each profile. You will then be able to fit a 3D geometry to the found markers. See [Fit 3D measurement context](S04_Types_of_3D_measurement_contexts.md) for more information.

*[Image: 3dmeas_fit_geometry.png]*

## Profile 3D measurement context

You can use a profile 3D measurement context to find markers in each row of a depth map, where each row is interpreted as a single profile. The 3D Measurement module can find the specified number of markers along each profile. This type of context is useful when you want to analyze the entire depth map.

A profile 3D measurement context contains a default template. The default template in a profile 3D measurement context is different from an explicitly defined template in a template 3D measurement context. The default template is inherent to the supplied profiles, such that it is the theoretical template that would result in these profiles. Note that if you need to fit a geometry to the found markers to compare their location to that of the template, you must use a template 3D measurement context.

## Fit 3D measurement context

You can use a fit 3D measurement context to fit a 3D geometry to markers found in a template's perpendicular profiles. After finding the markers, using [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) with a template 3D measurement context, you can fit a 3D geometry to them, using [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md). Note that you can also use [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to find the markers and perform the fit simultaneously, in which case, all possible markers are extracted and the one in each profile that provides the best fit is kept. You can then use [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) to retrieve information about the fitted 3D geometry and compare it to the defined template.
