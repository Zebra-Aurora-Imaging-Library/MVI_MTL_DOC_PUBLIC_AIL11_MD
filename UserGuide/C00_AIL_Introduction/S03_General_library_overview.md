---
doctype: UserGuide
part: "Getting started"
chapter: AIL_Introduction
section: General_library_overview
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / AIL_Introduction / General library overview"
---

# General Library overview

Aurora Imaging Library is a C library / SDK that you can use to write C/C++ applications. By including the provided Aurora Imaging Library .NET wrapper in your code, the library's functions are also available when writing managed Visual C# applications. By including the library's Python wrapper, its functions are also available when writing code in Python 3.x (supported versions only). As an SDK, the library also offers many tools to help create applications.

The library is designed for fast application development and ease of use. It can run with only the Host CPU only, and can take advantage of specialized acceleration Zebra hardware if it is available and is more efficient. The library has a transparent management system and handles physical objects (such as frame grabbers/boards) as virtual objects, allowing for platform-independent applications.

The library uses the notion of systems to identify boards, and more than one can be controlled by an application program. The library lets multiple computers collaborate across a network, either in a controlling configuration within a single application or in a monitoring configuration between concurrent applications. Both configurations are supported using Distributed Aurora Imaging Library.

## Some full and lite details

Aurora Imaging Library comes in a full version, and a version that includes only core functions. The latter is known as Aurora Imaging Library Lite. This Help refers to both full and lite versions of the library as Aurora Imaging Library (or simply "the library"), unless it is necessary to differentiate.

Aurora Imaging Library Lite lets you grab, display, archive, transfer, and annotate images. It also lets you perform some processing (typically for preprocessing grabbed images).

Aurora Imaging Library Lite can:

- Grab up to 16-bit grayscale images, or color images.
- Process 1, 8, 16, and 32-bit integer or floating-point grayscale images, depending on the operation.
- Process color images, depending on the operation. Each band of a color image is processed individually, one after the other. Basic color processing operations, such as converting color images to grayscale, are supported.
- Display 1, 8, or 16-bit grayscale or color images (if the platform supports it).
- Control custom FPGA configurations on applicable Zebra hardware to offload processing tasks from the Host.
- Communicate with automation and robot controllers using the specified protocols.
- Perform core 3D functions such as: display point cloud containers and 3D objects or generate 3D geometries.

The full version of the library expands on this basic functionality and allows you to perform more advanced image processing and analysis. The full version provides a more extensive set of functions in the image processing module (for example, Fast Fourier or watershed transformations), as well as letting you use modules to do: 3D reconstruction, bead analysis, blob analysis, code read/write, camera calibration, color analysis, edge finding, machine learning, measurement, metrology, geometric model finding, pattern matching, character recognition, registration, dot-matrix reading, and string reading.

If the available operations do not provide the required functionality, you can use the library's Function Development module to create your own pseudo-Aurora Imaging Library functions, also known as user-defined Aurora Imaging Library functions.
