---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_with_the_MIL_Classification_module
section: Requirements_recommendations_and_troubleshooting
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / ML_Classification_and_classifier_overview / Requirements recommendations and troubleshooting"
---

# Requirements, recommendations, and troubleshooting

The Classification module has computer related requirements, such as memory and GPU, that are additional to what is already required to run Aurora Imaging Library. For example, datasets can have many images, and you must ensure that you have enough RAM to use them for training. There are also several recommendations that you should follow, particularly related to training a predefined CNN, segmentation, object detection, or anomaly detection classifier. To take advantage of available hardware and maximize performance, such as training and predicting on a GPU, you should install the required [Aurora Imaging Library add-ons and updates](S06_Requirements_recommendations_and_troubleshooting.md).

Other than ensuring that you have enough RAM for your datasets, tree ensemble classifiers do not have any additional computer related requirements.

|   |   |
| --- | --- |
| *[Image: MclassTrainEngineTest.png]* | To validate that your system can optimally perform image classification (CNN), segmentation, object detection, or anomaly detection, a CNN Train Engine Test is available in the **Aurora Imaging Configurator** utility under the **DL Classifiers** item. |

## Aurora Imaging Library add-ons and updates

To maximize the capabilities of the Classification module, you should install the latest Aurora Imaging Library add-ons and updates, typically related to classification, training, prediction (inference), and Aurora Imaging CoPilot. Add-ons and updates are available for download exclusively from the Downloads section of the Zebra Aurora Imaging Library product support webpage ([zebra.com/ail-info](https://zebra.com/ail-info)). In particular, check for add-ons related to GPU training, OpenVINO predict engines, and CUDA predict engines.

> **Note:** If you are using an NVIDIA GPU to train a Deep Learning classifier, you must download and install the relevant update or add-on.

The Aurora Imaging Library release notes, accessible from Aurora Imaging Control Center and/or the product support webpage, can provide additional documentation, such as new or modified features, systems, and products.

## Computer requirements for a predefined classifier

To use a predefined CNN, segmentation, object detection, or anomaly detection classifier, note the requirements and recommendations for the following:

- GPU and CPU.
  - It is highly recommended to use a GPU for training (CPU training can take significantly longer). Some GPUs only function when connected to a monitor. GPUs are also known as graphic cards or display adapters.
    Prediction might be faster on a GPU. Aurora Imaging Library provides an example, _PredictEngineSelection.cpp_, to help you pick the fastest prediction engine available to you.
    > **Note:** GPU related requirements and recommendations can depend on whether you have installed the latest Aurora Imaging Library add-ons / updates.
  - The GPU must be from NVIDIA; for example, Quadro P1000, RTX 3060, and RTX 4070.
  - The NVIDIA GPU driver version must be at least 452.39.
  - The supported GPU compute capabilities range from 3.7 to 8.9; the recommended minimum is 6.0. For more information on compute capability, refer to the CUDA GPUs | NVIDIA Developer page at [developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus) (support for CUDA on NVIDIA GPUs is provided with an Aurora Imaging Library add-on / update).
  - The recommended graphics driver for Intel Integrated Graphics when using the Intel OpenVINO provider for predicting is 27.20.100.9079 (support for OpenVINO prediction is provided with an Aurora Imaging Library add-on / update).
  - When using a GPU, one CPU core is still needed. Using a CPU with a higher clock rate, rather than a higher number of cores, is recommended. If using the CPU for training, a high-end multi-core CPU is recommended.
  - It is recommended to have at least 4 gigabytes of GPU memory. Having more memory is preferable, as it allows for a larger mini-batch size during training.
  - Although it is possible to use eGPUs (external GPUs) for training, they are designed for gaming and have not been tested. To use an eGPU, try training with it on a laptop with a Thunderbolt 3 port. eGPUs are not supported on the desktop.
  - While training on the GPU, avoid running any other application on it.
- Operating system and main memory.
  - You must use a 64-bit Windows 10 or above operating system.
  - It is recommended that the system memory be at least twice the size of the GPU's memory. You should ensure that your system has at least 8 gigabytes of RAM. As previously discussed, you must have enough memory to process datasets, which can be large (in particular, for segmentation, object detection, and anomaly detection).
- SSD.
  - It is recommended to install Aurora Imaging Library on a fast storage drive, typically an NVMe-enabled solid state disk (SSD) to maximize performance and efficiency during training and prediction tasks. Furthermore, storing your datasets on a fast storage drive can significantly reduce the time required for these operations. Consider the size requirements of your dataset when selecting a storage drive.
  - Disable features and processes that can severely reduce disk performance, such as file indexing and anti-virus processes.

> **Note:** It is recommended to use the latest update of Aurora Imaging CoPilot for managing your images datasets. For additional examples and utilities, refer to the [Zebra Aurora Imaging Library](https://github.com/Zebra-Aurora-Imaging-Library) page in GitHub.

## Troubleshooting for training a predefined classifier

If you are having trouble using a predefined CNN, segmentation, object detection, or anomaly detection classifier on your computer, you can call [`MclassInquire`](../../Reference/class/MclassInquire.md) to retrieve information that can help diagnose the problem (for example, training errors or training very slowly). For example, you can inquire:

- The general engine (GPU or CPU) that Aurora Imaging Library is using to train ([`M_TRAIN_ENGINE_USED`](../../Reference/class/MclassInquire.md) and [`M_TRAIN_ENGINE`](../../Reference/class/MclassInquire.md)), or a description of the specific engine ([`M_TRAIN_ENGINE_USED_DESCRIPTION`](../../Reference/class/MclassInquire.md)), for example, "GeForce RTX (tm) 2060 6GB" and "Intel(R) Core(TM) i7-8700 CPU 3.20GHZ". To train properly, Aurora Imaging Library should use the engine you expect (preferably, the best compatible GPU).
- The status of the GPU ([`M_GPU_TRAIN_ENGINE_LOAD_STATUS`](../../Reference/class/MclassInquire.md)) or CPU ([`M_CPU_TRAIN_ENGINE_LOAD_STATUS`](../../Reference/class/MclassInquire.md)) training engine. To train properly, Aurora Imaging Library must successfully load the train engine.
- Whether a training DLL is installed ([`M_TRAIN_ENGINE_IS_INSTALLED`](../../Reference/class/MclassInquire.md)). To train properly, Aurora Imaging Library must successfully install the training DLL.
- Whether your training images adhere to the requirements of the classifier. Image requirements can include data type and depth ([`M_TYPE`](../../Reference/class/MclassInquire.md)), as well as the number and order of bands ([`M_SIZE_BAND`](../../Reference/class/MclassInquire.md) and [`M_BAND_ORDER`](../../Reference/class/MclassInquire.md)). Invalid training images lead to invalid training; it is recommended to check these requirements after training to ensure all images are valid.
- Whether the selected training mode (complete, fine tuning, or transfer learning) is allowed, given your classifier's current configurations ([`M_TRAINABLE_COMPLETE`](../../Reference/class/MclassInquire.md), [`M_TRAINABLE_FINE_TUNING`](../../Reference/class/MclassInquire.md) or [`M_TRAINABLE_TRANSFER_LEARNING`](../../Reference/class/MclassInquire.md)). Not all training modes support all training settings; for example, you cannot change the number of classes if you are using a fine tuning training mode.

To retrieve information about the general state of the training, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_STATUS`](../../Reference/class/MclassGetResult.md). The status can give you critical information, such as whether the training (or prediction) was successful, or whether it encountered an issue (for example, not enough memory or a non-finite computational value).

> **Note:** Similar information is also available for prediction (for example, [`M_PREDICT_ENGINE_PROVIDER`](../../Reference/class/MclassInquire.md), [`M_PREDICT_ENGINE_DESCRIPTION`](../../Reference/class/MclassInquire.md), and [`M_PREDICT_ENGINE_PRECISION`](../../Reference/class/MclassInquire.md), and [`M_STATUS`](../../Reference/class/MclassGetResult.md)).

### GPU and related installations

You should install the most appropriate drivers and updates for any machine learning component you are using, such as GPUs and iGPUs. These drivers and updates are not specific to Aurora Imaging Library and are typically available for download by right-clicking on the device name (such as the specific GPU you are using) from the **Display adapters** item found in Microsoft Window's **Device Manager**. On some computers, installing a discrete GPU will disable the onboard graphics (iGPU). A setting in the BIOS might be present to allow the use of the onboard graphics and the discrete GPU at the same time.

If necessary, you can follow these steps to verify whether your GPU is installed correctly:

1. Open the Windows Command Prompt (press the Windows key + R and type "cmd").
2. Type "devmgmt.msc".
3. Ensure that the GPU is working correctly; for example, there should not be an exclamation point. The following is an example of how a properly functioning GPU is listed:
   *[Image: MclassTroubleshootVideoCard.png]*
4. Ensure that you are not using a GPU that should be connected to a monitor and is not. The following is an example of how an improperly connected GPU is listed:
   *[Image: MclassTroubleshootVideoCardRequiresMonitor.png]*
5. If the problem persists, install the latest display driver for your GPU.
