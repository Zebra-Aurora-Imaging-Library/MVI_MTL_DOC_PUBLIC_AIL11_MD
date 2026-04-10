---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: Basic_concepts_for_3D_display
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / Basic concepts for 3D display"
---

# Basic concepts for 3D display

The basic concepts and vocabulary conventions when dealing with 3D displays are:

- **3D display**. An Aurora Imaging Library object used to show 3D data and annotations in a window.
- **3D graphic**. An Aurora Imaging Library object that is shown in a 3D display.
- **3D graphics list**. An Aurora Imaging Library object that holds 3D graphics, including 3D graphics that are associated with 3D-displayable point cloud containers or depth map image buffers.
- **Default 3D graphics list**. The 3D graphics list associated with a 3D display. This association can never be changed. A 3D display's default 3D graphics list is automatically allocated and freed at the same time as the 3D display.
- **Handle graphic**. A 3D graphic that can be rotated or translated with the mouse in a 3D display.
- **View**. Your field of view in the 3D space of your 3D display. The view always looks from the viewpoint to the interest point.
- **Viewpoint**. The position from which the view is looking.
- **Interest point**. The position the view is looking at. This point is always at the center of the window.
- **Up-vector**. A unit vector that indicates which direction is up for the view.
- **FoV angle (field-of-view angle)**. An angle used to determine the size of the view. FoV angle is analogous to the focal length of a camera. A 3D display has horizontal and vertical FoV angles, which always have the same aspect ratio as the size of the window. Increasing the FoV angle has the effect of zooming the view out and exaggerating perspective distortion at the edges of the view.
- **Root node**. The 3D graphic at the top of the descendent hierarchy of a 3D graphics list. All 3D graphics in a 3D graphics list are descendents of the root node.
