---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Basic_concepts
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Advanced Geometric Matcher module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Advanced Geometric Matcher module are:

- **Composite-definition model**. A model that can hold information about valid model appearances and background samples. This type of model requires training before it can be used to search for occurrences.
- **Coverage score**. The percentage of the total length of the model's edges that are found in an occurrence.
- **Detection score**. The likelihood that a found occurrence is a true occurrence.
- **Edge**. A curve that delineates a boundary, which can be established from intensity transitions in an image.
- **Edgel**. Elementary point (or edge element) within an edge.
- **Fit score**. A measure of the correlation between model edgels and occurrence edgels.
- **Model**. The information that defines the pattern of edges to find in the target.
- **Model occurrence**. An instance of the model found in the target.
- **Model image**. The model, when drawn, is a model image. For a single-definition model defined from an image, the model image is the entire source image or a copy of the region in the source image from which the model has been defined. For a single-definition model defined from an Edge Finder result buffer, the model image is the image source of the result buffer from which the model has been defined. For a single-definition model defined from a CAD DXF file, the model image is the image representation (edge map) of the model. For a composite-definition model, the model image is the entire source image; if a separate image has not been provided for the fit and coverage calculations, no model image exists.
- **Model origin**. The point in the model's coordinate system that is considered to be (0, 0).
- **Model source**. The immediate source of the model's edges. For a single-definition model, the model source is a single user-provided image, the buffer of the Edge Finder results, or a CAD DXF file. For a composite-definition model, the model source for detection is the train AGM result buffer; the model source for the fit and coverage can be the train AGM result buffer or a single user-provided image.
- **Negative model location**. An area in a training image where a model is not found. Negative model locations are background elements and are delimited by red rectangles.
- **Positive model location**. An area in a training image where a model is found. Positive model locations are model appearances and are delimited by blue rectangles.
- **Single-definition model**. A model built from a single model appearance. This type of model can be used to search for occurrences without prior training.
- **Target**. The image or Edge Finder result buffer in which to search for model occurrences.
- **Trained composite-definition model**. A composite-definition model that has been successfully trained with model appearances and background samples, allowing you to use it to search for occurrences.
- **Training image**. An image that contains appearances of the model and/or background.
