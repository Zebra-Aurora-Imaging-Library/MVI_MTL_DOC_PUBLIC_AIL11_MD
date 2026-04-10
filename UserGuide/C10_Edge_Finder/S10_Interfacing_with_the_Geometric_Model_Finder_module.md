---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Interfacing_with_the_Geometric_Model_Finder_module
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Interfacing with the Geometric Model Finder module"
---

# Interfacing with Geometric Model Finder or Advanced Geometric Matcher

The Aurora Imaging Library Model Finder and Advanced Geometric Matcher (AGM) modules allow you to define models from an Edge Finder result buffer or find model occurrences in an Edge Finder result buffer. If you want to use Model Finder or AGM, and are also familiar with Edge Finder, you can perform specialized edge selections, which can reduce the unnecessary geometric complexity in both the model and/or the target image. For example, if you want to find model occurrences in an Edge Finder result buffer, you can use [`MedgeSelect`](../../Reference/edge/MedgeSelect.md) to select which edges to include in the buffer, based on one of several criteria. As a result, Model Finder/AGM will have much less information to analyze, thereby potentially speeding up the search for the model(s).

Another way to speed up Model Finder's search for model(s) in the result of an edge extraction is to specify a lower scale for the edge extraction, using [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md). [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) must be set before calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). Be aware that reducing the extraction scale can result in less reliable results, including, the loss of important details and/or can reduce the accuracy of the search result. Note that [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) sets or returns the scale of the image at which to do the edge extraction. Once the extraction is complete, the results are scaled to the original scale of the image.

> **Note:** Note that this does not apply to AGM. For use with AGM, [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) must be set to 1 (default); otherwise, you will get an Aurora Imaging Library error.

To have further control over the edges that Model Finder/AGM uses, you can allocate an Edge Finder result buffer and add the required edge chains to the result buffer, using [`MedgePut`](../../Reference/edge/MedgePut.md). This allows you to combine results from multiple result buffers, or create a target that contains user-defined edge chains. Note that you do not need to call [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) after adding edge chains to a result buffer. For more information on adding edge chains, see [Putting data into an Edge Finder result buffer](S08_Advanced_edge_extraction.md).

> **Note:** Models and target images should use the same edge types (object contours or line crests). Therefore, if the target edges are object contours, the model edges must be object contours; if the target edges are line crests, the model edges must be line crests.

If you expect to use an Edge Finder result buffer with Model Finder or AGM, you must enable [`M_MODEL_FINDER_COMPATIBLE`](../../Reference/edge/MedgeControl.md) or [`M_AGM_COMPATIBLE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md) before the initial call to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). To add models defined from an Edge Finder result buffer to a Model Finder context, use [`M_EDGE_RESULT`](../../Reference/mod/MmodDefine.md) in [`MmodDefine`](../../Reference/mod/MmodDefine.md). To add a model defined from an Edge Finder result buffer to an AGM context, use [`M_EDGE_RESULT`](../../Reference/agm/MagmDefine.md) in [`MagmDefine`](../../Reference/agm/MagmDefine.md).

To set the sensitivity with which edgels are detected, Edge Finder users can use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with either [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) or [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md). [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) helps users already familiar with Model Finder. This control type should only be used when interfacing with Model Finder or AGM. In [`MedgeControl`](../../Reference/edge/MedgeControl.md), the [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) setting overrides the [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) setting.

The following is an example of a typical Edge Finder/Model Finder application:

*[Image: InterfacingGeoFinderSample.png]*

For more information on Model Finder, see [Geometric Model Finder module](../C08_Geometric_Model_Finder/S01_Geometric_Model_Finder_module.md). For more information on AGM, see [Advanced Geometric Matcher module](../C09_Advanced_Geometric_Matcher/S01_Advanced_Geometric_Matcher_module.md).
