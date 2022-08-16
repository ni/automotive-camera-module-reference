# PXIe-148X Getting Started Example - Common Generation Tutorials
{: .no_toc }

** IN WORK - DOCUMENTATION IS INCOMPLETE AND IN ACTIVE DEVELOPMENT **

This document covers a range of common scenarios using the PXIe-148X Generation Getting Started Example (GSE) to help you understand LLP generation, I2C and GPIO timestamping, and common configuration options.

> Note: This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be appliable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Prerequisites

## Generation and Acquisition Topics

### Performing a Simple Generation and Acquisition

### Generating and Displaying I2C Timestamps

## Generating and Filtering LLP Packets

## Playing Back Previously Acquired Data

## Setting FPGA Display Parameters

This tutorial shows you how to configure the **FPGA Display Parameters** to change the images displayed during generation.

1. Set the following controls on the Generation Example GSE VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | Resource | RIO Device | [System Specific] |
    | Resource | Bitfile Path | [Refer to Bitfile Path in the PXIe-148X Generation GSE Help](../../reference/gettingstartedexample/gse-gen-help.md#table-of-pxie-148x-generation-bitfiles) |
    | Resource | TDMS File Directory| A directory path containing a file named <font face = "courier new">SI0_LLP_Packets.tdms</font> |
    
    > Note: The **TDMS File Directory** is a folder selection and the browse dialog shows folders only, not file names. 
    >
    > Use the default **TDMS File Directory** value if generating from a [TDMS file created with the Create CSI-2 Packet TDMS Files utility's default settings](gse-gen-basic.md#create-tdms-files-for-generation).

### Displaying Generated Images

1. Follow the procedure defined in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#displaying-acquired-images) with the following modifications.
    - Replace all references to <font face = "courier new">SI0</font> with <font face = "courier new">SO0</font>.
    - When running the Generation Example VI, wait for the **Waiting for Serializer Setup** indicator to illuminate then click **Serializer Setup Complete**.

    > The logic for FPGA display parameters is shared for all PXIe-148X getting started examples.

### Reducing System Bandwidth Usage

1. Follow the procedure defined in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#reducing-system-bandwidth-usage) with the following modifications.
    - When running the Generation Example VI, wait for the **Waiting for Serializer Setup** indicator to illuminate then click **Serializer Setup Complete**.

    > The logic for FPGA display parameters is shared for all PXIe-148X getting started examples.

## Setting RAW Display Parameters

This tutorial shows you how to configure the **RAW Display Parameters** to change the interpretation of images being displayed during generation.

1. Set the following controls on the Generation Example GSE VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | Resource | RIO Device | [System Specific] |
    | Resource | Bitfile Path | [Refer to Bitfile Path in the PXIe-148X Generation GSE Help](../../reference/gettingstartedexample/gse-gen-help.md#table-of-pxie-148x-generation-bitfiles) |
    | Resource | TDMS File Directory| A directory path containing a file named <font face = "courier new">SI0_LLP_Packets.tdms</font> |

    > Note: The **TDMS File Directory** is a folder selection and the browse dialog shows folders only, not file names. 
    >
    > Use the default **TDMS File Directory** value if generating from a [TDMS file created with the Create CSI-2 Packet TDMS Files utility's default settings](gse-gen-basic.md#create-tdms-files-for-generation).

2. Follow the procedure defined in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#setting-raw-display-parameters) with the following modifications.
    - Replace all references to <font face = "courier new">SI0</font> with <font face = "courier new">SO0</font>.
    - When running the Generation Example VI, wait for the **Waiting for Serializer Setup** indicator to illuminate then click **Serializer Setup Complete**.

    > Note: The logic for setting RAW display parameters is shared for all PXIe-148X getting started examples, but results will not match the procedure descriptions exactly if using a TDMS file created with the Create CSI-2 Packet TDMS Files utility.

## Setting Serial Channel Configurations

This tutorial shows you how to configure the **Serial Channel** tab to generate and display images on multiple serial output channels.

### Creating TDMS Files to Generate

1. Open the Create CSI-2 Packet TDMS Files utility in the LabVIEW project.

2. Run the VI with all default settings to generate a TDMS file for channel SI0 containing 10 frames at 1920x1080 resolution.

3. Make the following modifications on the Create CSI-2 Packet TDMS Files utility front panel.
    - Set the value at index zero of the **Serial Input Channels** array to <font face = "courier new">SI1</font>.
    - Make the following modifications to the **Frame Data Configuration** control.
        - Set the **horizontal resolution** control value to <font face = "courier new">2880</font>.
        - Set the **vertical resolution** control value to <font face = "courier new">1280</font>.

4.  Run the VI to generate a TDMS file for channel SI1 containing 10 frames at 2880x1280 resolution.

### Generating and Displaying Multiple Images

1. Set the following controls on the Generation Example GSE VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | Resource | RIO Device | [System Specific] |
    | Resource | Bitfile Path | [Refer to Bitfile Path in the PXIe-148X Generation GSE Help](../../reference/gettingstartedexample/gse-gen-help.md#table-of-pxie-148x-generation-bitfiles) |

2. Select the **Serial Channel** tab and make the following modifications.
    - Select index <font face = "courier new">0</font> of the **Channel Configurations** array and make the following modifications.
        - Select **Grayscale** on the **Interpretation** control in the **RAW Display Parameters** cluster.
    - Select index <font face = "courier new">1</font> of the **Channel Configurations** array and make the following modifications.
        - Set the **Serial Channel** control value to <font face = "courier new">SO1</font>.
        - Select **Grayscale** on the **Interpretation** control in the **RAW Display Parameters** cluster.

3. Run the VI.
    > Once the generation completes, notice the **Number of Generated Packets** indicator array shows a value of <font face = "courier new">10820</font> at index zero and <font face = "courier new">12820</font> at index one, which equals the number of packets per frame (vertical resolution) times the number of frames generated for each channel. The indices in the **Number of Generated Packets** array correspond to the indices in the **Channel Configurations** array.

4. Click the **First Display Channel** tab to view the last frame displayed for SO0.
    > Note that the image resolution matches the 1920x1080 resolution of the TDMS file created for SI0.

5. Click the **Second Display Channel** tab to view the last frame displayed for SO1.
    > Note that the image resolution matches the 2880x1280 resolution of the TDMS file created for SI1.

6. Select the **Serial Channel** tab and set the **Start Trigger Delay (s)** control value to <font face = "courier new">2</font>.

7. Run the VI and notice that there is now a 2 second delay before images are generated after clicking the **Serializer Setup Complete** button.


## Manually Reading and Writing to GPIO

### Routing GPIO Channels


## Related Documents
- [PXIe-148X Getting Started Example - Basic Generation Tutorial](./gse-gen-basic.md)
- [PXIe-148X Getting Started Example - Generation Help](../../reference/gettingstartedexample/gse-gen-help.md)
- [PXIe-148X Getting Started Example - Common Acquisition Tutorials](./gse-acq-common.md)
- [PXIe-148X Getting Started Example - Common Tap Tutorials](./gse-tap-common.md)
