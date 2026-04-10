---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_under_Linux
section: Additional_Linux_notes_Classification_Arm
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / Linux / Additional Linux notes Classification Arm"
---

# Miscellaneous user guide information for Linux: Classification and Arm

This section includes additional user guide information for Linux regarding the Aurora Imaging Library Classification module, Arm support, and general/supplemental considerations. The information found in this section might be a reiteration of content previously documented.

## Classification updates/add-ons

Aurora Imaging Library 11 releases updates/add-ons for enhancing the Classification module. Below you will find information about some initial ones. Additional updates/add-ons can be made available. The Aurora Imaging Library release notes, accessible from Aurora Imaging Control Center and/or the Zebra Aurora Imaging Library product support webpages (for example, [zebra.com/ail-info](https://zebra.com/ail-info)), can provide additional documentation, such as new or modified features, systems, and products.

- Aurora Imaging Library 11 Update 2: add-on/support for predicting with OpenVINO / Intel hardware / CPU and integrated GPU.
  This update enables predicting (inference) offload to an Intel integrated GPU (IGPU) as well as potential acceleration using an Intel CPU and IGPU.
  To install this update/add-on, run:
  ```
  > sudo bash ail-openvino-11.00-002-installer.run
  ```
  This update/add-on requires the OpenCL runtime package. For Ubuntu, install `ocl-icd-libopencl`.
  To get IGPU acceleration, you must have suitable Intel graphics and you must install the Intel OpenCL driver. To do so, you can refer to the to the following: [dgpu-docs.intel.com/driver/installation.html](https://dgpu-docs.intel.com/driver/installation.html) and [github.com/intel/compute-runtime/blob/master/README.md](https://github.com/intel/compute-runtime/blob/master/README.md).
- Aurora Imaging Library 11 Update 3: add-on/support for predicting with NVIDIA CUDA.
  This update enables predicting (inference) using NVIDIA CUDA. That is, it enables offload to, and potential acceleration using, NVIDIA GPUs.
  To install this update/add-on, run:
  ```
  > sudo bash ail-cuda-11.00-003-installer.run
  ```
  This update/add-on requires installing the NVIDIA proprietary driver. To do so, follow the instructions for your specific Linux distro (distribution/version).

Installing other Classification updates/add-ons would follow a pattern that is similar to the previously stated commands. For example, installing a CUDA or OpenVINO Update numbered XXX would mean running:

```
> sudo bash ail-cuda-11.00-XXX-installer.run
> sudo bash ail-openvino-11.00-XXX-installer.run
```

## Arm support

The following hardware and distributions were tested:

- ```
  Raspberry Pi 5 (minimum 8GB of RAM) | Ubuntu 24.04
  ```
- ```
  NVIDIA Jetson Orin | Ubuntu LTS | 20.04 | 64-bit
  ```

The following hardware fingerprints are supported for runtime licenses:

- ```
  Zebra Concord PoE and Zebra Rapixo CXP frame grabbers
  ```
- ```
  Zebra ID USB dongle
  ```
- ```
  Raspberry Pi 5
  ```

When using an evaluation license, the date and time must always be set correctly. This requires the NTP service to be enabled, which in turn requires access to the time server (typically on the internet).

When the Raspberry Pi is configured in multi-monitor mode, OpenGL can encounter some challenges, which can adversely affect 3D displays.
