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

## Initial Software Setup

1.  First complete installation of software specified in the Getting Started Guide linked above.
2.  Use the NI Example Finder to create a default project for your specific interface module.
    - Open LabVIEW and click **Help -> Find Examples...**
    
    ![Open NI Example Finder](../../images/LabVIEW-Help-FindExamples.png)
    - In the NI Example Finder dialog, double-click **Hardware Input and Output -> FlexRIO -> Integrated IO -> Getting Started -> Getting Started FlexRIO Integrated IO.vi**
    ![NI Example Finder](../../images/NIExampleFinder-GSEFlexRIOWithIntegratedIO.png)
    - In the FlexRIO with Integrated IO Project Creator dialog set the **Project Name** to <font face = "courier new">PXIe-148X Tutorial</font> and make the **FlexRIO Integrated IO** selection for the desired PXIe-148X interface module then click **OK**.
    ![FlexRIO with Integrated IO Project Creator](../../images/FlexRIOWithIntegratedIOProjectCreator-1486-PXIe-148XTutorial.png)

## Create TDMS Files for Generation
> Note: For the purposes of this tutorial, all input control values not specified should be left as the default value.

1. Double click the Create CSI-2 Packet TDMS Files VI in the LabVIEW project.

    ![Open Create CSI-2 Packet TDMS Files VI](../../images/PXIe-148X-CreateTDMS-Project.png)

    > The opened front panel of the Create CSI-2 Packet TDMS Files Utility is similar to the figure below.

    ![Open Create CSI-2 Packet TDMS Files Front Panel](../../images/PXIe-148X-CreateTDMS-FrontPanel.png)

2.  Review the instructions on the VI front panel. For this tutorial the default settings is used to generate a TDMS file for channel SO0 containing 10 frames at 1920x1080 and 30fps .

    > Note: During the first run of the VI, the **TDMS File Directory** control automatically populates with a value pointing to a subfolder (\"TDMS Files\"). This subfolder is automatically created within the project folder to store any generated TDMS files.

3.  Run the VI to generate the TDMS file.

4.  Open Windows Explorer and navigate to <font face = "courier new">\<yourprojectdir\>\\Host\\Gen\\TDMS Files</font>. The newly created TDMS file has the prefix "SI0_" to indicate it is associated with the first channel 'SO0'. Although the prefix contains 'SI' suggesting 'serial input', when used with the Generation GSE, the TDMS file actually associates with serial output channel 'SO0'.

    ![Create CSI-2 Packet TDMS Files Explorer View](../../images/PXIe-148X-CreateTDMS-ExplorerView.png)

5. The newly created TDMS file is used in the following tutorial to perform a simple generation.


## Related Documents
- [PXIe-148X Getting Started Example - Basic Generation Tutorial](./gse-gen-basic.md)
- [PXIe-148X Getting Started Example - Common Generation Tutorials](./gse-gen-common.md)
- [PXIe-148X Getting Started Example - Generation Help](../../reference/gettingstartedexample/gse-gen-help.md) 
  
