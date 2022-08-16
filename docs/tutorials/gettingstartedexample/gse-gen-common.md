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


## Manually Reading and Writing to GPIO

### Routing GPIO Channels


## Related Documents
- [PXIe-148X Getting Started Example - Basic Generation Tutorial](./gse-gen-basic.md)
- [PXIe-148X Getting Started Example - Generation Help](../../reference/gettingstartedexample/gse-gen-help.md)
- [PXIe-148X Getting Started Example - Common Acquisition Tutorials](./gse-acq-common.md)
- [PXIe-148X Getting Started Example - Common Tap Tutorials](./gse-tap-common.md)
