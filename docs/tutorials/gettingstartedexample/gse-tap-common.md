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

    | Tab | Control | Value |
    |---|---|---|
    | **Resource** | **RIO Device** | [System Specific] |
    | **Resource** | **Bitfile Path** | [Refer to Bitfile Path in the PXIe-148X Acquisition GSE Help](../../reference/gettingstartedexample/gse-acq-help.md#table-of-pxie-148x-acquisition-bitfiles) |
    | **Serial Channel** | **Configuration Script** | Loopback Script - [Refer to Acquisition Scripts in the PXIe-148X Configuration Scripts User Guide](../../reference/gettingstartedexample/config-scripts-user-guide.md#acquisition-scripts) |

7. Run the Acquisition Example VI and wait for the **Acquisition In Progress** indicator to illuminate.

8. On the Tap Example VI, click **Sensor Setup Complete** to start the tap acquisition.

9. On the Generation Example VI, click **Serializer Setup Complete** to start the generation and wait for the generation to complete.

10. Select the First Display Channel tab on the Generation, Tap and Acquisition Examples and verify that images from the generated TDMS file are displayed on these tabs. The images should look identical.

![Compare First Display Channel Tabs](../../images/PXIe-148x-Gen-Tap-Acq-display.png)

10. Click the **Stop Acquisition** button on both the Tap and Acquisition Example VIs to stop the tap acquisition and stop the VIs.

## Logging and Displaying I2C Timestamps
This tutorial shows how to acquire and view I2C timestamps on the PXIe-148X interface module across a tap bridge using the tap and acquisition getting started examples.  

> Note: Timestamps are relative to a time immediately after the FPGA bitfile is downloaded and run, not the start of the acquisition. This allows capturing I2C and GPIO timestamps during configuration before the acquisition starts.

1. Set the following controls on the Tap Example VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Setting | Value |
    |---|---|---|
    | **Resource** | **RIO Device** | [System Specific] |
    | **Resource** | **Bitfile Path** | [Refer to Bitfile Path in the PXIe-148X Tap GSE Help](../../reference/gettingstartedexample/gse-tap-help.md#table-of-pxie-148x-tap-bitfiles) |
    | **Resource** | **Log I2C to Disk** | Enabled |
    | **Serial Channel** | **Deserializer (Input) Configuration Script** | [Refer to Deserializer Tap Scripts in the PXIe-148X Tap GSE Help](../../reference/gettingstartedexample/gse-tap-help.md#table-of-pxie-148x-deserializer-tap-scripts) |
    | **Serial Channel** | **Serializer (Output) Configuration Script** | [Refer to Serializer Tap Scripts in the PXIe-148X Tap GSE Help](../../reference/gettingstartedexample/gse-tap-help.md#table-of-pxie-148x-serializer-tap-scripts) |
    | **Board** | **Power Over Coax Source** | Internal |

2.  Select the **I2C** tab and make the following modifications.

    > The I2C tab only has one control, the I2C **timestamp filter**. This filter contains an array of timestamp IDs. **User24** represents the I2C traffic on serial channel 0 (SI0), **User25** represents the I2C traffic on serial channel 1 (SI1), and so on.
    - Set the **timestamp filter** array to contain only the **User24** timestamp ID. This will let you see the I2C traffic on the SI0 and SO0 channel pair. If you run the VI with a configuration script selected, you will see configuration traffic in the I2C Data Output tab after the VI has stopped.

    ![Configuration Settings I2C Tab](../../images/PXIe-148X-Acq-I2CTab-FiniteAcq.png)

3.  On the Acquisition Example VI set the following controls and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | **Resource** | **RIO Device** | [System Specific] |
    | **Resource** | **Bitfile Path** | [Refer to Bitfile Path in the PXIe-148X Acquisition GSE Help](../../reference/gettingstartedexample/gse-acq-help.md#table-of-pxie-148x-acquisition-bitfiles) |
    | **Resource** | **Log I2C to Disk** | Enabled |
    | **Serial Channel** | **Configuration Script** | [Refer to Configuration Script in the PXIe-148X Acquisition GSE Help](../../reference/gettingstartedexample/gse-acq-help.md#table-of-pxie-148x-acquisition-scripts) |
    | **Acquisition** | **Continuous Acquisition** | Disabled |

4.  On the Acquisition Example VI, select the **I2C** tab and make the following modifications.

    - Set the **timestamp filter** array to contain only the **User24** timestamp ID.

    ![Configuration Settings I2C Tab](../../images/PXIe-148X-Acq-I2CTab-FiniteAcq.png)

5.  Run the Tap Example VI and wait for the **Waiting for Sensor Setup** indicator to illuminate then click **Sensor Setup Complete**.
6.  Run the Acquisition Example VI and wait for the acquisition to complete.
7.  Click the **Stop Acquisition** button on both the Tap Example VI.
8.  Select the **I2C Timestamps** tab to view I2C timestamp data on both the Tap and Acquisition Example VIs.
    - View the displayed I2C timestamp data in the **I2C Timestamps** table. The **I2C Timestamps** table displays I2C timestamp information for all I2C traffic. I2C timestamps begin logging immediately after the FPGA bitfile is downloaded and include timestamp data prior to the start of the LLP packet data acquisition (i.e. I2C traffic from the configuration script).
    - The first set of timestamps in the displayed will correspond to the script defined by the **Deserializer (Input) Configuration Script** control. The next set of timestamps displayed will correspond to the script defined by the **Serializer (Output) Configuration Script** control. The last set of timestamps will correspond to the script defined by the **Configuration Script** control on the Acquisition Example VI. 
    - Note that the tap scripts change the addresses for the serializer and deserializer in the tap channel pair, which prevents an address conflict when running the acquisition script. As a result, the acquisition configuration script I2C commands sent to the default deserializer address result in an ACK response from the acq module and a NACK response from the tap module, while the I2C commands sent to the camera address are passed though the tap bridge to the camera and result in an ACK response on both the tap and acq modules.

        > Note: To display I2C timestamp data, **Log I2C to Disk** must be enabled and desired timestamp IDs must be added to the **timestamp filter** array. The I2C timestamp data displayed is read from the User_Timestamps.tdms file and filtered to display only timestamp IDs included in the timestamp filer array. The **I2C Timestamps** display is updated after the acquisitions completes.

    ![I2C Timestamps Data](../../images/PXIe-148X-Tap-I2CTimestamps.png)

## Setting FPGA Display Parameters
You can use the **FPGA Display Parameters** to change the images displayed during tap acquisition. Follow the procedure specified in the [Performing a Simple Continuous Tap Acquisition](../../tutorials/gettingstartedexample/gse-tap-basic.md#performing-a-simple-continuous-tap-acquisition) tutorial for configuring and running the Tap and Acquisition Example VIs for all tutorials in this section.

### Displaying Acquired Images
The logic for FPGA display parameters is shared for all PXIe-148X getting started examples. Therefore, you can use the acquisition tutorial for this objective, with a few minor differences.

1. Complete the steps in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#displaying-acquired-images) with the following differences.
    - Replace all references to <font face = "courier new">SI0</font> with **input** <font face = "courier new">SI0</font> and **output** <font face = "courier new">SO0</font> for the Tap Example VI.
    - Follow the procedure specified in the [Performing a Simple Continuous Tap Acquisition](../../tutorials/gettingstartedexample/gse-tap-basic.md#performing-a-simple-continuous-tap-acquisition) for configuring and running the Tap and Acquisition Example VIs.

### Reducing System Bandwidth Usage
The logic for FPGA display parameters is shared for all PXIe-148X getting started examples. Therefore, you can use the acquisition tutorial for this objective, with a few minor differences.

1. Complete the steps in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#displaying-acquired-images) with the following differences.
    - Replace all references to <font face = "courier new">SI0</font> with **input** <font face = "courier new">SI0</font> and **output** <font face = "courier new">SO0</font> for the Tap Example VI.
    - Follow the procedure specified in the [Performing a Simple Continuous Tap Acquisition](../../tutorials/gettingstartedexample/gse-tap-basic.md#performing-a-simple-continuous-tap-acquisition) for configuring and running the Tap and Acquisition Example VIs.


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