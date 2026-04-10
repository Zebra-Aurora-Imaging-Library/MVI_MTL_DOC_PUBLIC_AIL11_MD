---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Basic_concepts
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Geometric Model Finder module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Geometric Model Finder module are:

- **Active edges**. Edges which are used to compose the geometric model without being masked out, and are searched for in the target.
- **Bounding box of the edges**. The smallest rectangle that fully encloses all of the edges of the model.
- **Chain**. The set of connected edgels that construct an edge.
- **Edge**. A curve that delineates a boundary, which can be established from intensity transitions in an image. In Model Finder, an edge is considered to be a chain, and its features.
- **Edge feature**. Information describing an edge, such as its length.
- **Edge map**. The edges extracted from the model image.
- **Edgel**. Elementary point (or edge element) within an edge.
- **Model**. The information that defines the pattern of active edges to find in the target and the search settings with which to do so.
- **Model box**. The box that delimits the boundaries of the model. Essentially, the model box is the bounding box of the edges plus a margin. For image-type and Edge Finder-type models, the size of the model box is specified when defining the model. For synthetic models, the margin can be adjusted after model definition.
- **Model Finder context**. An Aurora Imaging Library object that stores all models for which to search. The Model Finder context also stores global search settings that apply to the search algorithm.
- **Model image**. For an image-type and Edge Finder-type model, the model image is a copy of the region in the model source image from which the model has been defined. Note that the model source image for an Edge Finder-type model is the image source of the result buffer. For a synthetic model, the model image is the image representation (edge map) of the model.
- **Model mask**. A binary image used to define irrelevant, inconsistent, or featureless areas in the model, so that only pertinent model details are used for the search.
- **Model origin**. The point in the model's coordinate system that is considered to be (0, 0).
- **Model source**. The immediate source of the model's edges. For example, for an Edge Finder-type model, the model source is the buffer of the Edge Finder results.
- **Model source image**. The image from which you have defined an image-type model, or from which you have extracted the edges for an Edge Finder-type model.
- **Occurrence**. An instance of the model found in the target.
- **Score**. A measure of the active edges in the model found in the occurrence, weighted by the deviation in position of these common edges.
- **Synthetic models**. Models that have a predefined shape, such as a circle or a square, or are defined from a CAD-type file.
- **Target**. The image or result buffer in which to search for occurrences of the model.
- **Target score**. A measure of edges found in the occurrence that are not present in the original model, weighted by the deviation in position of the common edges.
- **Target edges**. The edges in the target.
