---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Deep_learning_OCR
section: Requirements_and_recommendations
module_tag: dlocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Deep_learning_OCR / Requirements and recommendations"
---

# Requirements and recommendations

The Deep Learning OCR module has computer related requirements, such as a minimum amount of memory and a GPU, that are additional to what is already required to run Aurora Imaging Library. To take advantage of available hardware and maximize performance, such as reading on a GPU, you should install the required [Aurora Imaging Library add-ons and updates](S07_Requirements_and_recommendations.md).

|   |   |
| --- | --- |
| *[Image: MclassTrainEngineTest.png]* | To validate that your system can optimally perform Deep Learning OCR read operations, a Deep Learning OCR prediction engine test is available in the **Aurora Imaging Configurator** utility under the **DLOCR** item. |

## Aurora Imaging Library add-ons and updates

To maximize the capabilities of the Deep Learning OCR module, you should install the latest Aurora Imaging Library add-ons and updates, which are available for download exclusively from the Downloads section of the Zebra Aurora Imaging Library product support webpage ([zebra.com/ail-info](https://zebra.com/ail-info)). In particular, check for add-ons related to GPU training, OpenVINO predict engines and CUDA predict engines.

The Aurora Imaging Library release notes, accessible from Aurora Imaging Control Center and/or the product support webpage, can provide additional documentation, such as new or modified features, systems, and products.

## Computer requirements for a deep learning OCR context

To use a Deep Learning OCR context, note the requirements and recommendations for the following:

- GPU and CPU.
  - Prediction might be faster on a GPU. Use the **DLOCR** item found in the **Aurora Imaging Configurator** utility to change the predict engine and number of cores. Performance should be analyzed on your own applications.
    > **Note:** GPU related requirements and recommendations depend on whether you have installed the latest [Aurora Imaging Library add-ons](S07_Requirements_and_recommendations.md).
  - The GPU must be from NVIDIA (for example, Quadro P1000, GTX 1650, RTX 2060, and RTX 2070). Note, some GPUs only function when connected to a monitor.
  - The NVIDIA GPU driver version must be at least 452.39.
  - The supported GPU compute capabilities range from 3.7 to 8.9; the recommended minimum is 6.0. For more information on compute capability, refer to the CUDA GPUs | NVIDIA Developer page at [developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus) (support for CUDA on NVIDIA GPUs is provided with an Aurora Imaging Library add-on).
  - The recommended graphics driver for Intel Integrated Graphics when using the Intel OpenVINO provider for predicting is 27.20.100.9079 (support for OpenVINO prediction is provided with an Aurora Imaging Library add-on).
  - When using a GPU for read operations, one CPU core is still needed. Using a CPU with a higher clock rate, rather than a higher number of cores, is recommended.
  - When using the CPU for read operations, a high-end multi-core CPU is recommended.
  - It is recommended to have at least 4 gigabytes of GPU memory.
- Operating system and main memory.
  - You must use a 64-bit Windows 10 operating system (or later).
  - It is recommended that main memory is at least twice the size of the GPU's memory.
- SSD.
  - It is recommended to disable features and processes that can severely reduce disk performance, such as file indexing and anti-virus processes.

## GPU and related installations

You should install the most appropriate drivers and updates for any machine learning component you are using, such as GPUs and iGPUs. These drivers and updates are not specific to Aurora Imaging Library and are typically available for download by right-clicking on the device name (such as the specific GPU you are using) listed under the **Display adapters** item found in Microsoft Window's **Device Manager**.

If necessary, you can follow these steps to verify whether your GPU is installed correctly:

1. Open the Windows Command Prompt (press the Windows key + R and type "cmd").
2. Type "devmgmt.msc".
3. Ensure that the GPU is working correctly; for example, there should not be an exclamation point. The following is an example of how a properly functioning GPU is listed:
   *[Image: MclassTroubleshootVideoCard.png]*
4. Ensure that you are not using a GPU that should be connected to a monitor and is not. The following is an example of how an improperly connected GPU is listed:
   *[Image: MclassTroubleshootVideoCardRequiresMonitor.png]*
5. If the problem persists, download the latest display driver for your GPU.
