---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Types_of_Model_Finder_contexts
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Types of Model Finder contexts"
---

# Types of Model Finder contexts

Using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md), you can allocate one of six types of Model Finder contexts: one that uses a general geometric search algorithm ([`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md)), one that uses a controlled geometric search algorithm ([`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md)), one that uses a circular shape (with deformation tolerance) matching algorithm ([`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md)), one that uses an elliptical shape (with deformation tolerance) matching algorithm ([`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md)), one that uses a rectangular shape (with deformation tolerance) matching algorithm ([`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md), or one that uses a segment (with deformation tolerance) matching algorithm ([`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md)). All six algorithms use edge-based geometric features of the models and the target to establish a match.

All [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) controlled objects can be searched for at an angle. This can be done by setting the angle using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ANGLE_REGION`](../../Reference/mod/MmodControl.md). Note, rotated rectangular search regions can be set using [`MmodControl`](../../Reference/mod/MmodControl.md) with[`M_SEARCH_POSITION_FROM_GRAPHIC_LIST`](../../Reference/mod/MmodControl.md) or from a target image buffer associated with a rectangular [`M_VECTOR`](../../Reference/buf/MbufInquire.md) region of interest set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## General geometric contexts

General geometric Model Finder contexts use a general geometric search algorithm to locate user-specified models. General geometric contexts handle a full range of scale and angles unlike a controlled geometric context.

## Controlled geometric contexts

Controlled geometric Model Finder contexts use a controlled geometric search algorithm to locate user-specified models. Using a controlled geometric context is recommended in situations where the image has high geometric complexity and where there are only small differences in scale between the occurrence and the nominal scale of the model ([`M_SCALE`](../../Reference/mod/MmodControl.md)). In such a situation, a controlled geometric search is often faster and more robust than the general geometric search. The search method is especially fast when the angle between the occurrence and the nominal angle of the model ([`M_ANGLE`](../../Reference/mod/MmodControl.md)) is small. Model searches use a hierarchical method to reduce the time needed to complete the search. Note that this might cause very thin lines to be missed as candidates.

The speed of the search with a controlled geometric Model Finder context also depends on the angle range of the search. The search is faster when using a smaller angle range.

There are restrictions on when you can use a controlled geometric context. Scale range searches are disabled when you use this context. As well, if you are retrieving a result relating to the entire context, you cannot get or draw the edges for the entire target; you can only get or draw the target edges in the region of an occurrence.

## Circle-shape contexts

Circular-shape Model Finder contexts use a specialized algorithm to locate instances of circular shapes. This context has a tolerance for deformation of the shape being sought; therefore, it is not only applicable to circles but circular type shapes in general. This could include elliptical shapes and circular shapes with noisy contours. This context type is recommended when your target contains one or more circular shapes that must be found and other context types would either take too long to find the occurrences, or would not reliably find all occurrences of the circular shapes.

Unlike the geometric and controlled geometric types of Model Finder contexts, you can only add one model to the context and it must be a synthetic model of type [`M_CIRCLE`](../../Reference/mod/MmodDefine.md). There are further restrictions to an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context other than one model per context. For instance, you cannot add a mask (using [`MmodMask`](../../Reference/mod/MmodMask.md)) to a model in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. Any constants that are not supported for an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context are identified as such within their description.

## Ellipse-shape contexts

Elliptical-shape Model Finder contexts use a specialized algorithm to locate instances of elliptical shapes. This context searches for occurrences of elliptical shapes based on their aspect ratio relative to that of the defined model. This strategy searches for a range of aspect ratios around a nominal aspect ratio. You can use [`MmodControl`](../../Reference/mod/MmodControl.md) with[`M_ELLIPSE_TOLERANCE_TO_CIRCLE`](../../Reference/mod/MmodControl.md) to set how close an ellipse's aspect ratio must be to that of a circle's for it to be approximated as a circle. It also makes use of the deformation tolerance described for [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) types of Model Finder context, such that ellipses that are not perfectly conic would still be identified as potential occurrences. This context type is recommended when your target contains one or more elliptical shapes that must be found and other context types would either take too long to find the occurrences, are not suited for the shape being sought, or would not reliably find all occurrences of the elliptical shapes.

Like the [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context can only have one model added to the context and it must be a synthetic model of type [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md). Many of the restrictions that apply to an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context apply to an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. Any differences will be noted in the relevant constants.

## Rectangle-shape contexts

Rectangle-shape Model Finder contexts use a specialized algorithm to locate instances of rectangular shapes. This context searches for occurrences of rectangular shapes based on their aspect ratio relative to that of the defined model. This strategy searches for a range of aspect ratios around a nominal aspect ratio. This context has a tolerance for the deformation of shapes such that rectangles that are not perfectly rectangular would still be identified as potential occurrences. However, at least a portion of each of their sides must be visible; that is, the model coverage on any side cannot be 0. This context type is recommended when your target contains one or more rectangular shapes that must be found and other context types would either take too long to find the occurrences, are not suited for the shape being sought, or would not reliably find all occurrences of the rectangular shapes.

Like the [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context can only have one model added to the context and it must be a synthetic model of type [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md). Many of the restrictions that apply to an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context apply to an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. Any differences will be noted in the relevant constants.

## Segment-shape contexts

Segment-shape Model Finder contexts use a specialized algorithm to locate instances of segment shapes. This context searches for occurrences of segments defined on a contour. The segment's angle (direction) is determined from its gradient's angle (the direction of the most sudden change from dark to light causing the edge). The segment's direction is therefore the angle of the gradient - 90°. This context has a tolerance for the deformation of line segments such that segments that are not perfectly straight, or segments that are broken up, would still be identified as potential occurrences. This context type is recommended when your target contains one or more line segments that must be found and other context types would either take too long to find the occurrences, are not suited for the shape being sought, or would not reliably find all occurrences of the line segments.

Like the [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context can only have one model added to the context and it must be a synthetic model of type [`M_SEGMENT`](../../Reference/mod/MmodDefine.md). Many of the restrictions that apply to an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context apply to an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. Any differences will be noted in the relevant constants.
