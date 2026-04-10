---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Metrology
section: Basic_concepts
module_tag: met
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / metrology / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Metrology module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Metrology module are:

- **Active edgels**. The edgels within the metrology region that adhere to certain constraints.
- **Constructed feature**. A geometric primitive that has been mathematically defined or geometrically derived from a set of other features.
- **Edge**. A curve that delineates a boundary, which can be established from intensity transitions in an image.
- **Edgel**. Elementary points (or edge elements) within an edge. Each edgel represents an X- and Y-position, and the angular direction of the gradient.
- **Fitted edgels**. The active edgels that are used to build a fitted feature.
- **Geometric tolerance**. The acceptable deviation from the definition of a feature, or the acceptable relationship between multiple features.
- **Global frame**. The metrology template's default reference frame.
- **Gradient angle**. The angle between the target image's horizontal axis and the edge's perpendicular direction (from black to white) at each edgel location.
- **Iterative fit**. The calculated fit resulting from one of the iterations during the iteration process.
- **Local frame**. A constructed reference frame.
- **(Physically) measured feature**. A geometric primitive that can be physically measured using the edgels in the metrology region.
- **Metrology context**. An Aurora Imaging Library object that stores all features and geometric tolerances of the metrology template, and global processing settings.
- **Metrology region**. The delimited area in the target image from which to select the edgels used to establish a physically measured feature. The features with which to build constructed features can also have a delimited metrology region.
- **Metrology region orientation**. The alignment of a metrology region, with respect to a reference frame.
- **Metrology template**. The set of all features and geometric tolerances.
- **Output frame**. The coordinate system in which feature results are returned.
- **Parametrically constructed feature**. A geometric primitive that has been mathematically defined, rather than geometrically derived from a set of other features.
- **Reference frame**. The coordinate system in which features are defined and to which they are relative.
- **Target image**. The image in which to extract physically measured features, build constructed features, and verify geometric tolerances.
- **Template reference**. An image (or result buffer) that represents an example of a target image, used to help build or modify a metrology template.
