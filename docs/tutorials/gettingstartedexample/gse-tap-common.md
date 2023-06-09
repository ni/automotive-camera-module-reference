# PXIe-148X Getting Started Example - Common Tap Tutorials

This document covers a range of common scenarios using the PXIe-148X Tap Getting Started Example (GSE) to help you understand LLP generation, I2C and GPIO timestamping, and common configuration options.

> Note: This document references the example included with the NI-FlexRIO 23Q3 driver. Examples included in newer releases of the driver should be applicable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Prerequisites
A supported interface module that includes both serial input and serial output channels (tap module) and a second supported interface module that includes serial input channels (acq module) on a PXI system running Windows.

A camera supported by the getting started example configuration scripts (i.e. Leopard Imaging IMX490) or a third supported interface module that includes serial output channels (gen module).

| **Interface Module**                        | **Camera**                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| PXIe-1486 (8 In - 954 Deserializer)         | [LI-IMX490-FPDLINKIII](https://www.leopardimaging.com/product-category/autonomous-camera/ti-fpdlinkiii-cameras/li-imx490-fpdlinkiii/) |
| PXIe-1486 (4 In 4 Out - 953/954 SerDes)     | [LI-IMX490-FPDLINKIII](https://www.leopardimaging.com/product-category/autonomous-camera/ti-fpdlinkiii-cameras/li-imx490-fpdlinkiii/) |
| PXIe-1487 (8 I - 9296A Deserializer)        | [LI-IMX490-GMSL2](https://www.leopardimaging.com/product-category/autonomous-camera/maxim-gmsl2-cameras/li-imx490-gmsl2/)             |
| PXIe-1487 (4 In 4 Out - 9295A/9296A SerDes) | [LI-IMX490-GMSL2](https://www.leopardimaging.com/product-category/autonomous-camera/maxim-gmsl2-cameras/li-imx490-gmsl2/)             |

> Note: The tap and acq modules used in this tutorial must have matching model numbers (i.e. 1486).

This tutorial is written for users who understand how to perform a basic tap acquisition with PXIe-148X GMSL or FPD-Link interface modules. At a minimum it is required that you complete the [Initial Hardware Setup](gse-tap-basic.md#initial-hardware-setup) and [Initial Software Setup](gse-tap-basic.md#initial-software-setup) from the PXIe-148X Getting Started Example - Basic Tap Tutorial before starting this tutorial.

> Note: Tutorials in this document use one tap LabVIEW project and one acquisition LabVIEW project as well as a camera or a generation LabVIEW project to provide the image data to the tap module serial input. All tutorial steps in this document reference the Tap Example VI unless otherwise specified and all input control values not specified should be left as the default value.

## Running a Generation-Tap-Acquisition Getting Started Example Combination
This tutorial uses a gen module, tap module and acq module to demonstrate basic tap capabilities using a gen module in place of a camera. The sequence for this tutorial is configure generation example, configure tap example, configure acquisition example, start acquisition example, start tap example, start generation example, wait for generation example to complete, stop acquisition example, stop tap example.

1. Disconnect the camera from serial input channel 0 (SI0) on the tap module and connect serial output channel 0 (SO0) on the gen module to serial input channel 0 (SI0) on the tap module with a FAKRA cable.

2. Open a Generation Example VI (see [Basic Generation Tutorials - Initial Software Setup](./gse-gen-basic.md#initial-software-setup)) and run [Basic Generation Tutorials - Create TDMS Files for Generation](./gse-gen-basic.md#create-tdms-files-for-generation).
    > Note: If the gen module is the same module type as the tap module, there is no need to create a new project. If the gen module is not the same module type as the tap module, a new project needs to be created for the gen module in order for the gen module FPGA bitfiles to be included in the project.

3. On the Generation Example VI, run the [Performing a Simple Generation](./gse-gen-basic.md#performing-a-simple-generation) tutorial using the gen module, but do not run the final step that starts the generation.

4. Set the following controls on the Tap Example VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | **Resource** | **RIO Device** | [System Specific] |
    | **Resource** | **Bitfile Path** | [Refer to Bitfile Path in the PXIe-148X Tap GSE Help](../../reference/gettingstartedexample/gse-tap-help.md#table-of-pxie-148x-tap-bitfiles) |
    | **Serial Channel** | **Deserializer (Input) Configuration Script** | [Refer to Deserializer Tap Scripts in the PXIe-148X Tap GSE Help](../../reference/gettingstartedexample/gse-tap-help.md#table-of-pxie-148x-deserializer-tap-scripts) |
    | **Serial Channel** | **Serializer (Output) Configuration Script** | [Refer to Serializer Tap Scripts in the PXIe-148X Tap GSE Help](../../reference/gettingstartedexample/gse-tap-help.md#table-of-pxie-148x-serializer-tap-scripts) |

5. Run the Tap Example VI and wait for the **Waiting for Sensor Setup** indicator to illuminate.

6. On the Acquisition Example VI set the following controls and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab            | Control                 | Value                                                                                                                                                               |
    |----------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **Resource**       | **RIO Device**              | [System Specific]                                                                                                                                                   |
    | **Resource**       | **Bitfile Path**            | [Refer to Bitfile Path in the PXIe-148X Acquisition GSE Help](../../reference/gettingstartedexample/gse-acq-help.md#table-of-pxie-148x-acquisition-bitfiles)        |
    | **Serial Channel** | **Configuration Script**    | [Refer to Configuration Script in the PXIe-148X Acquisition GSE Help](../../reference/gettingstartedexample/gse-acq-help.md#table-of-pxie-148x-acquisition-scripts) * |

    > \* Since an I2C tap bridge has been established by the tap example, choose the appropriate configuration script for the tap device to run from the acquisition example regardless of which interface module is used for acquisition. 

7. Run the Acquisition Example VI and wait for the **Acquisition In Progress** indicator to illuminate.

8. On the Tap Example VI, click **Sensor Setup Complete** to start the tap acquisition.

9. On the Generation Example VI, click **Serializer Setup Complete** to start the generation and wait for the generation to complete.

10. Select the First Display Channel tab on the Generation, Tap and Acquisition Examples and verify that images from the generated TDMS file are displayed on these tabs. The images should look identical.

![Compare First Display Channel Tabs](../../images/PXIe-148x-Gen-Tap-Acq-display.png)

10. Click the **Stop Acquisition** button on both the Tap and Acquisition Example VIs to stop the tap acquisition and stop the VIs.

## Logging and Displaying I2C Timestamps
TODO

## Setting FPGA Display Parameters
TODO

### Displaying Acquired Images
TODO

### Reducing System Bandwidth Usage
TODO

## Setting RAW Display Parameters
TODO

## Setting Serial Channel Configurations
TODO

### Displaying Acquired Images From Multiple Cameras
TODO

## Using the General Purpose Input/Output (GPIO) Lines
TODO

### Manually Reading and Writing to GPIO
TODO

### Routing GPIO Lines
TODO