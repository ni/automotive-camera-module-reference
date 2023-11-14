# Production Test User Guide
This user guide highlights optimizations made to the PXIe-148X module software support to improve the user experience for production test development.

> Note: Software improvements referenced in this document are included with the NI-FlexRIO 24Q1 driver and later.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Using the Wait for PoC Safe to Disconnect Feature
Disconnecting and connecting coaxial cables to a PXIe-148X module with power-over-coax (PoC) enabled can result in damaged hardware. The **Wait for PoC safe to Disconnect** function waits and blocks until specified channels with power-over-coax are safe to disconnect to prevent accidental hardware damage. 

### FlexRIO LabVIEW Host API
The **Wait for PoC Safe to Disconnect** LabVIEW VI is located in the Automotive Vision sub-palette of the NI FlexRIO API palette.

![FlexRIO API Automotive Vision Palette](../images/FlexRIO-API-Automotive-Vision-Palette.png)

![Wait for PoC Safe to Disconnect VI](../images/Wait-for-PoC-Safe-to-Disconnect.png)

The **Channels** input parameter accepts a string input type or an array of strings input type. When using the string input type, channels are specified as a single channel such as "SI0", a range such as "SI0-SI3" or a list such as "SI0,SI3". When using the array of strings input type, each element of the array is a single channel such as "SI0", "SO2", etc.

The **Timeout (ms)** input parameter specifies a time limit in milliseconds to wait until safe to disconnect. A value of -1 indicates to wait indefinitely.

    > Warning: Waiting indefinitely causes the LabVIEW VI to hang if the discharge threshold for safe to disconnect is not met.

See [TODO - Link to new doc] for help in determining timeout values.

See the LabVIEW context help for complete function documentation.

### LabVIEW Block Diagram Example
The block diagram below provides and example of a production test sequence with the **Wait for PoC Safe to Disconnect** function included.

![Wait for PoC Safe to Disconnect](../images/Wait-For-PoC-Safe-To-Disconnect-Example.png)

### Timeout Errors
When a **Wait for PoC Safe to Disconnect** timeout error occurs, the error message provides a list of channels that timed out.

![Wait for PoC Safe to Disconnect Error](../images/Wait-For-PoC-Safe-To-Disconnect-Error.png)

## Optimizing Configuration Script Execution

### FlexRIO LabVIEW Host Utility VIs

## Using the SerDes Configuration Utility

### LabVIEW Block Diagram Example

## Parsing and Running Maxim Configuration Scripts in LabVIEW

### LabVIEW Block Diagram Example

## Creating a Custom Configuration Script Format