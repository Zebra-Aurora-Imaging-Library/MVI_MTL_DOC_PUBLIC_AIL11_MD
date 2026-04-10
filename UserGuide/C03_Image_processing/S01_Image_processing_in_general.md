---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Image_processing_in_general
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Image processing in general"
---

# Image processing overview

Pictures, or images, are important sources of information for interpretation and analysis. These might be images of a building undergoing renovations, a planet's surface transmitted from a spacecraft, plant cells magnified with a microscope, or electronic circuitry. Human analysis of these images or objects presents inherent difficulties: the visual inspection process is time-consuming and subject to inconsistent interpretations and assessments. Computers, on the other hand, are ideal for performing these tasks.

In order for computers to process images, the images must be numerically represented. This process is known as image digitization.

Once images are represented digitally, computers can reliably automate the extraction of useful information through the use of digital image processing. Digital image processing performs various types of image enhancements, distortion corrections, and measurements.

## Aurora Imaging Library and image processing

Aurora Imaging Library provides a comprehensive set of image processing operations. There are two main types of image processing operations:

- Those that enhance or transform an image.
- Those that analyze an image (that is, generate a numeric or graphic report that relates specific image information).

Aurora Imaging Library supports such operations as:

- **Point-to-point operations**. These operations include constant thresholding, image comparison, image subtraction, and image mapping. They compute each pixel result as a function of the pixel value at a corresponding location in either one or two source images.
- **Statistical operations**. These extract statistical information from a given image, such as the minimum or maximum image pixel value or a histogram. They condense a frame of pixels into a smaller, more functional set of values for analysis.
- **Spatial filtering operations**. These operations are also known as convolutions. They include operations that can smooth images, enhance and detect image edges, and remove 'noise' from an image. Most of these operations compute results based on an underlying neighborhood process: the weighted sum of a pixel value and its neighbors' values.
- **Morphological operations**. These operations include erosion, dilation, opening, and closing of images. They compute new values according to geometric relationships and matches to known patterns in the input image.
- **Geometric transformation operations**. These operations are used to resolve distortion problems. They can properly orient an image, flip the image vertically or horizontally, and resize an image to obtain the right aspect ratio.
