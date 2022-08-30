# PXIe-148X Tutorial - Create CSI-2 Packet TDMS Files
{: .no_toc }

This tutorial provides greater detail on how to generate TDMS files using the Create CSI-2 Packet TDMS Files VI for different use cases. It provides additional background details on how the VI constructs LLP Packet timestamps based on the VI's control values. 

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Prerequisites 
This tutorial is written for users who understand how to perform a basic generation and a basic acquisition with PXIe-148X GMSL or FPD-Link interface modules. It is recommended to complete the [PXIe-148X Getting Started Example - Basic Acquisition Tutorial](gse-acq-basic.md) and [PXIe-148X Getting Started Example - Basic Generation Tutorial](gse-gen-basic.md) before attempting this tutorial.

## Create TDMS Files for Generation
> Note: For the purposes of this tutorial, all input control values not specified should be left as the default value.

1. Double click the Create CSI-2 Packet TDMS Files VI in the LabVIEW project you created in the Basic Generation tutorial.

    ![Open Create CSI-2 Packet TDMS Files VI](../../images/PXIe-148X-CreateTDMS-Project.png)

    > The opened front panel of the Create CSI-2 Packet TDMS Files Utility is similar to the figure below.

    ![Open Create CSI-2 Packet TDMS Files Front Panel](../../images/PXIe-148X-CreateTDMS-FrontPanel.png)

2.  Review the instructions on the VI front panel. For this tutorial the default settings is used to generate a TDMS file for channel SO0 containing 10 frames at 1920x1080 and 30fps .

    > Note: During the first run of the VI, the **TDMS File Directory** control automatically populates with a value pointing to a subfolder (\"TDMS Files\"). This subfolder is automatically created within the project folder to store any generated TDMS files.

3.  Run the VI to generate the TDMS file.

4.  Open Windows Explorer and navigate to <font face = "courier new">\<yourprojectdir\>\\Host\\Gen\\TDMS Files</font>. The newly created TDMS file has the prefix "SI0_" to indicate it is associated with the first channel 'SO0'. Although the prefix contains 'SI' suggesting 'serial input', when used with the Generation GSE, the TDMS file actually associates with serial output channel 'SO0'.

    ![Create CSI-2 Packet TDMS Files Explorer View](../../images/PXIe-148X-CreateTDMS-ExplorerView.png)

5. The newly created TDMS file is used in the following tutorial to perform a simple generation.
    

## Conceptual Overview
TODO: Give description of what the LLP packet data looks like (and show diagram). Give explanation of how the various controls can affect the timing relationship of the generated data.

## What's the max frame rate I can do for my frame size

## Using line sync packets
### Controlling timing with line start/end packets
### Controlling timing without line start/end packets

## Evenly spacing long packets to fill the entire time window for a frame

## Other Considerations and Tips
### Accidentally creating impossible configurations (packet timing errors)
### Generating overly large files
    
## TBD - DO WE NEED THESE?
### What's some realistic numbers I can use for 1486, 1487, 1 channel? 8 channels?
### What's the most data I can saturate the link with?
    
## Create CSI-2 Packet TDMS Files VI Help
### General
- **Serial Input Channels** - TODO
- **TDMS File Directory** - Path to the directory used to load TDMS data files. 
    > If left blank the TDMS File Directory is automatically populated with a path to a subfolder ("TDMS Files") within the getting started example root directory. TDMS data files include files for LLP packet data, I2C timestamps, and GPIO timestamps.
- **Number of Frames** - TODO
- **Actual Frame Rate (fps)** - TODO
- **Error Out** - Displays any error that occurred when running the VI.
    
### Frame Data Configuration
- **pixel data type** - TODO
- **horizontal resolution** - TODO
- **vertical resolution** - TODO
- **virtual channel** - TODO
- **include line sync packets** - TODO

### Frame Timing Configuration
- **desired frame rate (fps)** - TODO
- **actual link data rate (B/s)** - TODO
- **minimum delay between packets (cycles)** - TODO
- **line blanking (cycles)** - TODO

## Related Documents
- [PXIe-148X Getting Started Example - Basic Generation Tutorial](./gse-gen-basic.md)
- [PXIe-148X Getting Started Example - Common Generation Tutorials](./gse-gen-common.md)
- [PXIe-148X Getting Started Example - Generation Help](../../reference/gettingstartedexample/gse-gen-help.md) 
  